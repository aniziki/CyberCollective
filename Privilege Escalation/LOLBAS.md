# [# Living Off The Land Binaries, Scripts and Libraries](https://lolbas-project.github.io/)

The goal of the LOLBAS project is to document every binary, script, and library that can be used for Living Off The Land techniques.

**Criteria**
A LOLBin/Lib/Script must:
- Be a Microsoft-signed file, either native to the OS or downloaded from Microsoft.
- Have extra "unexpected" functionality. It is not interesting to document intended use cases.
	- Exceptions are application whitelisting bypasses
- Have functionality that would be useful to an APT or red team.

Interesting functionality can include:
- Executing Code
	- Arbitrary Code Execution
	- Pass-through execution of other programs (unsigned) or scritps (via a LOLbin)
- Compiling Code
- File operations
	- Downloading
	- Upload
	- Copy
- Persistence
	- Pass-through persistence utilizing existing LOLBin
	- Persistence (e.g. hide data in ADS, execute at logon)
- UAC Bypass 
- Credential Theft
- Dumping process memory 
- Surveillance (e.g. keylogger, network trace)
- Log evasion/modification
- DLL side-loading/hijacking without being relocated elsewhere in the filesystem.

Source:
https://lolbas-project.github.io/
https://github.com/LOLBAS-Project/LOLBAS

#windows