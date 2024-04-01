- Solves: 387
- Points: 100
## Description
Magical contracts are hard. Occasionally, you sign with the flag instead of your name. It happens.
## Contents
- [contract.pdf](media/contracts/contract.pdf) - appears to be some fictional contract between two parties. 
## Methodology
At the end of the PDF, we can see two signatures:
![[media/contracts/signatures.png]]

Upon right-clicking on them in the PDF, we can see that they are actually images:
![[media/contracts/actually-images.png]]

Let's save both signatures as images, and examine them using `exiftool`.
![[media/contracts/sign1-exif.png]]
![[media/contracts/sign2-exif.png]]
That doesn't yield anything useful, which means the flag is not hidden inside the signatures, but somewhere else.

A tool called `pdftohtml` allows us to break down the PDF into all its constituent parts:
![[media/contracts/pdftohtml.png]]
Let's examine the directory now:
![[media/contracts/directory.png]]
We can see a new image that we haven't been able to see before - `contract-3_3.png`. Let's take a closer look:
![[media/contracts/flag.png]]
And indeed, it turns out to be the flag!