# Hyprshot-ocr

Hyprshot-ocr is a fork of hyprshot, an utility to easily take screenshots in Hyprland, that adds easyocr text recognition option on top and auto-labeling based on hyprctl client title/class 

It allows taking screenshots of windows, regions and monitors which are saved to a folder of your choosing and copied to your clipboard. It can also OCR a selected region and copy the extracted text to your clipboard.

## Installation

### Dependencies

- hyprland (this one should be obvious)
- jq (to parse and manipulate json)
- grim (to take the screenshot)
- slurp (to select what to screenshot)
- wl-clipboard (to copy screenshot to clipboard)
- libnotify (to get notified when a screenshot is saved)

### Optional Dependencies

- hyprpicker (to freeze the screen contents with the `--freeze` flag)
- easyocr for Python together with `python3` (for the `ocr` mode)

### Manual

To install manually, simply clone this repo and copy/symlink the `hyprshot` script to a folder in your `PATH`:

```bash
$ git clone https://github.com/makerze/Hyprshot-ocr.git Hyprshot
$ ln -s $(pwd)/Hyprshot/hyprshot $HOME/.local/bin
$ chmod +x Hyprshot/hyprshot
```

## Usage

You can get help on how to use hyprshot-ocr by executing:

```bash
$ hyprshot -h
```

The simplest usage of Hyprshot-ocr is executing it with one of the available modes.

For example, to screenshot an open window:

```bash
$ hyprshot -m window
```

You can also skip saving the screenshot to a file, copying it only to the clipboard:

```bash
$ hyprshot -m output --clipboard-only
```

To append a descriptive label to the saved filename:

```bash
$ hyprshot -m window --label
```

This keeps the usual timestamped filename and adds a context label such as the window title or visible client classes before the file extension.

To extract text from a selected region and copy it to the clipboard:

```bash
$ hyprshot -m ocr
```

## Configuration

You can add the various modes as keybindings in your Hyprland config like so:

```
# ~/.config/hypr/hyprland.conf

...

# Screenshot a window
bind = $mainMod, PRINT, exec, hyprshot -m window
# Screenshot a window with a label in the filename
bind = $mainMod SHIFT, PRINT, exec, hyprshot -m window --label
# Screenshot a monitor
bind = , PRINT, exec, hyprshot -m output
# Screenshot a region
bind = $shiftMod, PRINT, exec, hyprshot -m region
# OCR a region
bind = $ctrlMod, PRINT, exec, hyprshot -m ocr
```

This would allow you to:

Take a screenshot of a window by using `MOD + PrintScr`

Take a screenshot of a window with a labeled filename by using `MOD + SHIFT + PrintScr`

Take a screenshot of a monitor by using `PrintScr`

Take a screenshot of a region by using `MOD + Shift + PrintScr`

Extract text from a region by using `CTRL + PrintScr`

## Save location

You can choose which directory Hyprshot will save screenshots in by setting an `HYPRSHOT_DIR` environment variable to your preferred location.

If `HYPRSHOT_DIR` is not set, Hyprshot will attempt to save to `XDG_PICTURES_DIR` and will further fallback to your home directory if this is also not available.

## Filename labels

When you pass `--label`, Hyprshot appends an extra label to the filename:

- `window`: the captured client title, shortened and sanitized for filenames
- `output`: all visible client classes on the captured output, deduplicated and shortened
- `region`: the title of the client under the selected region, using the region center as the primary match

## OCR language

`hyprshot -m ocr` defaults to EasyOCR's `en` language model. You can override that by setting `HYPRSHOT_OCR_LANGS` to a comma-separated list such as:

```bash
HYPRSHOT_OCR_LANGS=en,cn hyprshot -m ocr
```
