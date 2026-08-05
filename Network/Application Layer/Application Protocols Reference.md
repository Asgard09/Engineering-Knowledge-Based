# SSH - Secure Shell (Port 22)
## SSH providing
- Encrypted remote shell access: Connect remotely and run command like you’re sitting in front of the machine.
- Secure file transfer: Copying files between two computer over an encrypt connect. 
- Tunneling: creates an encrypted “pipe” that forward traffic from port on the local to service in the remote server, even if it’s inaccessible.
## How it’s work?
The laptop already have pair private key and public key (by run ssh-keygen), when you connect the server will check if it has your laptop public key, if yes send the challenge, your laptop need to sign that challenge by your private key and send the signature back. The server will verify by public key. If success the connection is establish and your laptop doesn’t need type password anymore