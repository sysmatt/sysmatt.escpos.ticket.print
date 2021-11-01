# sysmatt.escpos.ticket.print
A python3 program to format STDIN into a ticket on a thermal receipt printer that understands ESC/POS.  




HELP OUTPUT
===========

usage: sysmatt.escpos.ticket.print [-h] [-v] [-D] [-l LOGO]
                                   [--logomax LOGOMAX] [-p PROFILE] [-d DEST]
                                   [-t TRAILER] [-T TSIZE] [-S SIZE]
                                   [-P PIXWIDTH] [-H HEAD] [-c] [-b] [-C]
                                   [-e EJECT] [-R REMOTE] [--image IMAGE]
                                   [--qrsize QRSIZE] [--qrdata QRDATA]
                                   [--dummy] [--impl IMPL]
                                   title

positional arguments:
  title                 the title of the ticket

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         increase output verbosity
  -D, --debug           enable debugging output
  -l LOGO, --logo LOGO  image file to print at top of ticket
  --logomax LOGOMAX     resize logo to max pixels in either direction
  -p PROFILE, --profile PROFILE
                        specify the escpos printer profile... In hindsight
                        this doesnt seem to work with anything... Go figure
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
  -R REMOTE, --remote REMOTE
                        remote print spooler
  --image IMAGE         specify image files to print after any body text. Can
                        be repeated. Images are resized and rotated.
  --qrsize QRSIZE       specify the size for qr barcodes, 1-16, default=3
  --qrdata QRDATA       print a qr barcode at the botton with this data
  --dummy               do everything except printing
  --impl IMPL           graphics implementation to use, one of:
                        bitImageRaster, graphics, bitImageColumn.
                        Default=bitImageColumn ... Note: bitImageRaster works
                        best with Citizen CT-S310, graphics mode seems to work
                        well with EPSON, the crappy generic 58mm unit only
                        works with bitImageColumn -- YMMV be prepared to waste
                        a lot of paper.
