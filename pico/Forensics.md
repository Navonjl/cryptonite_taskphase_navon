# trivial flag transfer protocol

**Flag:** `picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}`

Aperisolve goes brrr.

- ``Wireshark time``

Running ``strings file.pcap | grep pico`` turns up nothing, so use the proper tools -
Pcap file ? wireshark it is.

- All I see is FTP

TFTP ? What's that ? probably some weird sorta FTP. Wireshark shows a file being transfered, so google around a bit and find out how to export the files.
Cat the instruction.txt file and throw it into my beloved cyberchef.

Cyberchef did _not_ cook this time.

Seeing the lack of spacing and ALL CAPS, I decided to tackle this later.

attack the deb file and somehow extract it (remember kids - don't install random software),
and it's... steghide ? uses the debian email for authorship, so  probably the offical one too ?

anyways, images go direct to aperi.

throw it in and steghide _cannot_ find anything. but look - someone already has used a password ( I <3 aperi).
We try that, and boom - flag.txt.

Ez.

Obviously the text before was just rot13, which I realised later, and that where you are supposed to get the password from, but eh - a win is a win.

# m00nwalk

**Flag:** `picoCTF{beep_boop_im_in_space}`

Seeing the size of the file, it's obvious that it's an sstv file (did one a few days ago). I have a setup, involving waydroid
(android _containers_ for linux), Robot66 and a loopback device to do this pretty easily..
(Qsstv dosen't work well for me, tho that might be because I compiled it with -Ofast)

Run it, and get the flag !

# tunn3l_v1s10n

**Flag:** `picoCTF{qu1t3_a_v13w_2020}`

Took me the longest time, mainly cause just fixing the file header didn't make `file` detect the image as bmp or chromium render it, but
you also had to edit the dimensions too. I was also looking for a literal `BITMAPINFOHEADER` there (like you have for headers). Anyways,
fix the meteadata length offset thigy and then the dimesnsions using hecxurse and profit.

[relavent wikipeadia link](https://en.wikipedia.org/wiki/BMP_file_format)
