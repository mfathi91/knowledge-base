## HTTPS
HTTPS provides encrypted communication + avoid man-in-the-middle. HTTPS is simply plain HTTP on a TLS layer (TLS is between TCP and HTTP).

What is a secure TCP connection? First we need to clarify how the server itself becomes "certified". It all starts with a domain name for the server. Imagine on the server we have
two applications app1.myserver.com and app2.myserver.com. Now each of these applications listen on a different port (not 443, although both of them work with https).

It is a common practice to have one program/HTTP server running on the server (the machine, host, etc.) and managing all the HTTPS parts: receiving the encrypted HTTPS requests,
sending the decrypted HTTP requests to the actual HTTP application running in the same server (the FastAPI application, in this case), take the HTTP response from the application,
encrypt it using the appropriate HTTPS certificate and sending it back to the client using HTTPS. This server is often called a TLS Termination Proxy. Like Caddy or Nginx.
The TLS Termination Proxy listens on 443 port and handles the requests for different apps on the server. The TLS Termination Proxy "accquires" a certificate via communicating
with the let's encrypt servers. This is called ACME protocol. Example:
- Proxy: I want a certification for app1.myserver.com
- Let's encrypt: prove that you own myserver.com with hosting this random file that I send you

After that Let's encrypt issues a certificate. Then for the requests of app1.myserver.com, first a secure TCP connection via public and private keys happen, and then
the applications simply communicate without really knowing that they are communicating with HTTPS.

<img width="1413" height="1464" alt="image" src="https://github.com/user-attachments/assets/5a2d5f0c-1aee-49ea-b403-d1b05e8491a7" />

