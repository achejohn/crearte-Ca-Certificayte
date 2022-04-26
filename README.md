# crearte-Ca-Certificayte
Create and sign certs with OpenSSL
I talk about how to create and sign certs with OpenSSL (and convert them to PFX for Windows). This came out of my complete inability to ever remember how to do any of this, so I created a cheat sheet.

Cheat sheet below:

Self-signed

Generate a new private key:
openssl genrsa -out blah.key 4096

Create a self-signed cert from the private key:
openssl req -x509 -key blah.key -out blah.pub -days 365

Verify the certificate:
openssl x509 -noout -text -in blah.pub

Do all of the above in a single command:
openssl req -x509 -newkey rsa:4096 -keyout ca.key -out ca.crt -days 365 -nodes

Convert to pfx:
openssl pkcs12 -export -in ca.crt -inkey ca.key -out ca.pfx

Using a key to sign another:

Generate a new private key

Create a CSR from the private key:
openssl req -new -key blah.key -out signable.csr

Do both in the same step:
openssl req -newkey rsa:4096 -out signable.csr -keyout signable.key -nodes

Sign the CSR with the CA cert:
openssl x509 -req -in signable.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out signable.crt -days 365

Export as PFX:
openssl pkcs12 -export -in signable.crt -inkey signable.key -out signable.pfx
