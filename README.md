# CAT THINGS
 CAT THINGS is a lightweight inventory control system developed for the CAT.
 CAT THINGS will keep track of inventory levels, track usage trends, create shopping lists, and send alerts on low inventory.

## API Install Instructions
1) Prerequisite: [node.js](https://nodejs.org/en/) must be installed on your machine before proceeding.  
2) Open a console and navigate to the THINGS/API/things-api directory then execute:
```bash
npm install
```
3) Create a file in API/things-api/conf/db/db_info.js.  
4) Add the following lines:  
```javascript
    exports.config = {  
        user: 'username', //env var: PGUSER
        database: 'databasename', //env var: PGDATABASE  
        password: 'secret',  //env var: PGPASSWORD
        host: 'hostname',  // Server hosting the postgres database
        port: 5432,  //env var: PGPORT
        max: 10,  // max number of clients in the pool
        idleTimeoutMillis: 30000,  // how long a client is allowed to remain idle before being closed
    };
```  
5) Fill in the proper data to connect to your database.  
6) Create a self-signed SSL certificate:  
       (Prerequisite: openssl installed via npm insall -g openssl)  
       Execute the following from the things-api directory:  
```bash
    mkdir conf/ssl  
    cd conf/ssl
    openssl genrsa -des3 -passout pass:x -out server.pass.key 2048

    openssl rsa -passin pass:x -in server.pass.key -out server.key
    rm server.pass.key
    openssl req -new -key server.key -out server.csr
```
Fill out the fields when prompted. Leave the optional fields blank. Then execute the following:  
```bash
    openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt  
    rm server.csr
    chmod 600 ./*
```
7) Create a key to sign your json web tokens:  
        Create a file called things-api/conf/jwtSecret.key  
        Write 256 bytes of random data to the file. 
        This is easily done from an online random number generator and copy pasted in.
        This link will give you a random 256 bytes, just copy it into the file.
        https://www.random.org/cgi-bin/randbyte?nbytes=256&format=h
        or execute the following command:
```bash
    dd if=/dev/urandom of=./conf/jwtSecret.key bs=256 count=1  
```
8) Create a new file in API/things-api/conf/mailopt.js.  
9) Add the following lines:    
```javascript  
    exports.mail = {    
    from: '"name" <email-address>', // sender address
    to: 'destination' // list of receivers
    };
```
Where name is the name of the sender, email-address is their e-mail, and destination is the address to send the e-mail to.

10) Create another new file in API/things-api/conf/email_auth.js.
11) Add the following lines:
```javascript
    exports.auth = {  
    user: "sender",
    pass: "pw"
    };
```
Where sender is the e-mail address used to send e-mails about requests and inventory updates, and pw is the password to the account.  
12) Open API/things-api/Front_End/templates/js/services/thingsAPI.js and edit line 26 as follows:
```javascript
    var _urlBase = 'https://<YOUR_DOMAIN_HERE>:<YOUR_PORT>/api/';
```
    
13) Launch the server from the things-api directory with:
```bash
    npm start --port 3000
```
You may specify the port with the --port flag.
   
_This section should be updated regularly._

