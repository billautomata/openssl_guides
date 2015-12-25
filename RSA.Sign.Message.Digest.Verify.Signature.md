Use OpenSSL and an RSA key-pair to sign and verify the digest of an unencrypted file.  You create a digest of the file, then sign that digest with your private key creating a signature.  Others can then take your public key, the signature, and the file, and verify that the contents of the file have not been tampered with.

```bash
# create a plaintext
echo 'too many secrets' > plain.txt

# generate private / public key-pair (not necessary if you already have keys)
openssl genrsa -out rsa.private.4096.pem 4096
openssl rsa -in rsa.private.4096.pem -outform PEM -pubout -out rsa.public.4096.pem
# generate and sign a message digest with the private key
openssl dgst -md5 -sign rsa.private.4096.pem -out signature.bin plain.txt
# verify the signature with the public key
openssl dgst -md5 -verify rsa.public.4096.pem -signature signature.bin plain.txt
# returns > Verified OK
```
