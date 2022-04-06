# secure-chat-app

This program illustrates the secured communication between two parties[Let's say Alice and Bob] using secured TLS connection.

System setup : We have VM that runs two LXD container[One for Alice and other for Bob]  configured with star topology with the switch at the center and 
Alice,Bob are connected to the switch ports.Hostnames are assigned as Alice and Bob. 

Before jumping into running the program it's mandatory to generate TLS certificates for root CA and other parties using below openSSL commands.

Creation of self signed certificate by Root CA: openssl req -x509 -nodes -days 10000 -newkey rsa:2048 -keyout rootca-key.pem -out rootca.crt

Creating csr for Alice: openssl req -newkey rsa:2048 -nodes -keyout alice1-key.pem -out alice1-csr.pem
Issue Alice Certificate: openssl x509 -req -in alice1-csr.pem -CA rootca.crt -CAkey rootca-key.pem -CAcreateserial -out alice.crt -days 365 -sha256

Creating csr for Bob: openssl req -newkey rsa:2048 -nodes -keyout bob1-key.pem -out bob1-csr.pem
Issue Bob Certificate : openssl x509 -req -in bob1-csr.pem -CA rootca.crt -CAkey rootca-key.pem -CAcreateserial -out bob.crt -days 365 -sha256

Verify Alice’s certificates: openssl verify -CAfile root.crt alice.crt
Verify Bob’s certificates: openssl verify -CAfile root.crt bob.crt

Now once we have the certificates in the root store ,we can run secure_chat_app.c program in respective containers .

Bob[server] : ./secure_chat_app -s
Alice[client]: ./secure_chat_app -c bob






 
