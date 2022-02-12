# sysmatt.escpos.ticket.print
A python3 program to format STDIN into a ticket on a thermal receipt printer that understands ESC/POS.  




# HELP OUTPUT
# ===========
```
usage: sysmatt.escpos.ticket.print [-h] [-v] [-D] [-l LOGO] [--landscape]
                                   [--logomax LOGOMAX] [-p PROFILE]
                                   [--header HEADER] [--headerttf HEADERTTF]
                                   [--titlettf TITLETTF] [--hsize HSIZE]
                                   [-d DEST] [-t TRAILER] [-T TSIZE] [-S SIZE]
                                   [-P PIXWIDTH] [-H HEAD] [-c] [-b] [-C]
                                   [-e EJECT] [--density DENSITY] [-R REMOTE]
                                   [--image IMAGE] [--bodyttf BODYTTF]
                                   [--imagesep] [--imagename]
                                   [--qrsize QRSIZE] [--qrdata QRDATA]
                                   [--dummy] [--autorotate] [--impl IMPL]
                                   title

positional arguments:
  title                 the title of the ticket

options:
  -h, --help            show this help message and exit
  -v, --verbose         increase output verbosity
  -D, --debug           enable debugging output
  -l LOGO, --logo LOGO  image file to print at top of ticket
  --landscape           rotate everthing to landscape, for example banner
                        printing (alpha), Only works with text when TTF fonts
                        are specified
  --logomax LOGOMAX     resize logo to max pixels in either direction
  -p PROFILE, --profile PROFILE
                        specify the escpos printer profile... In hindsight
                        this doesnt seem to work with anything... Go figure
  --header HEADER       prepend a super header in extra large text
  --headerttf HEADERTTF
                        re-render header text into an image using this ttf
                        font file, --hsize is ttf font size
  --titlettf TITLETTF   re-render title text into an image using this ttf font
                        file, --tsize is ttf font size
  --hsize HSIZE         text size for super header
  -d DEST, --dest DEST  specify the printer destination, default=citizen-raw
  -t TRAILER, --trailer TRAILER
                        trailer appended to the end of the ticket in small
                        text
  -T TSIZE, --tsize TSIZE
                        text size for title
  -S SIZE, --size SIZE  text size for body
  -P PIXWIDTH, --pixwidth PIXWIDTH
                        pixel width, CT-S310=576 (default), Generic 58mm=384
  -H HEAD, --head HEAD  max lines of body to print, 0=no limit (default)
  -c, --cut             cut paper after printing
  -b, --beep            beep after printing
  -C, --center          center title, default is left
  -e EJECT, --eject EJECT
                        extra newlines after print to eject page
  --density DENSITY     set print density (alpha, escpos fn=49, may not work
                        with all models)
  -R REMOTE, --remote REMOTE
                        remote print spooler
  --image IMAGE         specify image files to print after any body text. Can
                        be repeated. Images are resized and rotated.
  --bodyttf BODYTTF     re-render body text into an image using this ttf font
                        file, --size is ttf font size
  --imagesep            print a bar between images
  --imagename           print the image filename after each image
  --qrsize QRSIZE       specify the size for qr barcodes, 1-16, default=3
  --qrdata QRDATA       print a qr barcode at the botton with this data
  --dummy               do everything except printing
  --autorotate          try auto-rotating --image imputs to produce largest
                        print
  --impl IMPL           graphics implementation to use, one of:
                        bitImageRaster, graphics, bitImageColumn.
                        Default=bitImageColumn ... Note: bitImageRaster works
                        best with Citizen CT-S310, bitImageRaster or graphics
                        mode seems to work well with EPSON, the crappy generic
                        58mm unit only works with bitImageColumn -- YMMV be
                        prepared to waste a lot of paper. NOTE2: I dont know
                        anymore... Its random. it seems to depend on the
                        image. Try all of them and keep buying more paper.

Hint:  How to build python-escpos, The current version avail from PIP is out of date...

    #!/bin/bash
    apt install -y python3-pip
    apt install -y python3-pil
    apt install -y python3-numpy
    pip3 install --upgrade  pyusb
    pip3 install --upgrade  qrcode
    pip3 install --upgrade  pyserial
    pip3 install --upgrade  python-barcode

    cd /root
    git clone --recurse-submodules  https://github.com/python-escpos/python-escpos.git
    cd python-escpos/
    python3 setup.py  build
    python3 setup.py install
[end]
```
