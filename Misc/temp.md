## Transport Layer Security **/** Secure Socket Layer A.K.A TLS/SSL
<font size=2>*[#Layer-5] [#Application-Based-Port] [#Encrypted]*</font><br><br>

Both TLS and SSL are protocols used to encrypt information on the wire in a way that both endpoints can decrypt them. That is why they belong in the `Session Layer`. Nowadays TLS is the standard for session encryption and regarded as the better protocol, originally TLS was born from SSL 3.0 and superseeded it.

```
SSL 1.0 -> SSL 2.0 -> SSL 3.0 -> TLS 1.0 -> TLS 1.1 -> TLS 1.2 -> TLS 1.3
```

WHY WERE THEY INVENTED?

At the time, communication on the internet was transmitted cleartext, which posed great dangers to it's users. So in the year 1995 [Netscape](https://www.youtube.com/watch?v=dQw4w9WgXcQ) introduced this protocol to serve protection to the communication by encrypting the packet's contents when it travels.

HOW DO THEY WORK?

The process of encryptions goes as such:
- Protocol Handshake
- Encryption

Sounds simple on paper, but it reality it is more complicated.

The SSL & TLS protocols are made up of 4 sub-protocols, each responsible for another part of the process.

| Protocol                    | Purpose                                                                             |
| :-------------------------- | :---------------------------------------------------------------------------------- |
| Handshake Protocol          | Establish security parameters and session.                                          |
| Change Cipher Protocol      | Responsible for cipher changing negotiations.                                       |
| Alert Protocol              | Responsible for alerting if there is an error/warning.                              |
| Record Protocol             | This is the protocol that encrypts, compresses and ecapsulates the application data.|

<sup style="font-size: 0.3em">* Doesn't exist in TLS 1.3</sup>

SSL/TLS also has **two major concepts**:

<font size=2> **SESSION** </font><br>
> A session is a set of negotiated security parameters (key exchnage & encryption algorithms, SID etc.) between one endpoint and another.

<font size=2> **CONNECTION** </font><br>
> A connection is the application of a session's parameters to an active network connection.

### Handshake Protocol (SSL 3.0 - TLS 1.2)

> <font size=4>**STEP 1: CLIENT HELLO**</font>
>
> Basically the client send a tls/ssl session initiation message, In this message will be a few things that are needed in order to synchronize the encryption. The `latest tls/ssl version the client supports`, `A random number`, `A session identification number`, `A cipher suite` (A list of the **encryption and key exchange** algorithms it supports sorted by preference), `A list of compression algorithms` sorted by preference as well.
> After sending the client hello the client waits for a **SERVER HELLO** message, meaning the server wishes to initiate a connection.
>
> If the server doesn't accept the **CLIENT HELLO** message for some reason then it will send back a `handshake failure` message.

> <font size=4>**STEP 2.1: SERVER HELLO**</font>
>
> The server looks at the **CLIENT HELLO** message and chooses all the best compatible options and send them back to the client as a sort of "I choose this from what you gave me". To this, the server will add a random number of his own and dictate the Session identifier.
>
> Depending on the Session ID the server may set already existing security parameters for this `connection` using the session the ID belongs to. Ultimately the server sets the session ID. If the SID is empty the server will create a new session and a new SID for the connection.

> <font size=4>**STEP 2.2: SERVER CERTIFICATE**</font>
>
> If the server needs to (which it most probably will nowadays), the server sends it's [certificate](github.com/TimonLevy/Networking/blob/main/Extras.md#what-are-ssltls-certificates) that validates it's authenticity and give the server it's public key.

> <font size=4>**STEP 2.3: SERVER HELLO DONE**</font>
>
> This is just an empty message that says "I am done, it's your turn again.". The server may add more messages inbetween the `SERVER CERTIFICATE` message and the `SERVER HELLO DONE` one for special purposes, but sending the `SERVER HELLO DONE` message means that the server is done and not to expect for more.

> <font size=4>**STEP 3: CLIENT KEY EXCHANGE**</font>
>
> After the server is done, the client will begin by **initiating a key exchange** that they will later use to **create session keys**. The exchange algorithm is defined by the cipher suite that the server chose in their `SERVER HELLO` message.
>
> We will not go into the different ways to exchange keys, but it is important to note:
> 
> in **TLS** the keys can not be calculated by anyone else other than the client and server since at least one parameter in the formula will be a secret. That is what's called a `pre-master secret`.
>
> In **SSL** a huge exploit was found that allowed attackers to extract the server's private key and possibly find out the key. SSL 3.0 was the first version to support Diffie-Hellman Key exchange, prior to that only RSA was allowed.

> <font size=4>**STEP 4.1: CHANGE CIPHER SPECIFICATIONS**</font>
>
> In this step the client sends a `change cipher spec` message telling the server to update it's connection's state.

> <font size=4>**STEP 4.2: HANDSHAKE FINISHED**</font>
>
> The client will send a `FINISHED` message that is already encrypted in the negotiated encryption algorithm and uses the exhcnaged keys for encrypting. this message will usually be the last before the encrypted conversation starts between the both but a server can send a `FINISHED` message as well.

```
.               CLIENT                            SERVER
.                 │           CLIENT HELLO          │
.                 │ ──────────────────────────────> │
.                 │                                 │
.                 │          SERVER  HELLO          │
.                 │       SERVER  CERTIFICATE       │
.                 │        SERVER HELLO DONE        │
.                 │ <────────────────────────────── │
Client verifies   │ <────────────────────────────── │
Certificate       │ <────────────────────────────── │
.                 │                                 │
.                 │          KEY  EXCHANGE          │
.                 │       CHANGE  CIPHER SPEC       │
.                 │            FINISHED             │
.                 │ ──────────────────────────────> │
.                 │ ──────────────────────────────> │
.                 │ ───────────ENCRYPTED──────────> │
.                 │                                 │
.                 │       CHANGE  CIPHER SPEC       │
.                 │             FINISHED            │
.                 │ <────────────────────────────── │
.                 │ <───────────ENCRYPTED────────── │
.                 │                                 │
.                 │            APP  DATA            │
.                 │ ───────────ENCRYPTED──────────> │
.                 │                                 │
.                 │             RESPONSE            │
.                 │ <───────────ENCRYPTED────────── │
.                 │                                 │
.                 ▼                                 ▼
``` 
A basic diagram showing the flow of a handshake. there may be more messages then this, but this is the bare-necessity.

### Change Cipher Spec Protocol

This protocol is the simplest of the sub-protocols, it is made up of a single byte with the value of "1". The purpose of this protocol is to tell the recipient to update the connection's state to use the negotiated CipherSpec immediately (Cipher Specifications: keys, Algorithms, etc.). This message will be enccrypted by the current connection's CipherSpecs.

### Handshake Protocol (TLS 1.3)
TLS saw a big revision in the handshake process, one that made it way less viable for attacks, quicker and kept `Perfect Forward Secrecy`.

Here's an overview of it.
```┌─┐│└─┘▼◄
.               CLIENT                            SERVER
.                 │           CLIENT HELLO          │
.                 │ ──────────────────────────────> │
.                 │                                 │
.                 │           SERVER HELLO          │
.                 │ <────────────────────────────── │
.                 │                                 │
.                 │        SERVER CERTIFICATE       │
.                 │             FINISHED            │
.                 │ <───────────ENCRYPTED────────── │
.           1 RTT │ <───────────ENCRYPTED────────── │
.                 │                                 │
.                 │        FINISHED + APP DATA      │
.                 │ ───────────ENCRYPTED──────────> │
.                 │                                 │
.                 │             RESPONSE            │
.                 │ <───────────ENCRYPTED────────── │
.                 │                                 │
.                 ▼                                 ▼
```

> <font size=4>**STEP 1: CLIENT HELLO**</font>
>
> The client send most of the same things that it used to. `Cipher suites`, `Client Random`, `tls/ssl version` (always set to 1.2), `session id` and added to that is a new attribute `key share`. The protocol specification only allow perfect forward secrecy compatible ciphers, so this `key share` value is the value the server needs in order to generate the key. The client does not know which protocol the server will pick so it may send a few `key share`s to match it's cipher suites.

> <font size=4>**STEP 2: SERVER HELLO**</font>
>
> The server will pick a cipher, calculate the `client key` using the client's `key share`. Then it creates a new `key share` of it's own and sends that and the chosen cipher to the client.
> 
> After this message the connection will be encrypted.

After this the server will send a certificate to validate itself and a finished message to let the client know it's done. Using the server's `key share` the client can `generate the server's symmetric key` and decrypt the messages from the server.

* The client will send messages encrypted with it's client key and decrypt messages from the server using the server's key.
* The server will send messages encrypted with it's server key and decrypt messages from the client using the client's key.

Another feature TLS version 1.3 provides, is the ability to send a client hello message together with the first byte of data already encrypted! This is called a 0-RTT handshake.

### Alert Protocol

This protocol will alert the recipient in case of an error or warning, the alerts are labeled by level of severity. In case of a fata severity the connection is immediately terminated and the session nullified, to not let a new connection use the same parameters and potentially repeat the error.

### Record Prtocol

The record protocol is responsible for the encryption of the message, the validation of message integrity, fragmantation and the compression. It does that using the current CipherSpecs which will include the encryption algorithms, keys, compression algorithms and [MAC algorithms](github.com/TimonLevy/Networking/blob/main/Extras.md#message-authentication-codes).

However, TLS uses a different MAC lgorithm called HMAC, essentially it hashes the message to make it more compact but still representative of the original message. 

The whole process goes like this:
1. The mesage will be fragmented.
2. Each fragment will be compressed and become smaller.
3. Each fragment will have a MAC calculated and appended to it, the fragment will be back to it's original size.
4. The fragments will be encrypted and a  Record Protocol header will be added to it.

```diff
- (Is the only purpose of the protocol is to create ecryption keys to symetrically encrypt the connection?)
- (Stratesd checking this protocol but you wrote W-A-Y too much information summaries it)
```
