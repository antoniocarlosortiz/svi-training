##How Emails Work

*The very basic flow.*

1. Email Client first connects to the SMTP Server of the same domain ex. dog@google.com connects SMTP.gmail.com

2. Email Client sends all the parts of the email to the Email Server including headers, body, tags, and attachments via SMTP

3. SMTP Server to look at the 'to' part from the email header and checks if it is the same domain.

	*	If it is the same. e.g. from dog@google.com to mouse@google.com:		
		*	3.a the SMTP transfers the email to the Mail Transder Agent (MTA) Server to handle where the email should be stored
	*	proceed to step 4

	*	If it is not the same e.g. from dog@google.com to cat@yahoo.com:
		*	3.b SMTP Server asks for the IP address of of the email from the Domain Name System Server (the action is called DNS lookup)
			
		*	3.c Once the IP address is obtain, SMTP server of sender connect s to the Mail Transfer Agent Server of the receiver.
		*	proceed to step 4

4. Email client of the receiver fetches the mail either via POP3 or IMAP4.

See below for the visual representation on how emails work that I got from google.

![alt text](https://github.com/antoniocarlosortiz/svi-training/blob/master/photos/email2.gif 'my drawing')

Please see link below for further reference:
[How Emails Work](http://computer.howstuffworks.com/e-mail-messaging/email.htm)

####What is POP3 and what is IMAP4?

**POP3** stands for Post Office Protocol. POP downloads emails to your computer and usually (but not always) deletes the email from the remote server. The communication is one way and the interaction of the client to server is to just download the emails to local. If you have numerous devices, all the activies you've done on one device will not be replicated on the other devices.

**IMAP4** allows users to store their email on remote servers. The two way protocol allows users to synchronize their email among multiple devices as all your activities in one device will be synched to the copy in the remote server which will then synch those changes to the other devices.

For further reference, please see links below for the official doc of both protocols:

[IETF official doc: POP3](https://www.ietf.org/rfc/rfc1939.txt)

[IETF official doc: IMAP4](https://tools.ietf.org/html/rfc3501)

####Question: If all my e-mails are stored on the server, how can i read my mail if i'm not connected to the internet?
The email client creates local copies which will automatically push all changes to the mail server the next a connection has been established.

Please see below for further reference:
[What is IMAP](http://whatismyipaddress.com/imap)

####How are attachments handled?
It is converted to a text file during the handling of the mail. Before these were all  manually encoded using the program called uenncode. What it does is that it coverts the binary of the attachment to a text file. 

Please see below for further reference on how mail servers send attachments:

[How MIME works](http://email.about.com/cs/standards/a/mime.htm)

####Internet Email Standards:

**Multipurpose Internet Mail Extensions (MIME)** - is an internet standard that extends the format of email to support like the attachment of audio or video on mails. Encoding and decoding is now auomatically done by this standard.

**POP3 and IMAP4** specify the email retrieval protocols used by e-mail clients.

**RFC 5391** Specifies the protocol by which email is transferred.

**RFC 5322** Specifies the basic format for email.

For further reference:

[Wikipedia: Internet Mail Standards](http://en.wikipedia.org/wiki/Internet_mail_standard)
