Windows Challenge/Response (NTLM) is the authentication protocol used on networks that include systems running the Windows operating system and on stand-alone systems.

The Microsoft [_Kerberos_](https://learn.microsoft.com/en-us/windows/win32/secgloss/k-gly) [_security package_](https://learn.microsoft.com/en-us/windows/win32/secgloss/s-gly) adds greater security than NTLM to systems on a network. Although Microsoft _Kerberos_ is the protocol of choice, NTLM is still supported. NTLM must also be used for logon authentication on stand-alone systems. For more information about Kerberos, see [Microsoft Kerberos](https://learn.microsoft.com/en-us/windows/win32/secauthn/microsoft-kerberos).

NTLM credentials are based on data obtained during the interactive logon process and consist of a domain name, a user name, and a one-way [_hash_](https://learn.microsoft.com/en-us/windows/win32/secgloss/h-gly) of the user's password. NTLM uses an encrypted challenge/response protocol to authenticate a user without sending the user's password over the wire. Instead, the system requesting authentication must perform a calculation that proves it has access to the secured NTLM credentials.

Interactive NTLM authentication over a network typically involves two systems: a client system, where the user is requesting authentication, and a domain controller, where information related to the user's password is kept. Noninteractive authentication, which may be required to permit an already logged-on user to access a resource such as a server application, typically involves three systems: a client, a server, and a domain controller that does the authentication calculations on behalf of the server.

The following steps present an outline of NTLM noninteractive authentication. The first step provides the user's NTLM credentials and occurs only as part of the interactive authentication (logon) process.

1. (Interactive authentication only) A user accesses a client computer and provides a domain name, user name, and password. The client computes a cryptographic [_hash_](https://learn.microsoft.com/en-us/windows/win32/secgloss/h-gly) of the password and discards the actual password.
2. The client sends the user name to the server (in [_plaintext_](https://learn.microsoft.com/en-us/windows/win32/secgloss/p-gly)).
3. The server generates a 8-byte random number, called a _challenge_ or [_nonce_](https://learn.microsoft.com/en-us/windows/win32/secgloss/n-gly), and sends it to the client.
4. The client encrypts this challenge with the hash of the user's password and returns the result to the server. This is called the _response_.
5. The server sends the following three items to the domain controller:
    - User name
    - Challenge sent to the client
    - Response received from the client
6. The domain controller uses the user name to retrieve the hash of the user's password from the Security Account Manager database. It uses this password hash to encrypt the challenge.
7. The domain controller compares the encrypted challenge it computed (in step 6) to the response computed by the client (in step 4). If they are identical, authentication is successful.

Source:
https://learn.microsoft.com/en-us/windows/win32/secauthn/microsoft-ntlm