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

Seeing the size of the file, it's obvious that it's an sstv file (did one a few days ago). I have a setup, involving waydroid (android _containers_ for linux), Robot66 and a loopback device to do this pretty easily..
(Qsstv dosen't work well for me, tho that might be because I compiled it with -Ofast)

Run it, and get the flag !

# tunn3l_v1s10n

**Flag:** `picoCTF{qu1t3_a_v13w_2020}`

Took me the longest time, mainly cause just fixing the file header didn't make `file` detect the image as bmp or chromium render it, but
you also had to edit the dimensions too. I was also looking for a literal `BITMAPINFOHEADER` there (like you have for headers). Anyways,
fix the meteadata length offset thigy and then the dimesnsions using hecxurse and profit.

[relavent wikipeadia link](https://en.wikipedia.org/wiki/BMP_file_format)


# Blast from the past

**Flag:** `picoCTF{71m3_7r4v311ng_p1c7ur3_12e0c36b}`

Fixup most of the file with
```sh
exiftool -SubSecCreateDate='1970:01:01 00:00:00.001' -SubSecDateTimeOriginal='1970:01:01 00:00:00.001' -SubSecModifyDate='1970:01:01 00:00:00.001' -AllDates='1970:01:01 00:00:00.001' uwu
```

and then instead of reading up on samsung's whatever, open the file in
hexcurse and find the sus looking timestamp (`Image_UTC_Data`) and change it manually
(after a quick lookup that 30 is infact 0 for ASCII)

Look ma, no custom tools !

# Mob psycho

Binwalk the file to extract the zip - (I have a fair amount of experience with android modding, so that was obvious),
and then we enter into an elaborate patchwork of files. after some mucking around we stumble upon
```zsh
find . -type f -exec ls {} + | grep flag
```

which shows us the file `./res/color/flag.txt`. do a `cat $(!!)` and get what seems to be a hexdump.
Throw that stuff into cyberchef, click the literal magic button, and profit.

# WPA-ing Out

**Flag:** `picoCTF{mickeymouse}`

Easiest challenge. Only has 2.3k solves. Don't ask me how.

Download the given pcap file. Absloutely forgot how to use aircrack, so search it up for a minute or so. get the cmd flags as
`aircrack-ng -w ~/Documents/rockyou.txt ~/Downloads/wpa-ing_out.pcap`. Get the flag in a literal millisecond. profit.

# MSB

**Flag:**  `picoCTF{custom_d2cr0pt6d_49fbee5b}`

First of all, learn how traditional LSB steganography works. Bascially you add/subtract stuff to the lowest RGB channel
bits, so that you can recover them later. Next, since the corruption is so obvious, load the image into photopea and find out where
the corruption and therefore the data ends - at 1280 lines.

Cook up some code -

```py
from PIL import Image

image_path = "output.png"

img = Image.open(image_path)
pixels = img.load()

binary_message = ''
#for y in range(img.height):
for y in range(1280):
	print(y)
	for x in range(img.width):
		r, g, b, *a = pixels[x, y]  # Extract RGB

		# The maximum value of a channel is 255 (1 byte), so we can shift by 7 bits to get the MSB.
		binary_message += str(r >> 7)
		binary_message += str(g >> 7)
		binary_message += str(b >> 7)


# Convert binary data to text
secret_message = ''.join(chr(int(binary_message[i:i+8], 2)) for i in range(0, len(binary_message), 8))
print(secret_message)
```

But this turns out to be _ridiculously_ slow. Like would take around 10 minutes slow.

Ask chatgpt to convert it into a numpy operation and we get -

```py
import numpy as np
from PIL import Image

def extract_msb_png_numpy(image_path):
    # Open the image and convert to a NumPy array
    img = Image.open(image_path)
    img_array = np.array(img)  # Shape: (height, width, channels)

    # Limit to the first 1280 rows (similar to `for y in range(1280)`)
    img_array = img_array[:1280, :, :3]  # Use only R, G, B channels for first 1280 rows

    # Extract MSB from R, G, B channels
    msb_array = img_array >> 7  # Right shift to extract MSBs (values will be 0 or 1)
    binary_message = msb_array.flatten()  # Flatten to a 1D array

    # Convert binary array to a binary string
    binary_message_str = ''.join(map(str, binary_message))

    # Convert binary data to text (8 bits at a time)
    secret_message = ''.join(
        chr(int(binary_message_str[i:i+8], 2))
        for i in range(0, len(binary_message_str), 8)
    )

    return secret_message

# Example usage
print(extract_msb_png_numpy("output.png"))
```

And that's that.
