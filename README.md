# Homelab Certificate Authority

## Creating a new domain certificate

First, create a private key for the domain:

```shell
$ openssl genrsa -out example.home.key 2048
```

Then we create the Certificate Signing Request (CSR):

```shell
openssl req -new -key example.home.key -out example.home.csr
```

Finally, create an X509 V3 certificate extension config file which is used to define the Subject Alternative Name (SAN) for the certificate.

```shell
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = example.home
```

To complete the generation, the final command generates the certificate.

```shell
openssl x509 -req -in example.home.csr -CA jetsonCA.pem -CAkey jetsonCA.key \
-CAcreateserial -out example.home.crt -days 825 -sha256 -extfile example.home.ext
```


Reference: https://deliciousbrains.com/ssl-certificate-authority-for-local-https-development/#becoming-certificate-authority
