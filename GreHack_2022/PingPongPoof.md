# PingPongPoof

This week-end I participate to the GreHack 2022. It's my first CTF, so it's my first writeup.

It was in the forensics category. The competences needed were Wireshark and a scripting language.

First, I opened the file with Wireshark. I quickly saw that a pptx has been exchanged during the capture. So I exported it.
The second thing we can remark is that there are a lot of ICMP packets and that the challenge is named *PingPongPoof*.

After opening the slideshow, I was able to see 5 slides. It was screenshots of Wikipedia pages about Base64, ROT13, Keepass, ICMP and QR codes. I understood that it was clues about what to look at in the challenge.

When looking at the QR code on the fifth slide, I realized that the image was cut, so I expanded it and I scanned the QR code. It gave me a string encoded in base64, so I decoded it (it is possible to use linux command base64 or CyberChef). I made the link with the slides and undertsood that it was a Keepass master password.

Then I went back on the capture to look at those ICMP packets, I knew about the Ping of Death attack, so I decided to check the Data bytes in the ICMP. I found out that there were unusual data on some packets, so I decided to extract all the ICMP packets in json with TShark. With this json, I could use a python script to remove normal packets and keep suspicious packets.

I put them in the right order and I save the output as a kdbx file. Then I just had to open it with Keepass and the password found before.
The flag was there as a password.
