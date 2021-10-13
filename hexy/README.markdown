First, try to unzip:
```bash
$ unzip hexy.zip
Archive:  hexy.zip
file #1:  bad zipfile offset (local header sig):  0
```

Try to get more info:
```bash
$ zipinfo hexy.zip
Zip file size: 194 bytes, number of entries: 1
-rw-a--     6.3 fat       40 Bx stor 21-Oct-11 11:09 flag
1 file, 40 bytes uncompressed, 40 bytes compressed:  0.0%

$ zipdetails hexy.zip
Unexpecded END at offset 00000000, value 04034B70
Done
```

So, seems like the beginning of the file is goofed up.  What
does the file look like?
```bash
$ hexdump -C hexy.zip

00000000  70 4b 03 04 14 00 01 00  00 00 24 59 4b 53 56 af  |pK........$YKSV.|
00000010  4d d4 34 00 00 00 28 00  00 00 04 00 00 00 66 6c  |M.4...(.......fl|
00000020  61 67 12 d6 6e 0d 7b 1e  c0 42 14 f9 10 3a ec 87  |ag..n.{..B...:..|
00000030  f1 d9 06 1b aa 49 eb d9  79 8a 39 94 d3 00 60 4a  |.....I..y.9...`J|
00000040  94 e5 53 2f 54 75 ce 76  58 62 a2 57 5e ee b7 b2  |..S/Tu.vXb.W^...|
00000050  b4 56 c6 19 3c 55 50 4b  01 02 3f 00 14 00 01 00  |.V..<UPK..?.....|
00000060  00 00 24 59 4b 53 56 af  4d d4 34 00 00 00 28 00  |..$YKSV.M.4...(.|
00000070  00 00 04 00 24 00 00 00  00 00 00 00 20 00 00 00  |....$....... ...|
00000080  00 00 00 00 66 6c 61 67  0a 00 20 00 00 00 00 00  |....flag.. .....|
00000090  01 00 18 00 67 b1 a8 ef  b1 be d7 01 67 4c a9 ef  |....g.......gL..|
000000a0  b1 be d7 01 07 9e a8 ef  b1 be d7 01 50 4b 05 06  |............PK..|
000000b0  00 00 00 00 01 00 01 00  56 00 00 00 56 00 00 00  |........V...V...|
000000c0  00 00                                             |..|
000000c2
```

But... what should a ZIP file look like?  Check file structure:
https://en.wikipedia.org/wiki/ZIP_(file_format)
https://users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html

OK, file should start with 'PK' instead of 'pK'.  Edit in vim. Retry:
```bash
$ unzip hexy.zip
Archive:  edithexy.zip
[edithexy.zip] flag password:
```

IDK the password, there are no obvious hints.  Send it to john the ripper...
