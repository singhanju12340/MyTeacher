---
Creation Time: Monday, December 2nd 2024
Modified Time: Monday, December 2nd 2024
---
#### JSON Web Token

retdfsdughajks . cfgvhbjnklm . dtfyguhbjnkml

EncodingAlgo **.** Payload . Signature


Steps:
1. Client send auth creds to Server
2. Server returns JWT Auth Token
3. Client stores it at local storage or cookie
4. Client sends this for next subsequent request

_How it works_

Client encode payload with private key
Server has the private key
JWT token is send, by decoding it server can extract the payload and use private key to verify the signature.
This ensure, payload data is not modified in transmission
JWT should be send via secure connection, because JWT contains payload.

JWT ensure data integrity, it do not secure data in transmission, as it do not encrypt data.
Implementaing token expiration can prevent token misuse.
Can utilise JWT refresh token, to avoid long lived token



`While the payload of a JWT is base64url-encoded and can be decoded easily, the security of JWT-based authentication relies on the token's signature, secure transmission, proper handling of sensitive data, and adherence to best practices. By following these guidelines, JWTs can be effectively and securely used for authentication and authorization purposes.



**Workflow of Refresh Tokens:**

1. **User Authentication:** Upon successful login, the server issues both an access token and a refresh token to the client.
    
2. **Accessing Resources:** The client uses the access token to request protected resources.
    
3. **Token Expiration:** When the access token expires, the client sends the refresh token to the server to obtain a new access token.
    
4. **Token Renewal:** The server verifies the refresh token's validity. If valid, it issues a new access token (and optionally a new refresh token) to the client.
    
5. **Continuation:** The client continues to access protected resources using the new access token.



