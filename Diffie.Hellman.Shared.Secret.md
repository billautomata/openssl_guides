# Diffie-Hellman + AES256 

* alice and bob generate public/private DH key-pair
* alice derives the shared secret using bob's public key and her private key
* alice symmetrically encrypts the text using the shared secret
* bob derives the shared secret using alice's public key and his private key
* bob decrypts the ciphertext using the shared secret

```bash
# generate parameters for each person
openssl genpkey -genparam -algorithm DH -pkeyopt dh_rfc5114:2 -out params.diffie.alice.pem
openssl genpkey -genparam -algorithm DH -pkeyopt dh_rfc5114:2 -out params.diffie.bob.pem

# generate the private keys for each person
openssl genpkey -paramfile params.diffie.alice.pem -out alice.private.pem
openssl genpkey -paramfile params.diffie.alice.pem -out bob.private.pem

# generate the public keys for each person
openssl pkey -in alice.private.pem -pubout -out alice.public.pem
openssl pkey -in bob.private.pem -pubout -out bob.public.pem

# derive the shared secret using each others public parts
openssl pkeyutl -derive -inkey bob.private.pem -peerkey alice.public.pem -out shared_secret_bob.bin
openssl pkeyutl -derive -inkey alice.private.pem -peerkey bob.public.pem -out shared_secret_alice.bin

# encrypt the file using aes256 and the shared secret as the password
openssl enc -aes-256-cbc -pass file:./shared_secret_alice.bin -in plain.txt -out cipher.bin

# decrypt the file using aes256 and the shared secret as the password
openssl enc -d -aes-256-cbc -pass file:./shared_secret_bob.bin -in cipher.bin
```
