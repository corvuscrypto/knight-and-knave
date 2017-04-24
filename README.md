# knight-and-knave
Tandem-arbitration crypto protocol to allow direct end-end secure communication in JS

# Disclaimer
This is a prototype protocol that is experimental and likely to have massive holes in it. **DO NOT TRUST IT FOR SECURE COMMUNICATION**

# Details
This project just came to me for fun to explore some crypto protocols. It's really just a toy and I do not
really believe the concept is a viable solution for in-browser direct communication browser-browser.

## Etymology
The name is derived from the fact that this protocol requires two separate servers that share no information and each has a specialized 
purpose. This gave me the name idea because it likened to a scenario in which a traveler encounters a knight and a knave. The knight tells 
the truth, while the knave tells a lie, but the traveller must find out which is the knight and which is the knave. It's actually not that 
similar but I like the name so mleh.

## Protocol
In this protocol, there are 4 parties involved. Alice, Bob, Trent, and Victor. In the protocol, Alice wishes to speak with Bob securely, but cannot use symmetric techniques normally afforded to them in other environments.

1. Alice and Bob establish communication with both Trent and Victor.
2. Trent gives Alice and Bob a long string of random data `P` which will be used to generate one time pads.
3. Alice generates the message `M` of length `n` she wished to send to Bob, then uses `n` bytes of `P` to generate `E(P[:n], M)` where `E` is a simple XOR.
4. Alice sends `E(P[:n], M)` to Victor, who sends a digital signature obtained by `S(K, E(P[:n], M))` to Bob where `S` is the HMAC function and `K` is an internal key known only to Victor.
5. Alice sends `E(P[:n], M)` to Bob.
6. Bob then sends `E(P[:n], M)` to Victor, and Victor sends back `S(K, E(P[:n], M))` to Bob to verify that the message has been unchanged.
7. Bob then decrypts the encrypted message `E(P[:n], M)` with his own copy of `P[:n]` to obtain `M`.
8. Alice and Bob never use `P[:n]` and they both splice out those bytes to prevent reuse. 

That's pretty much it. Some of these steps can be done in parallel.
