# Adwaita Icon Theme
Private UI icon set for GNOME core apps.

![Adwaita Icons](src/logo.svg)

## Bugs and Requests
If you're a core GNOME application maintainer and you have an icon need that bridges multiple components or apps, feel free to file a request in the [issue tracker](https://gitlab.gnome.org/GNOME/adwaita-icon-theme/-/issues). If you're an application developer, file a request against the [Icon Development Kit](https://gitlab.gnome.org/Teams/Design/icon-development-kit/-/issues) instead.


## Fullcolor vs symbolic
For an up to date guide on how to use and how to design GNOME style icons, see the GNOME User Interface Guidelines: [UI Icons](https://developer.gnome.org/hig/guidelines/ui-icons.html) and [App Icons](https://developer.gnome.org/hig/guidelines/app-icons.html).

## Building and Contributing to Adwaita
The icon set for system components shares the same workflow as 3rd party app symbolics, the [icon devkit](https://gitlab.gnome.org/Teams/Design/icon-development-kit).

While many legacy symbolics only live as the exported individual SVGS in `Adwaita/symbolic/`, the replacements are maintained in `src/symbolic/core.svg`. Using icon categories/contexts are [no longer used](https://gitlab.gnome.org/GNOME/adwaita-icon-theme/-/issues/73) and all new icons go into `actions`. Please refer to the [Devkit guidelines](https://gitlab.gnome.org/Teams/Design/icon-development-kit) on how to structure the metadata.

Do note that no new additions should be made unless very thoroughly discussed. *a-i-t* is the wrong way to reuse icon assets (no API, false promise of stability).

### Recoloring
The color of the icon set is defined at runtime by [gtk](https://gtk.org). Every single icon from the set is actually embedded inside an xml container that has a stylesheet overriding the colors.

There is a couple of things the icon author needs to be aware of and a few things they can make use of. The stylesheet is setting the color of the fill for all rectangles and paths. **DO NOT** leave any rectangles or paths with no fill/stroke thinking it's invisible.

[Symbolic Preview](https://flathub.org/apps/details/org.gnome.design.SymbolicPreview) doesn't convert strokes to paths yet, so you need to do it manually for now in Inkscape (`Path -> Stroke to Path`). Alternatively you can add Live path effect `join type` to your stroke and keep it non destructive.

Gtk doesn't care about the colors you define for the icon. They are recolored at runtime. If you need portions of icons to have a color, you need to include a `class` attribute to the shape or group and set it to one of the three values below. 

- `warning` - this maps to gtk `@warning_color`
- `error` - maps to `@error_color`
- `success` - maps to `@success_color`

### Cursor generation

1. Install inkscape from flathub: (using inkscape packaged from your linux distribution will not work)

        flatpak install flathub org.inkscape.Inkscape

1. Install script dependencies: (on debian, for example)

        apt install python3-pil

1. Clone this repo and cd into `adwaita-icon-theme/src/cursors`.

1. Remove the old pngs rendered: `rm -r pngs`.

1. Run `./renderpngs.py adwaita.svg` to generate the pngs from the svg file.

1. Run `./cursorgen.py` to generate cursor files in the xcursorgen format from the pngs generated in the previous step.

1. Done! Your cursor files are saved to `adwaita-icon-theme/Adwaita`.

