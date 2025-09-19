---
tags:
  - type/writing/social-media
status: published
created-date: 10-12-2024
---
## Post #6 - Reverse Engineering Encrypted Firmware

💡 Reverse Engineering Encrypted Firmware 💽  
  
A while back when I was working on Reverse Engineering Router Firmware. First step used to be acquiring the Firmware Binary, which could be done using various techniques like reading flash chip, downloading it from the internet, etc.  
  
In some cases Vendors(DLINK, Netgear, TP-Link, etc) FTP server had all the firmware update files, and the URLs of the FTP server were all over the internet. Sometime simply visiting the product page would be enough to get the firmware binary.  
  
To analyze the firmware all you do is simply run binwalk(its a popular firmware extraction tool) on the firmware update file and the file-system open for analysis. The vendors were aware about this issue and they soon started encrypting the new firmware releases making it difficult to analyse.  
  
Around that time I came across ZDI blogpost addressing the very problem. It was a very clever trick lets understand it. 🕵‍♂️  
  
See, to decrypt the firmware update file the existing firmware needs to have the decryption feature in it, only then it can process an encrypted firmware and run the rest of the updation process.  
  
So the devices which didn't have decryption mechanism has to first updated which the firmware which had the decryption feature, and all the firmware release following that release were encrypted.  
  
To reverse engineer the decryption algorithm all you have to do is figure out the which software release in the release history of the device this decryption transition was done. Next, Reverse engineer that release version (lets call its release-N) to figure-out the decryption algorithm🔑  
  
To figure decryption algorithm, you have to diffing the firmware release-N version with the release-(N-1) to see what new has been added and use the same algorithm to decrypt the following firmware updates. Usually these updates are specifically adding decryption so the changes will be small and simple to follow Reverse.  
  
⚔PWNED⚔  
  
I tried this on D-Link firmware and it worked like a hot knife through butter, I have documented this adventure in the Blogpost below. It has all the details of tools and techniques I used to solve the above mentioned problems.  
  
💻 Blog Links  
🔗 [https://lnkd.in/dYA6YY8v](https://lnkd.in/dYA6YY8v)  
🔗 [https://lnkd.in/g669BCNV](https://lnkd.in/g669BCNV)  
  
🎮 🕹  
  
I am super passionate about Reverse Engineering and Vulnerability Research and I channel this enthusiasm into my blog at [www.trainedbits.com](http://www.trainedbits.com/).  