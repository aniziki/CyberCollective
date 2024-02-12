## **Summary**: 
A padding oracle is a function of an application which decrypts encrypted data provided by the client, e.g., internal session state stored on the client, and leaks the state of the validity of the padding after decryption. The existence of a padding oracle allows an attacker to decrypt encrypted data and encrypt arbitrary data without knowledge of the key used for these cryptographic operations. This can lead to leakage of sensitive data or to privilege escalation vulnerabilities, if integrity of the encrypted data is assumed by the application.

Block ciphers encrypt data only in blocks of certain sizes. Block sizes used by common ciphers are 8 and 16 bytes. Data where the size doesn't match a multiple of the block size of the used cipher has to be padded in a specific manner so the decryptor is able to strip the padding. A commonly used padding scheme is PKCS#7. It fills the remaining bytes with the value of the padding length.

### Example 1:
If the padding has the length of 5 bytes, the byte value `0x05` is repeated five times after the plain text.
An error condition is present if the padding doesn't match the syntax of the used padding scheme. A padding oracle is present if an application leaks this specific padding error condition for encrypted data provided by the client. This can happen by exposing exceptions (e.g. `BadPaddingException` in Java) directly, by subtle differences in the responses sent to the client or by another side-channel like timing behavior. 

Certain modes of operation of cryptography allow bit-flipping attacks, where flipping of a bit in the cipher text causes that the bit is also flipped in the plain text. Flipping a bit in the Nth block of CBC encrypted data causes that the same bit in the (N+1)th block is flipped in the decrypted data. The Nth block of the decrypted cipher text is garbaged by this manipulation.

The padding oracle attack also enables an attacker to decrypt arbitrary plain texts without knowledge of the encryption key and used cipher by sending skillful manipulated cipher texts to the padding oracle and observing of the results returned by it. This causes loss of confidentiality of the encrypted data. E.g. in the case of session data stored on the client-side the attacker can gain information about the internal state and structure of the application. 

A padding oracle attack also enables an attacker to encrypt arbitrary plain texts without knowledge of the used key and cipher. If the application assumes that integrity and authenticity of the decrypted data is given, an attacker could be able to manipulate internal session state and possibly gain higher privileges.

### Test Objectives
- Identify encrypted messages that rely on padding.
- Attempt to break the padding of the encrypted messages and analyze the returned error messages for further analysis.


Source:
https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle