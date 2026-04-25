# sysmatt.escpos.ticket

A command-line Python utility for printing formatted tickets to ESC/POS-compatible thermal printers. Supports logos, headers, titles, body text, images, QR codes, and barcodes — with optional TTF font rendering, landscape mode, and remote spooling.

---

## Requirements

- Python 3.8+
- [`python-escpos`](https://github.com/python-escpos/python-escpos) (see install note below)
- Pillow
- NumPy

### Installing dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

If the PyPI release of `python-escpos` is outdated, install it from source after the above:

```bash
pip install pyusb qrcode pyserial python-barcode
git clone --recurse-submodules https://github.com/python-escpos/python-escpos.git
cd python-escpos && pip install .
```

---

## Usage

```
sysmatt.escpos.ticket.print [OPTIONS] TITLE
```

Body text is read from **stdin**.

### Minimal example

```bash
echo "Your order is ready." | ./sysmatt.escpos.ticket.print "Order #42"
```

### Full example

```bash
echo -e "Line 1\nLine 2\nLine 3" | ./sysmatt.escpos.ticket.print \
    --logo /path/to/logo.png \
    --header "ACME CORP" \
    --dest my-printer \
    --cut \
    --qrdata "https://example.com" \
    --trailer "Thank you!" \
    "Receipt #1234"
```

---

## Options

### General

| Flag | Default | Description |
|------|---------|-------------|
| `TITLE` | `Ticket` | The ticket title (required, positional) |
| `-v`, `--verbose` | off | Increase output verbosity |
| `-D`, `--debug` | off | Enable debug output and save intermediate images to `/tmp/` |
| `--dummy` | off | Run everything except sending output to the printer |

### Printer destination

| Flag | Default | Description |
|------|---------|-------------|
| `-d`, `--dest` | `citizen-raw` | CUPS printer destination name |
| `-R`, `--remote` | _(local)_ | Remote print spooler host (`lp -h HOST`) |
| `-p`, `--profile` | _(none)_ | ESC/POS printer profile name |
| `-P`, `--pixwidth` | `576` | Printable pixel width — CT-S310: `576`, generic 58 mm: `384` |
| `--impl` | `bitImageColumn` | Graphics implementation: `bitImageRaster`, `graphics`, or `bitImageColumn` — see [Choosing `--impl`](#choosing---impl) |
| `--density` | _(unset)_ | Print density (alpha; ESC/POS fn=49, not supported on all models) |

### Layout & text sizing

| Flag | Default | Description |
|------|---------|-------------|
| `-C`, `--center` | off | Centre-align the title (default: left) |
| `-S`, `--size` | `1` | Body text size multiplier |
| `-T`, `--tsize` | `2` | Title text size multiplier |
| `--hsize` | `4` | Header text size multiplier |
| `-e`, `--eject` | `1` | Number of blank lines printed after the ticket to advance paper |
| `--landscape` | off | Rotate all output 90° for banner/landscape printing (alpha) |

### Logo

| Flag | Default | Description |
|------|---------|-------------|
| `-l`, `--logo` | _(none)_ | Image file to print at the top of the ticket |
| `--logomax` | _(auto)_ | Resize logo so neither dimension exceeds this many pixels |

### Header & title fonts

| Flag | Default | Description |
|------|---------|-------------|
| `--header` | _(none)_ | Super-header text printed above the title |
| `--headerttf` | _(none)_ | TTF font file — renders the header as an image instead of native ESC/POS text |
| `--titlettf` | _(none)_ | TTF font file — renders the title as an image |

### Body text fonts

| Flag | Default | Description |
|------|---------|-------------|
| `--bodyttf` | _(none)_ | TTF font file — renders the entire body as an image |
| `--bodyttfwrap` | `0` | Force word-wrap at N characters when rendering body with TTF (0 = auto) |
| `-H`, `--head` | `0` | Print only the first N lines of body text (0 = no limit) |

### Images

| Flag | Default | Description |
|------|---------|-------------|
| `--image FILE` | _(none)_ | Image file to print after the body (repeatable) |
| `--imagesep` | off | Print a separator bar between images |
| `--imagename` | off | Print the filename below each image |
| `--autorotate` | off | Auto-rotate landscape images to maximise print size |

### QR codes & barcodes

| Flag | Default | Description |
|------|---------|-------------|
| `--qrdata DATA` | _(none)_ | Print a QR code at the bottom (repeatable) |
| `--qrsize` | `3` | QR code size, 1–16 |

Inline **CODE39 barcodes** can be embedded in body text by surrounding a word with asterisks: `*BARCODE*`. Words longer than 20 characters are ignored.

### Finishing

| Flag | Default | Description |
|------|---------|-------------|
| `-c`, `--cut` | off | Partial-cut the paper after printing |
| `-b`, `--beep` | off | Trigger the printer buzzer after printing |
| `-t`, `--trailer` | _(none)_ | Small text appended after the timestamp in the ticket footer |

---

## Ticket structure

A printed ticket is laid out in this order:

```
┌─────────────────────────────┐
│  Logo (optional)            │
├─────────────────────────────┤  ← separator bar
│  Header (optional)          │
├─────────────────────────────┤  ← separator bar
│  Title                      │
├─────────────────────────────┤  ← separator bar
│  Body text (stdin)          │
│  Extra images (optional)    │
│  QR codes (optional)        │
├─────────────────────────────┤  ← separator bar
│  [timestamp] trailer        │
│                             │  ← eject lines
└─────────────────────────────┘
```

---

## Choosing `--impl`

The graphics implementation flag controls how images are sent to the printer. There is no universal default — experiment with your hardware:

| Value | Works well with |
|-------|----------------|
| `bitImageRaster` | Citizen CT-S310, some EPSON models |
| `graphics` | EPSON |
| `bitImageColumn` | Generic 58 mm printers (default) |

---

## TTF font rendering

When `--bodyttf`, `--headerttf`, or `--titlettf` are specified, text is rasterised into an image using Pillow before being sent to the printer. This allows custom fonts regardless of what the printer natively supports. The image is cropped tightly to the text content and scaled to fit `--pixwidth`.

Use `--debug` to save intermediate render images to `/tmp/` for inspection.

---

## Known limitations

- `--landscape` is **alpha** quality and only works reliably with TTF-rendered text.
- `--density` is **alpha** and may have no effect on some printer models.
- Image output quality varies significantly by printer and `--impl` setting.

---

## License

See the original project repository for license information.
