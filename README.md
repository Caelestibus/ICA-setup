# ICA-setup

Internal Certificate Authority (CA) Setup with OpenSSL (Creating the CA Infrastructure)

---

## 🚀 Steps by step process

---

### 1. Create Certificate Authority (CA) directory, creating both the parent directory and subdirectory:

```bash
mkdir -p ~/internalCA/ca
cd ~/internalCA/ca
```
---


### 2. Generate encrypted private key for the CA:

```bash
openssl genpkey -algorithm RSA -aes256 -out ca.key -pkeyopt rsa_keygen_bits:4096
```
or

```bash
openssl genpkey -algorithm RSA -out ca.key -pkeyopt rsa_keygen_bits:4096 -aes-256-cbc
```

At this point you would be prompt to set a passphrase (set a password).

---

### 3. Generate self-signed CA certificate (valid for 5 years):

```bash
openssl req -new -x509 -days 1825 -key ca.key -out ca.crt
```

After running this command, you will have to fill in all the required information. Kindly note that you have successfully created ca.crt as no indicator would be displayed.

---


### 4. Create a server directory using the same command line, as we did while creating the CA directory:

``` bash
mkdir -p ~/internalCA/server
cd ~/internalCA/server
```

---

### 5. Generate private key for server:

```bash
openssl genpkey -algorithm RSA -out devportal.key -pkeyopt rsa_keygen_bits:2048
```

---

### 6. Create CSR (Certificate Signing Request):

```bash
openssl req -new -key devportal.key -out devportal.csr
```

This point you fill in the neccessary required information with your companies info, as you would see on the screenshot attached under the file session.

---


### 7. Sign the CSR using our internal CA:

```bash
openssl x509 -req -in devportal.csr -CA ../ca/ca.crt -CAkey ../ca/ca.key -CAcreateserial -out devportal.crt -days 365
```

Certificate devportal.crt successfully created and signed.

---


### 8. Run OpenSSL verification:

```bash
openssl verify -CAfile ../ca/ca.crt devportal.crt
```

devportal.crt: OK

---


# Note 📝

Run this command to view your certificate

```bash
openssl x509 -in ca.crt -text -noout
```

As running

```bash
cat ca.crt
```
would open the certificate in it's encoded form, xoxo guys 💋💋

---

