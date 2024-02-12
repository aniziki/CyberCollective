Previously known as CrackMapExec, verbiage will stay same for the time being.
## **Description:**
[CrackMapExec](https://attack.mitre.org/software/S0488), or CME, is a post-exploitation tool developed in Python and designed for penetration testing against networks. [CrackMapExec](https://attack.mitre.org/software/S0488) collects Active Directory information to conduct lateral movement through targeted networks

Tool usually preinstalled into Kali Linux.

**[[SMB]] :
- [Enumerate Hosts:](https://mpgn.gitbook.io/crackmapexec/smb-protocol/enumeration/untitled) CME can be utilized in the beginning of an assessment to generate a target list for a specified network range with SMB signing disabled. 
- [Enumerate Null Sessions:](https://mpgn.gitbook.io/crackmapexec/smb-protocol/enumeration/enumerate-null-sessions) Checking if **Null Session** is enabled on the network, can be very useful on a Domain Controller to enumerate users, groups, password policy etc
- [Enumerate Anonymous Logon:](https://mpgn.gitbook.io/crackmapexec/smb-protocol/enumeration/enumerate-anonymous-logon) Using a random username and password you can check if the target accepts anonymous logon
- [Enumerate Shares and Access: ](https://mpgn.gitbook.io/crackmapexec/smb-protocol/enumeration/enumerate-shares-and-access) Enumerate permissions on all shares
- [Password Spraying: ](https://mpgn.gitbook.io/crackmapexec/smb-protocol/password-spraying) Password spraying is an attack that attempts to access a large number of accounts (usernames) with a few commonly used passwords.


Github: 
https://github.com/Pennyw0rth/NetExec
https://github.com/byt3bl33d3r/CrackMapExec

Source:
https://mpgn.gitbook.io/crackmapexec/
https://attack.mitre.org/software/S0488/
#tools #cme #python #smb
