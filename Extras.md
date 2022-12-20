# EXTRAS

Extra content for the bigous protocolous.

| TABLE OF CONTENTS____________________________________________________________________________________________________________________ |
| :------------------------------------------------------------------------------------------------------------------------------------ |
| [SSL/TLS Certificates](#what-are-ssltls-certificates)                                                                                 |

[to Bibliography](#bibliography)


---------------------------------------------------------------------------------------------------------------------------------------------------


## what are SSL/TLS Certificates

When sending a public key over the net to a client, the message (that is not yet encrypted) is very susceptible to a MitM attack. An attack can easily switch the server's public key inside with their own and relay the message back to us. So what now? Does that mean that we can no longer trust public keys? Nope.

The way we get around that problem is by `digitally signing our keys` and creating what's known as a `digital certificate`. Do not confuse both terms, signing and certifying are not the same.

The digital certificate's job is to `certify our key as our server's true public key` and serve as a `signature of it's validity`.
You may continue reading how that is done, but it's not truly necessary.

### Digital Signature

Let's meet our new best friend, CA Server, Certificate Authority servers. A CA server's 

A digital signature is simply put, the `server's key encrypted by the CA's private key`. That way, the client receiving the signature can `decrypt the server's key with the CA's public key`. This method ensure that the decrypted key was not changed in transit by two ways:

1. The key will not decrypt correctly if the ciphertext was changed
2. The key will not decrypt correctly if it was encrypted by another private key (For example, a MitM can change the signature to another signed by their own private key).
3. No one can fake a signature issued by that CA unless they have their **private** key. (Which no one else should, it's called private for a reason lol)

### Digital Certificate

Now that we have a `digital signature`, all we need to do to create the certificate is superrrr simple. Attach the original key to the signature to create the certificate.

That is done so after decrypting the key fron the signature we will have something to cross-refernce will the result. If they are the same that means that the original key (The one that was added to the signature) was not changed and we can trust it. If they are not the same it means something is fishy and we should not use that key.

### But even with all of this, it's not enough!

With the current model in mind, an attacker can invest great effort and create a CA server of his own. Change the `SERVER CERTIFICATE` message to include their own key, `impersonate a CA` and the decryption of the digital signature will come out fine (since he signed his own key and the client thinks he is the CA so he will use the attacker's public key).

So we need a way to certify the certificates, and for that we can just tweak something in the existing model. We **take the Certificate Authority servers and we give some of them higher importance** so that they will be able to **certify other CA servers**, that job is not done by computers but by humans, so a threat actor (like MitM) will not be able to fake that. If you want to create a CA server you will have to **go to a higher level CA** and tell them, **they will verify you** and your validity, and after tons of bureaucracy and paperwork **you will be given a certificate** that means you are *certified to certify*.

So then your certificate, will not only include the key and it's signature but also a link to the higher level CA's certificate of you.

It's goes like, layer to layer until we get to the `ROOT CA`, that certificate has no linked certificate. It certifies itself and it is hardcoded into your system so you wont be able to fake that.

So that's how we certify a message in the network, a little complicated but it gets the job done.

**NOTICE**: Digital signatures and certificates can be applied to any sort of information, not only server public keys.


---------------------------------------------------------------------------------------------------------------------------------------------------


## Message Authentication Codes

MAC algorithms are **Message Authentication Codes** algorithms, using them the server calculates a "Code" from the message and a HMAC key and couples it with the message. Then the recipient of the message will decrypt the message, perform the same mac function using the decrypted message and the key and compare the values to check that the message was not changed during transit.


---------------------------------------------------------------------------------------------------------------------------------------------------


## Bibliography

> ### SSL/TLS Certificates
>
> 2. "[What are SSL/TLS Certificates? Why do we Need them? and How do they Work?](https://www.youtube.com/watch?v=r1nJT63BFQ0)", Youtube video by **Hussein Nasser**.
