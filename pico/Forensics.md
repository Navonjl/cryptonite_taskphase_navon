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
