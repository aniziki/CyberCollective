## **Description:**
The Server Message Block (SMB) Protocol is a network file sharing protocol, and as implemented in Microsoft Windows is known as Microsoft SMB Protocol. The set of message packets that defines a particular version of the protocol is called a dialect. The Common Internet File System (CIFS) Protocol is a dialect of SMB. Both SMB and CIFS are also available on VMS, several versions of Unix, and other operating systems.

Common Misconfigurations: 
- **SMB Signing:** SMB signing essentially signs each packet with a digital signature so the client and server can confirm where they originated from as well as the authenticity of the call. When SMB signing is enabled, if an attacker attempts to steal an SMB session they would be unable to modify the packets allowing them to steal the session.

- [SMB Authentication Protocols](https://library.netapp.com/ecmdocs/ECMP1366834/html/GUID-F0BACF71-B793-4037-8CB9-2AB442584176.html)
- [LM, NTLM, NTLMv2 Overview](https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4)

#active-directory #windows #hashing #smb 