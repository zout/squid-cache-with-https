# Squid https cache proxy
Based on a cusom build of squid with an builder step so all the 
build tools are not included in the final image.

### Make certs

Run the image:

    $ docker-compose run proxy bash

Open the OpenSSL config:

    $ nano /etc/ssl/openssl.cnf
    
When you're in, add under the `\[ v3_ca \]` section the following: 

    [ v3_ca ]
    keyUsage = cRLSign, keyCertSign
    
Now it's time to generate the keys
    
    $ openssl req -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -extensions v3_ca -keyout /usr/local/squid/etc/ssl_cert/myCA.pem -out /usr/local/squid/etc/ssl_cert/myCA.pem
    $ openssl x509 -in /usr/local/squid/etc/ssl_cert/myCA.pem -outform DER -out /usr/local/squid/etc/ssl_cert/myCA.der
    
In the `/conf/ssl_cert` folder should be a `myCA.der`. You need 
to thrust that in your browser or system.

## Running the system

With the following command you can get the proxy server up. 

    $ docker-compose up
    
As you can see I've man in the middle attacked myself.

![screenshot][logo]

[logo]: docs/Screenshot.png "Screenshot with Peperholding as signer of Google.com"
