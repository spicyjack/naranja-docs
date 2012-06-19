There's a Perl script inside of OpenSSL called 'CA.pl' that will do the work of creating server/client/certificate authority 'certificate requests' for you. Here's how you use it...

Generate a simple self-signed SSL certificate

openssl req -new -x509 -nodes -out server.crt -keyout server.key
Become your own SSL Certificate Authority

Need to sign SSL certificates, but don't want to pay? Become your own Certiciate Authority (CA). The good news is that it's free, the bad news is that you'll have to install your CA certificate onto client browsers in order to get them to stop bitching about not trusting the CA.

$ CA.pl -newca #(You'll be prompted for a new CA key password)

# You can see the details of this RSA private key via the command
$ openssl rsa -noout -text -in /root/CA/private/cakey.pem

# And you can create a decrypted PEM version (not recommended) of this
# private key via:
$ openssl rsa -in ca.key -out ca.key.unsecure
Generate a self-signed SSL server certificate

The below steps assume you have already created your CA certificate using the steps outlined above.

# Create a new Certificate Request; You'll be prompted for a password
# and then the certificate info for this new SSL key/certificate pair
$ perl CA.pl -newreq 

# You can see the details of this RSA private key via the command:
$ openssl rsa -noout -text -in newkey.pem
 
# And you could create a decrypted PEM version (not recommended) of this RSA
# private key via:
$ openssl rsa -in newkey.pem -out newkey.pem.unsecure

# sign the CSR with CA.pl
$ perl CA.pl -sign #(You'll be prompted for the CA password)

# View the signed certificate
$ openssl x509 -noout -text -in newcert.pem
