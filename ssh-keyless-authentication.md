# How To Set Up SSH Key-Based Authentication on an Ubuntu Server

## Step 1 — Creating the Key Pair

The first step is to create a key pair on the client machine (usually your computer):

```shell
ssh-keygen
```
By default, recent versions of ssh-keygen will create a 3072-bit RSA key pair. For enhanced security, you can create a 4096-bit key using:

```
ssh-keygen -b 4096
```
After entering the command, you'll see:

```
Generating public/private rsa key pair.
Enter file in which to save the key (/your_home/.ssh/id_rsa):
```
Press Enter to use the default location or specify an alternate path.

Security Note: If an existing key is detected:

```
/home/your_home/.ssh/id_rsa already exists.
Overwrite (y/n)?
```
Overwriting will make previous keys unusable. Proceed with caution.

You'll then be prompted for a passphrase (recommended for added security):

```
Enter passphrase (empty for no passphrase):
```
After completion, you'll see:

```
Your identification has been saved in /your_home/.ssh/id_rsa
Your public key has been saved in /your_home/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:/hk7MJ5n5aiqdfTVUZr+2Qt+qCiS7BIm5Iv0dxrc3ks user@host
The key's randomart image is:
+---[RSA 3072]----+
|                .|
|               + |
|              +  |
| .           o . |
|o       S   . o  |
| + o. .oo. ..  .o|
|o = oooooEo+ ...o|
|.. o *o+=.*+o....|
|    =+=ooB=o.... |
+----[SHA256]-----+
```
## Step 2 — Copying the Public Key to Your Ubuntu Server
** Copying the Public Key Manually **
Display your public key:

```
cat ~/.ssh/id_rsa.pub
```
Example output:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCqql6MzstZYh1TmWWv... demo@test
```
On your server, create SSH directory (if needed):

```
mkdir -p ~/.ssh
```
Add public key to authorized keys:

```
echo public_key_string >> ~/.ssh/authorized_keys
```
Set proper permissions:

```
chmod -R go= ~/.ssh
```
Ensure correct ownership (replace sammy with your username):
```
chown -R sammy:sammy ~/.ssh
```
**Step 3 — Authenticating to Your Ubuntu Server Using SSH Keys**
Connect using:

```
ssh username@remote_host
```
First connection warning (type yes to continue):

```
The authenticity of host '203.0.113.1 (203.0.113.1)' can't be established...
Are you sure you want to continue connecting (yes/no)? yes
```
If using a passphrase, you'll be prompted to enter it.

Successful authentication opens a new shell session.

Security Recommendation: After verifying key-based authentication works, consider disabling password authentication for enhanced security.

Coopy war file to tomcat server using below command 

```
scp demo.war ubuntu@54.90.114.48:/opt/tomcat/webapps/
```
