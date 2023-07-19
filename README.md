# MailMan

# Interest
I've been trying to think of a fun C2 setup and decided to implement an email based one. This allows asynchronous control and wide support. Using two layers of encryption (the normal SSL provided by the SMTPS/POP3S/IMAPS protocols) as well as the encrypted payloads provides a lot of protection. This still wouldn't prevent a defender just blacklisting unauthorized outbound mail protocols.

# Three Main Ideas
1. Async - The client piece periodically polls the C2 account for encoded emails. It gets these emails, decrypts the payload, and executes the commands. It then packages up the output from the command in an email, and sends it to a second account. Whenever an email is retrieved it is deleted.
2. Versatile - The tool supports SMTP(S) for sending and POP3(S)/IMAP(S) for retrieving. This could be expanded via libraries, and aside from some outliers most email servers implement the protocols in the same way.
3. Protocols are fun - I'm always looking for a new opportunity to understand and play with a new protocol. This was a good reason to get familiar with SMTP/POP3/IMAP.

# Future Direction
1. Test with public mail servers - I was only able to really test against a locally hosted postfix server. Gmail no longer allows access by "insecure applications", AOL and Yahoo make it difficult to create unverified accounts, and Yandex seems to be fussy garbage. Creating valid accounts to test with various services would be ideal.
2. Break the code apart - For convenience within the development time frame, the client code (that would run on a compromised machine) and the controller code (which would run on the users machine) are in the same Python file. Breaking the code into libraries for the protocol interactions and then importing it into the respective client or controller would make it easier to work with and safer to deploy.
3. Improve large scale deployment support - Right now the malware controller would need to manually create and update keys and accounts for every deployment. It would be better if a new client could use information on the machine its running on to create a unique email account and crypto keys.
4. Make the emails look better - Currently the tool just creates a basic email with a From: To:, and then a base64 encoded blob. It would be better to have the tool use a combination of MIME as well as placing the encoded text into attachments of various sorts. This would hopefully better prevent spam filters from intercepting the emails.
5. Better (any) error handling - Currently the tool explodes if there is an email that doesn't fit the expected format. Handling and ignoring arbitrary emails would be good, as would better handling POP3.
6. Backup plans - Implement fall back servers/accounts to rotate through.

# Resources
* https://github.com/allenwillan/MailMan - this github
* https://youtu.be/fQgzZo_NUU8 - overview video
* https://help.ubuntu.com/community/PostfixBasicSetupHowto - Walk through I used to get a basic email server up and running.
* https://smsreceivefree.com - Questionable site that provides various phone numbers to receive SMS on for attempting to circumvent one time codes for sign-up websites. I used it to successfully create Yandex accounts, although Yahoo and AOL seems to have blacklisted the number ranges.
* https://support.google.com/accounts/answer/6010255?hl=en - "To help keep your account secure, from May 30, 2022, Google no longer supports the use of third-party apps or devices which ask you to sign in to your Google Account using only your username and password."
* https://docs.python.org/3/library/smtplib.html
* https://docs.python.org/3/library/poplib.html
* https://docs.python.org/3/library/imaplib.html
