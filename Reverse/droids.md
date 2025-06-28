# Writeup for FSU CTF CIS4930 Challenges

---

## The Bacon Thief

**Flag:** `fsuCTF{h4h4_y0u_7h0u6h7_7h15_w45_4n_4ndr01d_57Ud10_pr0813m}`

### Steps

1.	Open the droids.jar file in a java decompiler
2.	Find class files
3.	Find flagcheckin.class main function file
4.	Find function detailing flag reconstruction process and relevant files
5.	Use the Strings in the .class files to reconstruct the flag
6.	Submit and verify the flag.


---

### Detailed Explanation (Including Screenshots)

This problem gives a droids.jar file. I attempted to run the file with `java -jar`, but it was missing its main attribute. I decided to open the `droids.jar` file in my java decompiler

[![image.png](https://i.postimg.cc/PJf6swhm/image.png)](https://postimg.cc/TLSjnhKw)

This showed me all of the class files, I decided to first open the `flagcheckin.class` file.

It contained the static void main, so it is the main class in the file. 

[![image.png](https://i.postimg.cc/W3YZzFgm/image.png)](https://postimg.cc/G8YtSpM9)

The `reconstructFlag()` looked like it could be my answer. It shows the base64in file contains a base64 string that is decoded:

[![image.png](https://i.postimg.cc/rybWL48M/image.png)](https://postimg.cc/t7373YZM)

It does, I put it into CyberChef:
[![image.png](https://i.postimg.cc/59Z3STQB/image.png)](https://postimg.cc/dDjrqWB3)
This is plaintext and doesn’t look like what I need. I moved onto the next function `droids()`:

[![image.png](https://i.postimg.cc/BZ15bp7X/image.png)](https://postimg.cc/pyR51fHH)
This again contains instructions decoding the strings found in the .class files. It:
```bash
•	Decodes the base85 strings in base85in.class
•	XORs the string in xoragainnn.class with 9
•	Reverses the string starwars.class
•	Converts the octal in OCTALpus.class into a string
```
The first is to decode the base85 string in the `base85in.class` file:

[![image.png](https://i.postimg.cc/13RNGrB0/image.png)](https://postimg.cc/fJFLZX6k)

Because `“\\”` is an escape sequence for printing `“\”` in java, I need to remove it before I decode the string:

[![image.png](https://i.postimg.cc/dVSD8PLv/image.png)](https://postimg.cc/0zD8s4YH)
This gives the first part of the flag `“fsuCTF{h4h4_y0`, so it looks like the `reconstructFlag()` function was a decoy. Next is to get the string `xoragainn.getPart()` and XOR it with 9

[![image.png](https://i.postimg.cc/HLCRvjJt/image.png)](https://postimg.cc/sMwTvj4B)
[![image.png](https://i.postimg.cc/d0vNmXXH/image.png)](https://postimg.cc/TK7cTkTg)
This gives us part 2: `u_7h0u6h7_7h15_w4`
Next is to reverse the string in `starwars.getPart()`:
[![image.png](https://i.postimg.cc/Vk9D6g6j/image.png)](https://postimg.cc/jD5z3zy5)
[![image.png](https://i.postimg.cc/MGd50G1H/image.png)](https://postimg.cc/Z9v6447z)
This part of the string is `7Ud10_pr0813m}` This doesn’t seem like the part that follows as it doesn’t match the last and has the closing brace

Next to to decode the octal in `OCTALpus.getPart()`:
[![image.png](https://i.postimg.cc/QMxwZwbB/image.png)](https://postimg.cc/Sn3DWZHq)
[![image.png](https://i.postimg.cc/0jpcw0nR/image.png)](https://postimg.cc/SYx608HD)

This gives the 3rd and final part: `5_4ndr01d_5`

The flag together is: `fsuCTF{h4h4_y0u_7h0u6h7_7h15_w45_4n_4ndr01d_57Ud10_pr0813m}`

I submitted the flag and verified it.



 


