* generate an RSA private key
* generate

```bash
# generate 4096 bit private key
openssl genrsa -out rsa.private.4096.pem 4096

# generate the public key from the private key, in PEM format
openssl rsa -in rsa.private.4096.pem -outform PEM -pubout -out rsa.public.pem

echo 'too many secrets' > plain.txt

# encrypt with public key
openssl rsautl -encrypt -inkey rsa.public.pem -pubin -in plain.txt -out cipher.txt

# decrypt with private key
openssl rsautl -decrypt -inkey rsa.private.4096.pem -in cipher.txt

```
