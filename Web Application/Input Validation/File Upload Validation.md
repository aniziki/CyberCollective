**Upload Verification**
- Use input validation to ensure the uploaded filename uses an expected extension type.
- Ensure the uploaded file is not larger than a defined maximum file size.
- If the website supports ZIP file upload, do a validation check before unzip the file. The check includes the target path, level of compress, estimated unzip size. 
	- [[Zip Slip]]

**Upload Storage**
- Use a new filename to store the file on the OS. Do not use any user controlled text for this filename or for the temporary filename.
- When the file is uploaded to web, it's suggested to rename the file on storage. For example, the uploaded filename is test.JPG, rename it to JAI2432sdklsdfg.JPG with a random filename. The purpose of doing it to prevent the risks of direct file access and abiguous filename to evade the filter, such as `test.jpg; .asp or /../../../../../../test.jpg`
- Uploaded files should be analyzed for malicious content (anti-malware, static analysis, etc.).
- The file path should not be able to specify by client side. It's decided by the server side.

**Public Serving of Uploaded Content**
- Ensure uploaded images are served with the correct content-type (e.g. image/jpeg, application/x-xpinstall)

**Beware of "Special" Files**
The upload feature should be using an allow-list approach to only allow specific file types and extensions. However, it is important to be aware of the following file types that, if allowed, could result in security vulnerabilities:
- **crossdomain.xml / clientaccesspolicy.xml**: allows cross-domain data loading in Flash, Java and Silverlight. If permitted on sites with authentication this can permit cross-domain data theft and CSRF attacks. Note this can get pretty complicated depending on the specific plugin version in question, so it's best to just prohibit files named "crossdomain.xml" or "clientaccesspolicy.xml".

Source: https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#file-upload-validation