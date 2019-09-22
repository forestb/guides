- [Introduction](#introduction)
- [Manual Certificate Generation](#manual-certificate-generation)
- [Credits](#credits)

# Introduction
The goal was to secure a `cname` that points to an Azure web app using Let's Encrypt - a tool/root CA used to generate free SSL certificates. This is something other cloud hosting providers (such as Heroku) do automatically.

# Manual Certificate Generation
Note: Manually generating the SSL certificate is not recommended. It's also difficult to find documentation for completing this, which made me want to understand it even more.

Step 1: Clone the Let's Encrypt repository in some Linux environment

`git clone https://github.com/letsencrypt/letsencrypt`

Step 2: Generate the certificate manually.   

`sudo -H ./letsencrypt-auto --manual certonly --manual -d <domain>`

Step 3: Upload the (to be specified) file and complete the HTTP-01 challenge

*Let's Encrypt will direct you to save some file contents to some file, and ensure it's available at some path. Create the file, upload the file, and ensure it's available from the path specified.*

Step 4: Locate the generated `.pem` files.

*I was using the WSL to execute Let's Encrypt. The certificate files will be saved at `/etc/letsencrypt/live/` which, for the WSL, can be found at `%LOCALAPPDATA%\Packages\<Debian, Ubuntu, etc>`*

Step 5: Convert `.pem` to `.pfx` for Azure

`openssl pkcs12 -export -out certificate.pfx -inkey privkey.pem -in cert.pem`

Breaking down the command:
- `openssl` – the command for executing OpenSSL
- `pkcs12` – the file utility for PKCS#12 files in OpenSSL
- `export -out certificate.pfx` – export and save the PFX file as certificate.pfx
- `inkey privkey.pem` – use the private key file privateKey.key as the private key to combine with the certificate.
- `in cert.pem` – use certificate.crt as the certificate the private key will be combined with.

# Credits

- https://www.linode.com/docs/security/ssl/install-lets-encrypt-to-create-ssl-certificates/