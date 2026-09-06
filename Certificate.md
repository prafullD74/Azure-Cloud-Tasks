# What is Certificate?

- A digital certificate is an electronic credential—similar to a digital passport or ID card—used to prove the identity of an entity (like a website, server, user, or device) and enable encrypted communication over a network.
- [To get a certificate](https://www.digicert.com/what-is-an-ssl-certificate), you must [create a Certificate Signing Request (CSR)](https://knowledge.digicert.com/general-information/how-to-create-a-csr?utm_medium=organic&utm_source=google&referrer=https://www.google.com/) on your server. This process creates a private key and public key on your server. The CSR data file that you send to the SSL Certificate issuer (called a [Certificate Authority or CA](https://www.digicert.com/blog/what-is-a-certificate-authority)) contains the public key. The CA uses the CSR data file to create a data structure to match your private key without compromising the key itself. The CA never sees the private key.
- A certificate authority (CA) is a trusted organization that verifies identities and issues digital certificates to secure websites, software, email, and connected devices.
- By validating identities and issuing trusted certificates, CAs help establish secure connections between browsers, web servers, applications, and devices. It's how sensitive information like passwords, payment details, and personal data stays protected while in transit.

## **Core Components of a Certificate**

* **Subject:** Identifies who owns the certificate (e.g., domain name like `contoso.com`, organization name, or user).
* **Public Key:** The public half of an asymmetric cryptographic key pair used to encrypt data or verify signatures.
* **Issuer:** The Certificate Authority (CA) that generated, verified, and digitally signed the certificate.
* **Digital Signature:** A cryptographic signature created by the CA using its private key to prove the certificate is authentic and hasn't been tampered with.
* **Validity Period:** The specific start ("Not Before") and expiration ("Not After") dates defining when the certificate is active.
* **Serial Number:** A unique alphanumeric identifier assigned by the CA to track and manage the certificate.
* **Subject Alternative Name (SAN):** An extension specifying additional hostnames, subdomains, or IP addresses secured by the single certificate.
* **Key Usage / Extended Key Usage (EKU):** Flags defining allowed purposes for the certificate (e.g., Server Authentication, Client Authentication, Code Signing).

### [SSL/TLS Certificate](https://aws.amazon.com/what-is/ssl-certificate/)

- An SSL/TLS certificate is a digital object that allows systems to verify the identity & subsequently establish an encrypted network connection to another system using the Secure Sockets Layer/Transport Layer Security (SSL/TLS) protocol.
- SSL/TLS certificates thus act as digital identity cards to secure network communications, establish the identity of websites over the Internet as well as resources on private networks.
- Businesses install SSL/TLS certificates on web servers to create SSL/TLS-secured websites. SSL/TLS certificates establish trust among website users.
- SSL/TLS encryption builds on this concept by using public key cryptography, with two different keys to encrypt and decrypt a message. 
- The **public key** is a cryptographic key that the web server gives the browser in the SSL/TLS certificate. The browser uses the key to encrypt the information before sending it to the web server.
- Only the web server has the private key. A file that is encrypted by the private key can only be decrypted by the public key, and vice versa.
- A **certificate authority (CA)** is an organization that sells SSL/TLS certificates to web owners, web hosting companies, or businesses. The CA validates the domain and owner details before issuing the SSL/TLS certificate. 
- An SSL/TLS certificate contains the information - Domain name, Certificate authority, Certificate authority's digital signature, Issuance date, Expiration date, Public key, SSL/TLS version.
- SSL/TLS handshake involves the following steps:
  1. The browser opens an SSL/TLS-secure website and connects to the web server.
  2. The browser attempts to verify the authenticity of the web server by requesting identifiable information. 
  3. The web server sends the SSL/TLS certificate that contains a public key as a reply.
  4. The browser verifies the SSL/TLS certificate, ensuring that it is valid and matches the website domain. Once the browser is satisfied with the SSL/TLS certificate, it uses the public key to encrypt and send a message that contains a secret session key.
  5. The web server uses its private key to decrypt the message and retrieve the session key. It then uses the session key to encrypt and send an acknowledgment message to the browser.
  6. Now, both browser and web server switch to using the same session key to exchange messages safely. 
- A **session key** maintains encrypted communication between the browser and web server after the initial SSL/TLS authentication is completed. The session key is a cipher key for symmetric cryptography.
- SSL/TLS certificates that support different domain types are:
  1. Single domain certificate
  2. Wildcard certificate
  3. Multi-domain certificate

### [What's the difference between SSL and TLS?](https://aws.amazon.com/compare/the-difference-between-ssl-and-tls/)

- Secure Sockets Layer (SSL) is a communication protocol, or set of rules, that creates a secure connection between two devices or applications on a network. 
- SSL is technology your applications or browsers may have used to create a secure, encrypted communication channel over any network.
- Transport Layer Security (TLS) is the upgraded version of SSL that fixes existing SSL vulnerabilities. TLS authenticates more efficiently and continues to support encrypted communication channels.
- At present, all SSL certificates are no longer in use. TLS certificates are the industry standard.
- A **cipher suite** is a collection of algorithms that create keys to encrypt information between a browser and a server.
- [differences: SSL vs. TLS](https://aws.amazon.com/compare/the-difference-between-ssl-and-tls/)

#### Create new certificates
1. [Create a self-signed certificate](https://learn.microsoft.com/en-us/azure/iot-hub/reference-x509-certificates#create-a-self-signed-certificate)
2. [Certificates for localhost](https://letsencrypt.org/docs/certificates-for-localhost/)
3. [Generate self-signed certificates with the .NET CLI](https://learn.microsoft.com/en-us/dotnet/core/additional-tools/self-signed-certificates-guide)
   1. [Install .NET](https://learn.microsoft.com/en-us/dotnet/core/install/windows#net-installer) and Docker desktop 

   2. Create a folder to store your certificate

      ```powershell
      New-Item -ItemType Directory -Path "C:\certs" -Force
      ```

   3. Generate the self-signed certificate

      ```powershell
      $cert = New-SelfSignedCertificate -DnsName @("localhost", "contoso.com") -CertStoreLocation "cert:\LocalMachine\My"
      ```

   4. Define the file path and export password

      ```powershell
      $certKeyPath = "C:\certs\contoso.com.pfx"
      ```

      ```powershell
      $password = ConvertTo-SecureString 'MySecurePassword123!' -AsPlainText -Force
      ```

   5. Export the certificate to a `.pfx` file

      ```powershell
      $cert | Export-PfxCertificate -FilePath $certKeyPath -Password $password
      ```

   6. Trust the certificate on your host machine

      ```powershell
      Import-PfxCertificate -FilePath $certKeyPath -CertStoreLocation 'Cert:\LocalMachine\Root' -Password $password
      ```

   7. Run your Web App in Docker with HTTPS enabled `https://localhost:8001`

      ```docker
      docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_Kestrel__Certificates__Default__Password="MySecurePassword123!" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/contoso.com.pfx -v C:/certs:/https mcr.microsoft.com/dotnet/samples:aspnetapp
      ```
   8. Clean up

      ```powershell
      $cert | Remove-Item
      Get-ChildItem $certKeyPath | Remove-Item
      $rootCert | Remove-item
      ```

4. The [`New-SelfSignedCertificate`](https://learn.microsoft.com/en-us/powershell/module/pki/new-selfsignedcertificate?view=windowsserver2025-ps) cmdlet creates a self-signed certificate for testing purposes.


#### In General

1. Public key cryptography uses a pair of mathematically related cryptographic keys, known as the public and private key.
2. RSA-2048 is an asymmetric cryptographic algorithm.
3. **Self-signed certificate**:  A certificate signed by the private key that matches the public key of the certificate is known as a self-signed certificate. Root certification authority (CA) certificates fall into this category.
4. **Certification Authority**: Certificates are obtained from certification authorities. Because these CAs are widely trusted organizations, the certificate will be recognized widely.
