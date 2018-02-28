# Summary

This repo helps setup local Certificate Authority (CA) and issue local SSL/TLS server certificates.

With locally issued certificate in conjuction with reverse proxy (e.g. nginx) and modifying /etc/hosts file, this could help you alleviate the problems when developing in the microservice/frontend environments

## Step 1 Setup local CA

After checking out this repo, cd into the repo, and run the following command

```shell
./scripts/setup_ca
```

You'll be prompted to enter passphrases for both Root CA and intermediate CA keys. The passphrase used for intermediate CA key will be used later when generating certificates for domain names.

After completion of this step, copy the CA chain file `intermediate/certs/ca-chain.cert.pem` to the CA store on your system.

On Fedora 27, you could run the following commands to setup the system CA stores. Chrome, Firefox and Java (default) will then recognize the newly created CA.

```shell
sudo cp intermediate/certs/ca-chain.cert.pem /etc/pki/ca-trust/source/anchors/
sudo update-ca-trust
```

## Step 2 Create certificate for a domain

Run the following command to generate the key/certificate pair.

```shell
./scripts/create_server_cert
```

You'll be prompted to enter the domain name, and the passphrase for the intermediate CA key.

For example \*.example.com, you'll find the key at `intermediate/private/com.example._.pem` and the certificate at `intermediate/certs/com.example._.cert.pem`.

## What you can do after you genereated the key/certificate pair

* configure the reverse proxy (e.g. nginx) to hijack the site (domain)
* modify /etc/hosts to add an entry which points to the loopback address the reverse proxy listens to
* repeat Step 2 and the steps listed here to add more domain names
