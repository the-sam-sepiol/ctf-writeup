# Writeup for FSU CTF CIS4930 Challenges

---

## 64 Bit Safe

**Flag:** `fsuCTF{00p5!_1_473_4ll_7h3_b4c0n}`

### Steps

1.	Open Ciphertext
2.	Read hint, understand an 8-byte key is required. Also understand that it is a cyclic XOR problem
3.	XOR first 7 bytes of the ciphertext with the first 7 known letters of the flag: fsuCTF{
4.	Find missing last byte (letter) needed for the cyclic XOR
5.	Write python script to automate XOR
6.	Obtain flag


---

### Detailed Explanation (Including Screenshots)

The first thing I did was open the ciphertext:
It gives this string:

`161b1031353e125d401850533e493659475b3a460d14365a185b3a10551b59030d`

From the hint I gathered that this is a cyclic XOR problem, so I can likely find the key by XORing the first 8 bytes with what I know from the flag.

We know the flag is in the format fsuCTF{…}, since I don’t know what’s inside, I can use the beginning: fsuCTF{ to find it:

The ASCII codes are:
```bash
f: 0x66
s: 0x73
u: 0x75
C: 0x43
T: 0x54
F: 0x46
{: 0x7b
```

I can XOR these with the first bytes of the ciphertext:
```bash
0x16 XOR 0x66 = p
0x1b XOR 0x73 = h
0x10 XOR 0x75 = e
0x31 XOR 0x43 = r
0x35 XOR 0x54 = a
0x3e XOR 0x46 = x
0x12 XOR 0x7b = i
```


I wrote a python script that does the cyclic XOR with the key “pheraxi”

[![image.png](https://i.postimg.cc/pXmcHYRV/image.png)](https://postimg.cc/2Vf7dW1g)

Output:

[![image.png](https://i.postimg.cc/W4v5gBrt/image.png)](https://postimg.cc/gxgyPBgP)

I’m obviously missing the last byte of the key, so I modified my script to cycle the last letter from a-z until I find the flag:

[![image.png](https://i.postimg.cc/LX4v0jxp/image.png)](https://postimg.cc/QBPpFKL4)

Output:

[![image.png](https://i.postimg.cc/L4bvNYfc/image.png)](https://postimg.cc/nMqvLL32)

It looks like m was the missing character giving the key “pheraxim”
The flag is `fsuCTF{00p5!_1_473_4ll_7h3_b4c0n}`

I submitted and verified the flag
