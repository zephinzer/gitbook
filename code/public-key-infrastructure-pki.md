# Public Key Infrastructure \(PKI\)

## Create an SSH key-pair

The following should generate a 4096-bit key pair at `./id_rsa` and `./id_rsa.pub` for use with SSH Git pulls/SSH deploys/SSH login to VM:

```text
ssh-keygen -t rsa -b 4096 -f ./id_rsa -q -N "";
```

## Create a certificate authority \(CA\)

The following creates a new CA with 3650 days validity at `testdomain.com`:

```text
openssl genrsa -out ./cakey.pem 4096;
openssl req -new -x509 -sha256 \
        -key ./cakey.pem \
        -days 3650 \
        -extensions v3_ca \
        -subj "/C=SG/ST=Singapore/L=Singapore/O=zephinzer/OU=example/CN=testdomain.com" \
        -out ./cacert.pem;
```

## Create a certificate signing request \(CSR\)

The following creates a CSR at `./test.csr`

```text
openssl req -new -sha256 \
  -key ./ssl/test.pem \
  -subj "/C=SG/ST=Singapore/L=Singapore/O=zephinzer-demo/OU=helm/CN=helmuser" \
  -out ./ssl/test.csr;
```

## Create a signed client certificate

Given a CA key and certificate at `./cakey.pem` and `./cacert.pem` respectively, the following creates a client certificate at `./test.pem` from the CSR at `./test.csr`

```text
openssl x509 -req \
  -days 3650 \
  -CAcreateserial \
  -CA ./cacert.pem \
  -CAkey ./cakey.pem \
  -in ./test.csr \
  -out ./test.pem;
```

## Create an SSL key

```text
openssl genrsa -out ./test.pem 4096;
```

