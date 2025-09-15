# PKI (Public Key Infrastructure) and its Relation to SSL/TLS

## What is PKI?
**PKI (Public Key Infrastructure)** is the system, policies, roles, and technologies that enable secure digital communication using **cryptographic keys and certificates**.

It provides the **trust framework** that SSL/TLS relies on.

### Core Components of PKI
1. **CA (Certificate Authority):** Issues and signs digital certificates (e.g., DigiCert, Let’s Encrypt).  
2. **RA (Registration Authority):** Verifies identity before CA issues certificates.  
3. **Certificate (X.509):** A digital document that binds a public key to an entity (domain, person, organization).  
4. **CRL (Certificate Revocation List) / OCSP (Online Certificate Status Protocol):** Ways to check if a certificate is revoked.  
5. **Public/Private Key Pairs:** Used for encryption, signing, and authentication.

---

## How PKI Relates to SSL/TLS
SSL/TLS is the **protocol** that provides secure communication between a client (browser, app) and a server.

PKI provides the **trust mechanism** that SSL/TLS relies on to verify identities.

### SSL/TLS Handshake Flow with PKI
1. **Server presents its certificate** (issued by a CA).  
2. **Client validates the certificate** using PKI:  
   - Checks the CA’s signature (must be in the client’s trusted CA store).  
   - Checks validity dates, revocation status, and domain name.  
3. **If trusted, client and server exchange keys** (Diffie-Hellman or RSA) to establish a secure session.  
4. **All communication is encrypted** using session keys (symmetric encryption for speed).

---

## Example
Visiting `https://bank.com`:
- The server sends its SSL/TLS certificate, signed by **GlobalSign (CA)**.  
- Browser checks its **PKI trust store** → finds GlobalSign → validates signature.  
- If valid, TLS session is established → traffic is encrypted.  
- If invalid (e.g., self-signed or expired certificate), the browser warns the user.

---

## Summary
- **PKI = Trust framework** (keys, certificates, CAs, policies).  
- **SSL/TLS = Protocol** that uses PKI to secure communication.  
- Without PKI, SSL/TLS would still encrypt traffic, but there would be **no assurance of identity**, leaving users vulnerable to attackers.

---

# 🔐 SSL/TLS Certificates Explained
> **SSL** – Secure Sockets Layer (old version of TLS, now obsolete).

## 📖 Abbreviations

- **SSL** – Secure Sockets Layer (old version of TLS, now obsolete)  
- **TLS** – Transport Layer Security (successor to SSL, used today)  
- **mTLS** – Mutual TLS (both client and server present certificates)  
- **CA** – Certificate Authority (trusted organization that issues certificates)  
- **DV** – Domain Validation (certificate validation level)  
- **OV** – Organization Validation (certificate validation level)  
- **EV** – Extended Validation (certificate validation level)  
- **SAN** – Subject Alternative Name (extension in certificate allowing multiple domains)  
- **AES** – Advanced Encryption Standard (symmetric encryption algorithm)  
- **S/MIME** – Secure/Multipurpose Internet Mail Extensions (email signing/encryption)  
- **X.509** – Standard format for public key certificates  

---

## 🔑 What is an SSL/TLS Certificate?

An **SSL/TLS certificate** is a **digital file** issued by a **Certificate Authority (CA)** that helps:

1. **Encrypt** communication between two parties (client ↔ server).  
2. **Authenticate** the server (and sometimes the client).  
3. **Provide trust** to users that they are talking to the right system.  

Think of it as a **passport for a website/server**:
- Contains identity info (domain, company, etc.).
- Holds a **public key** that others use for encryption.
- Is signed by a **CA**, so browsers/clients trust it.

---

## 📦 What’s Inside a Certificate (X.509 format)?

1. **Subject** – Who the certificate is issued to (`www.example.com`).  
2. **Issuer** – Who issued the certificate (e.g., DigiCert, Let’s Encrypt).  
3. **Public Key** – Used for encryption/signature verification.  
4. **Validity Period** – From (start date) to (expiry date).  
5. **Serial Number** – Unique ID for the certificate.  
6. **Extensions** – Additional info:
   - **SAN (Subject Alternative Names)** – extra domains covered.  
   - **Key Usage** – allowed uses (encryption, signing, etc.).  
7. **Signature** – The CA’s cryptographic signature proving authenticity.

📌 **Example:**  
Certificate for `www.amazon.com`:
- Subject = `www.amazon.com`
- Issuer = DigiCert
- Validity = 2025-01-01 → 2026-01-01
- Public Key = Amazon’s TLS key
- Extensions = SAN (`amazon.com`, `*.amazon.com`)

---

## 🔐 How Does Encryption Work?

When a browser connects to `https://example.com`:

1. **Handshake Start** – Browser: *"Let’s talk securely."*  
2. **Server Sends Certificate** – Includes its public key.  
3. **Browser Verifies**:
   - Correct domain, valid issuer, not expired.  
4. **Key Exchange**:
   - Browser generates a session key.  
   - Encrypts it with server’s **public key**.  
   - Server decrypts with **private key**.  
5. **Secure Channel** – Both share a secret session key (AES).  
6. **Data Transfer** – Communication is now encrypted.

---

## 🏷 Types of Certificates (by Validation Level)

1. **DV (Domain Validation)**  
   - Proves ownership of domain only.  
   - Fast, automated, cheap (e.g., Let’s Encrypt).  
   - ✅ Best for personal sites, blogs, dev/test systems.  

2. **OV (Organization Validation)**  
   - Verifies domain + company details.  
   - Shows organization name in certificate.  
   - ✅ Best for business websites, APIs, corporate apps.  

3. **EV (Extended Validation)**  
   - Strongest validation (legal checks + documents).  
   - Browser shows company name in address bar.  
   - ✅ Best for banks, e-commerce, high-trust sites.  

---

## 🌐 Types of Certificates (by Scope)

1. **Single-domain certificate**  
   - Covers one domain (`example.com`).  
   - ✅ Use for simple websites.  

2. **Wildcard certificate**  
   - Covers all subdomains (`*.example.com`).  
   - ✅ Use if you have many subdomains.  

3. **Multi-domain (SAN) certificate**  
   - Covers multiple domains (`example.com`, `example.org`).  
   - ✅ Use for SaaS platforms with multiple brands.  

4. **Self-signed certificate**  
   - Signed by yourself, not by a CA.  
   - Not trusted by browsers.  
   - ✅ Use for local development, internal services.  

---

## 📡 Certificates Beyond HTTPS

Certificates are not only for websites:

1. **Web (HTTPS / TLS)**  
   - Browser ↔ Website encryption.  
   - Usually DV/OV/EV certificates.  

2. **Server-to-Server (mTLS)**  
   - Both client and server exchange certificates.  
   - ✅ Used in APIs, microservices, Kubernetes, service mesh.  

3. **Email Security (S/MIME certificates)**  
   - Sign and encrypt emails.  

4. **Code Signing Certificates**  
   - Sign executables or scripts.  
   - Ensures code is authentic and unaltered.  

5. **Client Certificates**  
   - Used by clients (users/apps) to authenticate to servers.  
   - ✅ Example: VPN login with certificates.  

---

## 🧭 Where to Use What?

| Context              | Certificate Type         | Example                                      |
|----------------------|--------------------------|----------------------------------------------|
| Personal blog        | DV                       | `https://myblog.com`                         |
| E-commerce site      | OV or EV                 | `https://shop.com`                           |
| SaaS with subdomains | Wildcard                 | `https://customer1.app.com`                  |
| SaaS with domains    | Multi-domain (SAN)       | `https://saas.com`, `https://saas.net`       |
| Internal services    | Self-signed + mTLS       | Service A ↔ Service B                        |
| Banking APIs         | OV/EV + mTLS             | Bank ↔ Fintech partner                       |
| Signed apps          | Code Signing             | Windows EXE, Mac App                         |
| Email security       | S/MIME                   | Signed corporate emails                      |

---

## 💡 Summary

- **Websites for the public** → DV/OV/EV from trusted CA.  
- **Internal systems** → Self-signed or private CA + mTLS.  
- **APIs/microservices** → mTLS with client/server certificates.  
- **Software/email** → Code Signing, S/MIME.  

---

## Tools to deal with this 

### 1. 🌍 Public Certificates (from Certificate Authorities)
Let’s Encrypt (via Certbot)

Free, automated CA for public TLS certificates.

**Certbot is the most popular client.**

Example:
```sh
sudo certbot certonly --standalone -d example.com
sudo certbot renew
```

> Certificates are valid for 90 days and auto-renew.

**acme.sh**

Lightweight ACME client written in shell script.

Works with Let’s Encrypt and other ACME-compatible CAs.

Popular in Docker and Kubernetes environments.

[How to use](https://wiki.archlinux.org/title/Acme.sh)

**Commercial CA APIs**

Providers like `DigiCert`, `GlobalSign`, `Venafi offer APIs`.

Enterprise-grade lifecycle management for certificates.

### 2. 📦 Automation in Cloud & DevOps

*HashiCorp Vault*
- Acts as a dynamic CA.
- Issues short-lived certificates on demand.
- Example: microservices automatically request TLS certs from Vault.

*Kubernetes Cert-Manager*

Kubernetes operator that automates issuing and renewing TLS certificates.

Works with ACME (Let’s Encrypt) or internal CAs.

Example manifest:
```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: mysite-cert
spec:
  secretName: mysite-tls
  dnsNames:
    - mysite.example.com
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
```

**Cloud Provider Services**

`AWS Certificate Manager (ACM)` – Auto provisions and renews certs for ALB, CloudFront, API Gateway.

`Azure Key Vault Certificates` – Stores, manages, and auto-renews certs for Azure services.

`Google Cloud Certificate Manager` – Auto TLS certificate management for GCP workloads.

### 3. 🔍 Validation & Testing Tools

`openssl s_client` - Debug and check certificate details manually.

Example:
```sh
openssl s_client -connect example.com:443 -servername example.com
```

**SSL Labs Test (Qualys)**

`Online tool` - [https://www.ssllabs.com/ssltest](https://www.ssllabs.com/ssltest)

Analyzes certificate validity, TLS config, expiry, and vulnerabilities.

**CLI Tools**

`sslscan` – Scans supported cipher suites.

`testssl.sh` – Deep TLS testing script.

`checkssl` – Quick trust and expiry check.

`mkcert` - Developer tool to generate trusted local certificates.

Example:
```
mkcert localhost 127.0.0.1 ::1
```

### 4. 🔄 Certificate Lifecycle Management (Enterprise)

`Venafi` – Enterprise certificate lifecycle management.

`Keyfactor` – Automation and governance at scale.

`AppViewX CERT+` – Full lifecycle automation platform.

`Sectigo Certificate Manager` – Enterprise PKI and IoT device certificates.
