## The Mission

We cannot expect anyone to grant us privacy out of their beneficence. Distrust the infrastructure and any encryption services provided by 3rd parties, by default. We must encrypt manually, locally.

Mechanisms such as TLS and passwords only partially protect the content of our messages. For a lot of threat models, it is better to use encryption for everything, not just _secret_ data.

Although not perfect, GPG is still _pretty good_ and very relevant even after decades. It is also relatively easy to use. Follow this simple guide to learn the basics.

## Assumptions

-  [`gnupg`](https://gnupg.org/download/) is installed on your system
-  `gpg --gen-key` to create a new private key
-   basic terminal knowledge
-   read [that](https://www.devdungeon.com/content/gpg-tutorial) 

## 1. Key exchange

### 1.1 Export your public key

Open up a terminal and list your secret keys first:

`gpg -K`

The output should contain at least one block like this:

```
sec   rsa4096 2022-11-01 [SC]
      BCE15F74D18112824899608AFD103120DC7BCC36
uid           [ultimate] escapethe3ra@dnmx.org
ssb   rsa4096 2022-11-01 [E]
```

The next line below _sec_ should contain your key’s fingerprint.

Now you can export your publick key in .asc (ascii armored) format:

`gpg --export -a BCE15F74D18112824899608AFD103120DC7BCC36 > escapethe3ra@dnmx.org`

_Note: replace the key fingerprint and file name accordingly._

### 1.2 Share your public key

The previous command should have created a file that starts with _—–BEGIN PGP PUBLIC KEY BLOCK—–_ and ends with _—–END PGP PUBLIC KEY BLOCK—–_. This can be shared with the world.You could make your pubkey publicly available by uploading it to your own website. It is recommended to also include your key’s fingerprint.


### 1.3 Fetch your partner’s public key

To start communicating, you need to encrypt your messages with your partner’s public key.
If the key’s fingerprint matches import it:

`gpg --show-keys 3RA_pubkey.asc`

`gpg --import 3RA_pubkey.asc`

_Note: if the fingerprint does not match, do not import it. Delete the file and try downloading it again._

## 2. Sending encrypted messages

### 2.1 Write the message

Write a simple message and save it to _msg-1.txt_:

`echo "Hello 3RA! GPG is easy :)" > msg-1.txt`

### 2.2 Encrypt and send the message

Now let’s encrypt and sign the message:

`gpg --encrypt --sign --armor --recipient BCE15F74D18112824899608AFD103120DC7BCC36 msg-1.txt`

You should now have a _msg-1.txt.asc_ file that looks like this:

```
-----BEGIN PGP MESSAGE-----
[..]
-----END PGP MESSAGE-----
```

You might want to use temporary [email](https://www.guerrillamail.com/) to communicate. 

## 3. Decrypting messages

### 3.1 Decrypting raw text streams

Run `gpg` in a terminal and paste the message at the prompt. Then, simply press _CTRL+D_ to signal end of message and trigger decryption. You should see the decrypted message in your terminal.

### 3.2 Decrypting files

For files, type in:

`gpg file_name_encrypted`

Type `N` and enter a new file name if you don’t want to overwrite the original.

Alternatively, you can skip all prompts by using `gpg --decrypt file_name_encrypted > file_name_decrypted`.

## Observations

-   any file type can be encrypted and decrypted (pdf, images, audio, video, text); watch out for malware and don’t execute stuff you don’t trust
-   GPG encryption only hides the contents of files (does not hide meta-data: message size, sender and receiver)
-   protect your secret key with a strong passphrase and never share it with anyone
-   respect your partner by fully encrypting your replies (ie. do not expose your partner’s messages in cleartext)


<br>
<p align="center">
  <img src="https://gifdb.com/images/high/pepe-frog-slow-clap-sunglasses-9td0n5o11t3kq3m2.gif"/>
</p>

