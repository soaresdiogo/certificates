# Digital Certificates
To Create a certificate with root by openssl

## First step is to create a root certificate

To generate your root certificate, first open your preferred shell terminal and run the following openssl command;

```bash
openssl genrsa -des3 -out rootCA.key 4096
```

## Second step is to create the signed root certificate

```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 9125 -out rootCA.crt
```

This generated .crt certificate is the certificate that you'll install on your server as a trusted root certificate.

# Domain certificate

## Third step is to create the certificate for your domain

```
openssl genrsa -out mydomain.com.key 2048
```

## Fourth step is to create the signed certificate  (csr)

**Importante:** The common Name must be different from the one generated in the root certificate, in this case the ideal is to put the domain as common name.

```
openssl req -new -key mydomain.com.key -out mydomain.com.csr
```

## Check the contents of the csr's file.

```
openssl req -in mydomain.com.csr -noout -text
```

## Generate the certificate using `mydomain.com.br` csr with the root private key

```
openssl x509 -req -in mydomain.com.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out mydomain.com.crt -days 9125 -sha256
```

## Check the content of the certificate

```
openssl x509 -in mydomain.com.crt -text -noout
```

## Generate the certificate pfx

```
openssl pkcs12 -export -out mydomain.com.pfx -inkey mydomain.com.key -in mydomain.com.crt
```
Or if you want to add intermediate strings you can add using the following command
```
openssl pkcs12 -export -out mydomain.com.pfx -inkey mydomain.com.key -in mydomain.com.crt -certfile cert.intermediate.crt
```
