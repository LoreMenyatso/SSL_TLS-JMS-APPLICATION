# SSL_TLS-JMS-APPLICATION
This task aims to establish security from a messaging application that transmits data to an IBM MQ application. How security will be implemented will be through encrypting whatever data is sent to the MQ using SSL/TLS.


First, when analysing our system network-wise, we see that data is sent bare (plain text). This makes our system susceptible to Man-in-the-middle-attacks or eavesdropping. For most legal jurisdictions data sent to be compliant to certain data protection policies such as PCI DSSI & GDPA.



So in the previous section after we managed to get the application to send data to our MQ. So when we execute the application this is the data we get in plain text.







To test we opened WireShark and traced the traffic from our server.



As displayed in the zoomed picture below, our data passes in plain text. We can see the exact message that we got from the application being sniffed by the Wireshark.  The contents of the message travelled in plain text and could have been viewed by a third party just as I have done here with Wireshark




Open the Java code and uncomment line below.




Open the server that is hosting MQ, in this case, it's a Linux server. However, navigate to a directory and execute the command below to create a self-signed cert.

Run the code below to initiate the SSL cert creation process.

openssl req -newkey rsa:2048 -nodes -keyout key.key -x509 -days 365 -out key.crt




Fill in the details as prompted.



Below are the certs created.



Now share these certificates from the server to the client via any desirable method. In this case, I was using gsutil as my server is hosted on Google Cloud.






Now that you have the MQ client libraries, you’ll have the MQ security command line tool, runmqakm. Enter this command to create a keystore in .kdb format and store the password in a .sth file



After the command is executed we see the newly created password store file.



Import certificates into the keystore using the command below.



We should have this file in our directory.


__________________

When we initialize the container we should provide the directory of the key files.

So from the documentation, it seems we have to create a non running container just for the mere purpose of mounting the files to the storage volume.




We can see the newly non-running container named config container




Copy the files over to the directory in the container.





This completes the initial set-up. Now, we can enable TLS using the volume created previously and the MQ Docker image.

