# Signature-part-1
>> Blinding Light
Here's my token signing and verification server. I'm not sure it's doing signing properly, but I've implemented some safeguards to ensure it won't hand out admin tokens to just anyone.
Connect at socket.cryptohack.org 13376
>> The problem deals with Blinding Attack: https://en.wikipedia.org/wiki/Blind_signature . The algorithm is exactly the same as in the link above, so I won't write in detail here.
>> Flag: crypto{d0n7_516n_ju57_4ny7h1n6}

>>Let's Decrypt
After reading the source code, I realized that the server will release the flag when the conditions are met:
digest = emsa_pkcs1_v15.encode(msg.encode(), 256)
calculated_digest = pow(SIGNATURE, e, n)
    if bytes_to_long(digest) == calculated_digest:
        r = re.match(r'^I am Mallory.*own CryptoHack.org$', msg)
        if r:
            return {"msg": f"Congratulations, here's a secret: {FLAG}"}
Therefore, we need to choose , , suitable to send to the server.nemsg
I have learned:
MSG = 'We are hyperreality and Jack and we own CryptoHack.org'
DIGEST = emsa_pkcs1_v15.encode(MSG.encode(), 256) #m
SIGNATURE = pow(bytes_to_long(DIGEST), D, N) #s
Written in mathematical form, we have:
s ≡ m^D (mod N)
One of the matches is:msg
msg = 'I am Mallory, I own CryptoHack.org'
--> left = emsa_pkcs1_v15.encode(msg.encode(), 256)
--> left = bytes_to_long(left)
Our job is to find and make sure that:en
left ≡ s^e (mod n)
To make it simpler, we choose:e = 1
left ≡ s (mod n)
--> left = s - k*n
--> n = s - left #Viết thế này cho dễ vì s > left
At this point, we can immediately choose the satisfying value. Note, we should add a line so that the equation is definitely correct because the sign above is an inference. Done .k = 1 assert
Flag: crypto{dupl1c4t3_s1gn4tur3_k3y_s3l3ct10n}
