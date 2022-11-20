# PingPongPoof

![PingPongPoof challenge description](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/pingpongpoof.png "PingPongPoof challenge description")

This week-end I participate to the GreHack 2022. It's my first CTF, so it's my first writeup.

It was in the forensics category. The competences needed were Wireshark and a scripting language.

First, I opened the file with Wireshark. I quickly saw that a pptx has been exchanged during the capture. So I exported it.
![Capture in wireshark](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-03-11.png "Capture in wireshark")
![Extracting the pptx](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-06-53.png "Extracting the pptx")
The second thing we can remark is that there are a lot of ICMP packets and that the challenge is named *PingPongPoof*.

After opening the slideshow, I was able to see 5 slides. It was screenshots of Wikipedia pages about Base64, ROT13, Keepass, ICMP and QR codes. I understood that it was clues about what to look at in the challenge.
![The extracted slide](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-07-23.png "The extracted slide")

When looking at the QR code on the fifth slide, I realized that the image was cut, so I expanded it and I scanned the QR code. It gave me a string encoded in base64, so I decoded it (it is possible to use linux command base64 or CyberChef). I made the link with the slides and undertsood that it was a Keepass master password.
![The QR code slide](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-07-45.png "The QR code slide")
![Decoding the QR code](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-09-21.png "Decoding the QR code")
![Decoding the base64](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-11-31.png "Decoding the base64")

Then I went back on the capture to look at those ICMP packets. I knew about the Ping of Death attack, so I decided to check the Data bytes in the ICMP. I found out that there were unusual data on some packets, so I decided to extract all the ICMP packets in json with TShark. With this json, I could use a python script to remove normal packets and keep suspicious packets.
![Suspicious ICMP](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-15-27.png "Suspicious ICMP")
![The script to extract the icmp data](https://github.com/Fireye025/Writeups/blob/main/GreHack_2022/Screenshot_from_2022-11-20_17-16-27.png "The script to extract the icmp data")

I put them in the right order and I save the output as a kdbx file. Then I just had to open it with Keepass and the password found before.
The flag was there as a password.
