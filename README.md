# openssl_guides

* alice and bob generate public/private DH key-pair

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


* alice generates a strong password using bob's public key and her private key (shared secret)
* alice asymmetrically encrypts the password using her private key and bobs public key and sends it to bob
* alice symmetrically encrypts a file with the password using aes256
* bob decrypts the password using alice's public key and his private key
* bob decrypts the file using the password
