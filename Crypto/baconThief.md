# Writeup for FSU CTF CIS4930 Challenges

---

## The Bacon Thief

**Flag:** `fsuCTF{IMGOINGTOHIDEINASECRETPITNEARSUWANNEE}`

### Steps

1.	Open ciphertext
2.	Observe that it is Morse Code like, but has no translation to source code
3.	Clue from the name “The Bacon Thief” that it may be Bacon’s Cipher
4.	Match up the 5 symbols to the 5 letters in a Bacons Cipher
5.	Assign A to “.” And B to “-“
6.	Decode to A&B strings of length five
7.	Translate strings to Bacon’s Cipher
8.	Get plaintext and wrap in flag

---

### Detailed Explanation (Including Screenshots)

The first thing I did was open the ciphertext provided with the problem:

[![image.png](https://i.postimg.cc/7hnHY8g7/image.png)](https://postimg.cc/Zv0tVQsK)
 
My initial thought was that this was Morse code; however, all Morse code sequences are at most 4 symbols long, and all of these are 5 symbols long. I took the hint from the name and assumed it may be a Bacon’s Cipher.

Bacon’s Cipher is a steganographic method that encodes each letter of the alphabet into a unique five-symbol binary sequence, represented using two different symbols. In most cases these symbols are A and B. The decoding sheet is below:

[![image.png](https://i.postimg.cc/VLs1BhqG/image.png)](https://postimg.cc/QVzRj0n1)

`Source: Wikipedia`

There are actually two versions of the cipher, one that combines I, J and U, V, (classical) and a second that is expanded. I am using the classical version that combines the letters as it is more common:

I assigned the “.” to A, and the “-“ to B:

```bash
1. .-… → ABAAA → I
2. .-.– → ABABB → M
3. ..–. → AABBA → G
4. .–.- → ABBAB → O
5. .-… → ABAAA → I
6. .–.. → ABBAA → N
7. ..–. → AABBA → G
8. -..-. → BAABA → T
9. .–.- → ABBAB → O
10. ..— → AABBB → H
11. .-… → ABAAA → I
12. …– → AAABB → D
13. ..-.. → AABAA → E
14. .-… → ABAAA → I
15. .–.. → ABBAA → N
16. ….. → AAAAA → A
17. -…- → BAAAB → S
18. ..-.. → AABAA → E
19. …-. → AAABA → C
20. -…. → BAAAA → R
21. ..-.. → AABAA → E
22. -..-. → BAABA → T
23. .—. → ABBBA → P
24. .-… → ABAAA → I
25. -..-. → BAABA → T
26. .–.. → ABBAA → N
27. ..-.. → AABAA → E
28. ….. → AAAAA → A
29. -…. → BAAAA → R
30. -…- → BAAAB → S
31. -..– → BAABB → U
32. -.-.. → BABAA → W
33. ….. → AAAAA → A
34. .–.. → ABBAA → N
35. .–.. → ABBAA → N
36. ..-.. → AABAA → E
37. ..-.. → AABAA → E
```

This spells out the flag: `IMGOINGTOHIDEINASECRETPITNEARSUWANNEE`
I wrapped it in the `fsuCTF{}` and it gives: 
`fsuCTF{IMGOINGTOHIDEINASECRETPITNEARSUWANNEE}`

I submitted and verified the flag.

