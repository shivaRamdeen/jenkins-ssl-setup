# jenkins-ssl-setup
Quick note on how to run Jenkins with SSL (not using a reverse proxy)

# Step: 1
```sudo certbot certonly --standalone --preferred-challenges http -d yourdomain.com```

#Step: 2
Follow the on screen prompts and enter the required passwords for your keystores (keep them safe)
```
cd /etc/letsencrypt/live/example.com
openssl pkcs12 -inkey privkey.pem -in cert.pem -export -out keys.pkcs12
keytool -importkeystore -srckeystore keys.pkcs12 -srcstoretype pkcs12 -destkeystore /var/lib/jenkins/jenkins.jks

```

#Step: 3
Run jenkins with https
NOTE: we set httpPort=-1 below since we are runing jenkins on port 8080 and don't want http at all

```
java -jar jenkins.war --httpPort=-1 --httpsPort=8080 --httpsKeyStore=/PATH/TO/jenkins.jks --httpsKeyStorePassword=PASSWORD_FOR_jenkins.jks 
```
