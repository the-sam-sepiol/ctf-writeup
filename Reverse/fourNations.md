# Writeup for FSU CTF CIS4930 Challenges

---

## Four Nations

**Flag:** `fsuCTF{Th3r3_1s_n0_w4r_1n_B4_S1ng_S3}`

### Steps

1.	Reviewed the decompiled binary to understand that it performs four elemental tests by decrypting encrypted scrolls using XOR with a key.
2.	Identified that each decryption key is chosen by taking the corresponding verification function’s return value modulo 100 and using that index on a fixed keys array.
3.	Computed the keys for each test
4.	Earth: 40→key 5; 
5.	Fire: 777→key –70; 
6.	Water: 326→key –81; 
7.	Air: 110→key –98).
8.	Decrypted each encrypted scroll by XORing every character with its respective key.
9.	Concatenated the decrypted parts and appended the closing brace to form the final flag

---

### Detailed Explanation (Including Screenshots)

For this problem we are given a binary file. I put the binary file into Ghidra to analyze it:

The code shows that the program conducts four elemental tests:
Earth, Fire, Water, and Air
and that each test involves decrypting an encrypted “scroll.” 

I noted that every decryption routine takes an encrypted string and XORs each of its bytes with a key obtained from a fixed keys array. 
[![image.png](https://i.postimg.cc/t4V7tvPx/image.png)](https://postimg.cc/9DCWcpLm)

Next, I examined how the decryption key is chosen. The program calls a verification function (such as `verify_earth`) for each test, and then computes the key by taking the returned value modulo 100 and using that as an index into the keys array. This means the key is calculated using the formula: 

`key = keys[verify_function_return % 100]`

[![image.png](https://i.postimg.cc/J0nzypMt/image.png)](https://postimg.cc/MMCSNbc8)

Using the information provided by the decompiled code, I then computed the decryption key for each test.

[![image.png](https://i.postimg.cc/y89YSg4h/image.png)](https://postimg.cc/jDxrpjjC)

For the Earth test, `verify_earth` returns `40 (0x28)`, so the key is `keys[40]` which equals `05` from our keys array

[![image.png](https://i.postimg.cc/YSMkGdxk/image.png)](https://postimg.cc/TK7BBqSH)
[![image.png](https://i.postimg.cc/4yDRJP3D/image.png)](https://postimg.cc/D4PM6qL6)

I got the encrypted “scroll” from the data section for earth and XORed it with `05`

[![image.png](https://i.postimg.cc/bJ5KTg1Z/image.png)](https://postimg.cc/vgWq8Wqy)

This gives the first part of the flag `fsuCTF{Th`

[![image.png](https://i.postimg.cc/Ls89QygD/image.png)](https://postimg.cc/xXBVc3RN)

For the Fire test, `verify_fire` returns `777 (0x309)`, and `777 mod 100` gives `77`, so the key is `keys[77] `
which equals `ba` in our keys array

[![image.png](https://i.postimg.cc/L8NRPzQ1/image.png)](https://postimg.cc/9DRv5qhm)
[![image.png](https://i.postimg.cc/xTPSrv24/image.png)](https://postimg.cc/N5LnmrL6)

This gives the next part of the flag: `3r3_1s_n0`

[![image.png](https://i.postimg.cc/BbGRk6w3/image.png)](https://postimg.cc/0MctJ8sX)
I then XORed it with its encrypted string:

[![image.png](https://i.postimg.cc/1XPYMdf5/image.png)](https://postimg.cc/0MWfycYL)
[![image.png](https://i.postimg.cc/4NfS9jDW/image.png)](https://postimg.cc/jC1cV3df)

This gives us the next part: `_w4r_1n_B`

[![image.png](https://i.postimg.cc/TPD7xxb3/image.png)](https://postimg.cc/R3vQ1jGk)

And for the Air test, `verify_air` returns `110 (0x63)` (resulting in `keys[10] = 9e`).

[![image.png](https://i.postimg.cc/mgcV348y/image.png)](https://postimg.cc/bZhQ87XZ)

I XORed it with its string and got:
 
[![image.png](https://i.postimg.cc/ZYVL1nQL/image.png)](https://postimg.cc/Th5mbdfy)

With this we can now compile the entire flag:
`fsuCTF{Th3r3_1s_n0_w4r_1n_B4_S1ng_S3}`
I submitted and verified the flag

 
