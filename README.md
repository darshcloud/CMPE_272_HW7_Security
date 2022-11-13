# CMPE 272 HW7 Security

Design and build a PKI infrastructure that includes Root CA, Signing CA, and TLS Certificate,
E.g., as described here: https://pki-tutorial.readthedocs.io/en/latest/simple/.
Use the TLS certificate to install a web server, e.g. tomcat, https://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html.
Document your progress and submit Word document including github reference to code, etc..

## Implementation Steps<br/>
### Step 1:<br/>
Spin up an EC2 Linux instance on AWS cloud<br/>
### Step 2: 
Install git using the below command<br/>
sudo apt-get install git
### Step 3:
Fetch the sample files using the command git clone https://bitbucket.org/stefanholek/pki-example-1 and follow the instructions provided in the URL https://pki-tutorial.readthedocs.io/en/latest/simple/ and change into the new directory created using the cd command.
### Step 4:
Creating a Root CA
* Create root directories by running the below commands<br/>
mkdir -p ca/root-ca/private ca/root-ca/db crl certs<br/>
chmod 700 ca/root-ca/private<br/>
* Create database using the below commands<br/>
cp /dev/null ca/root-ca/db/root-ca.db <br/>
cp /dev/null ca/root-ca/db/root-ca.db.attr<br/>
echo 01 > ca/root-ca/db/root-ca.crt.srl<br/>
echo 01 > ca/root-ca/db/root-ca.crl.srl<br/>
* Creating a CA request<br/>
openssl req -new \ <br/>
-config etc/root-ca.conf \ <br/>
-out ca/root-ca.csr \ <br/>
-keyout ca/root-ca/private/root-ca.key <br/>
* Creating a CA certificate <br/>
openssl ca -selfsign \ <br/>
    -config etc/root-ca.conf \ <br/>
    -in ca/root-ca.csr \ <br/>
    -out ca/root-ca.crt \ <br/>
    -extensions root_ca_ext <br/>
### Step 5:
Creating a Signing CA
* Create Signing CA directories by the running the below command <br/>
mkdir -p ca/signing-ca/private ca/signing-ca/db crl certs <br/>
chmod 700 ca/signing-ca/private <br/>
* Create Database using the below commands
cp /dev/null ca/signing-ca/db/signing-ca.db <br/>
cp /dev/null ca/signing-ca/db/signing-ca.db.attr <br/>
echo 01 > ca/signing-ca/db/signing-ca.crt.srl <br/>
echo 01 > ca/signing-ca/db/signing-ca.crl.srl <br/>
* Creating a CA request
openssl req -new \ <br/>
    -config etc/signing-ca.conf \ <br/>
    -out ca/signing-ca.csr \ <br/>
    -keyout ca/signing-ca/private/signing-ca.key <br/>
* Creating a CA certificate using the below command
openssl ca \ <br/>
    -config etc/root-ca.conf \ <br/>
    -in ca/signing-ca.csr \ <br/>
    -out ca/signing-ca.crt \ <br/>
    -extensions signing_ca_ext <br/>









