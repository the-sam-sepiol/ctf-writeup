# Writeup for FSU CTF CIS4930 Challenges

---

## Epic Coding Bro

**Flag:** `fsuCTF{0n3_47_4_71m3}`

### Steps


1.	Connect and read the AES-ECB encrypted flag’s hex, splitting it into 16‑byte blocks.
2.	Note which blocks correspond to known characters fsuCTF{ } and mark them.
3.	For each unknown block index:
4.	Open a new connection (same AES key within that session).
5.	Re‑read the flag blocks and extract the target block.
6.	At the prompt, send one candidate character.
7.	Compare its 16‑byte ciphertext to the target block.
8.	When they match, record the character and move to the next position.
9.	Repeat until all flag characters are recovered.

---

### Detailed Explanation (Including Screenshots)

I am given a server that gives an encrypted flag and the opportunity to encrypt one more character. After entering another character, it gives the encryption and closes the connection. 

[![image.png](https://i.postimg.cc/vHXH7FcT/image.png)](https://postimg.cc/KRk2FW7h)

Every connection uses a different hash, so you cannot just reconnect and build a dictionary of the letter’s hashes:

[![image.png](https://i.postimg.cc/nzvnCNyH/image.png)](https://postimg.cc/VrLxhZsT)

We are also given a python file that contains the source code for the problem:

[![image.png](https://i.postimg.cc/ZRFhTpNn/image.png)](https://postimg.cc/ZBRQH9Vz)

It uses AES ECB for encryption and a `super_pad()` function that takes the flag from `flag.txt`, strips it, applies `super_pad()`, and prints the hex of its AES‑ECB encryption.

The vulnerability here is that AES-ECB encrypts 16-byte blocks separately and deterministically. If two plaintext blocks are identical, their ciphertext blocks will be identical. Because `super_pad()` repeats each char 16 times, each char of the flag becomes its own 16-byte plaintext block:

You can obtain the encryption of any single character under the same key once per connection, so within that connection you know the ciphertext of one block of the form `c=AES_ECB(char*16)`

Putting this together, each ciphertext block of the flag equals the encryption of 16 copies of a single flag character. If you can query the service for the encryption of the same character, you can match blocks and recover the flag.

Here is what I did to exploit this vulnerability:

Open a connection to the server. Read and parse the encrypted flag, extract the 16 byte block corresponding to the target position. We know the flag contains fsuCTF{ and }, so we can skip the query for those and move onto the next. wait for the “Encrypt one more character!” prompt, then:

```bash
1.	Loop over all printable ASCII
2.	Send one character
3.	Read its single-block ciphertext
4.	Compare it against the target block
5.	If they match, we have found the right character
6.	If they don’t match, close connection and try again
```

I made a python script that uses pwntools to automate this:

[![image.png](https://i.postimg.cc/VkcpNdYb/image.png)](https://postimg.cc/GHqq73Tc)

I ran this with the server IP and port, and was yielded this result:

[![image.png](https://i.postimg.cc/q7JF8S6J/image.png)](https://postimg.cc/xXZgQsPh)

The flag is: `fsuCTF{0n3_47_4_71m3}`
I submitted and verified the flag

