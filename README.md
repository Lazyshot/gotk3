gotk3
=====

[![Build Status](https://travis-ci.org/andre-hub/gotk3.png?branch=master)]
(https://travis-ci.org/andre-hub/gotk3)

The gotk3 project provides Go bindings for GTK+3 and dependent
projects.  Each component is given its own subdirectory, which is used
as the import path for the package.  Partial binding support for the
following libraries is currently implemented:

  - GTK+3 (3.6 and later)
  - GDK 3 (3.6 and later)
  - GLib 2 (2.36 and later)
  - Cairo (1.10 and later)

Care has been taken for memory management to work seamlessly with Go's
garbage collector without the need to use or understand GObject's
floating references.

## Based on
  - https://github.com/conformal/gotk3
  - https://github.com/sqp/gotk3
  - https://github.com/dradtke/gotk3
  - https://github.com/MovingtoMars/gotk3
  - https://github.com/shish/gotk3
  - ....

## Sample Use

The following example can be found in `gtk/examples/simple/simple.go`.
Usage of additional features is also demonstrated in the
`gtk/examples/` directory.

```Go
package main

import (
	"github.com/lazyshot/gotk3/gtk"
	"log"
)

func main() {
	// Initialize GTK without parsing any command line arguments.
	gtk.Init(nil)

	// Create a new toplevel window, set its title, and connect it to the
	// "destroy" signal to exit the GTK main loop when it is destroyed.
	win, err := gtk.WindowNew(gtk.WINDOW_TOPLEVEL)
	if err != nil {
		log.Fatal("Unable to create window:", err)
	}
	win.SetTitle("Simple Example")
	win.Connect("destroy", func() {
		gtk.MainQuit()
	})

	// Create a new label widget to show in the window.
	l, err := gtk.LabelNew("Hello, gotk3!")
	if err != nil {
		log.Fatal("Unable to create label:", err)
	}

	// Add the label to the window.
	win.Add(l)

	// Set the default window size.
	win.SetDefaultSize(800, 600)

	// Recursively show all widgets contained in this window.
	win.ShowAll()

	// Begin executing the GTK main loop.  This blocks until
	// gtk.MainQuit() is run. 
	gtk.Main()
}
```

## Documentation

Each package's internal `go doc` style documentation can be viewed
online without installing this package by using the GoDoc site (links
to [cairo](http://godoc.org/github.com/lazyshot/gotk3/cairo),
[glib](http://godoc.org/github.com/lazyshot/gotk3/glib),
[gdk](http://godoc.org/github.com/lazyshot/gotk3/gdk), and
[gtk](http://godoc.org/github.com/lazyshot/gotk3/gtk) documentation).

You can also view the documentation locally once the package is
installed with the `godoc` tool by running `godoc -http=":6060"` and
pointing your browser to
http://localhost:6060/pkg/github.com/lazyshot/gotk3

## Installation

gotk3 currently requires GTK 3.6-3.12, GLib 2.36-2.40, and
Cairo 1.10 or 1.12.  A recent Go (1.2 or newer) is also required.

The gtk package requires the cairo, glib, and gdk packages as
dependencies, so only one `go get` is necessary for complete
installation.

The build process uses the tagging scheme gtk_MAJOR_MINOR to specify a
build targeting any particular GTK version (for example, gtk_3_10).
Building with no tags defaults to targeting the latest supported GTK
release (3.12).

To install gotk3 targeting the latest GTK version:

```bash
$ go get github.com/lazyshot/gotk3/gtk
```

On MacOS (using homebrew) you would likely specify PKG_CONFIG_PATH as such:
```bash
$ PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig:`brew --prefix gtk+3`/lib/pkgconfig go get -u -v github.com/lazyshot/gotk3/gdk
```

To install gotk3 targeting the older GTK 3.10 release:

```bash
$ go get -tags gtk_3_10 github.com/lazyshot/gotk3/gtk
```

Ubuntu 14.04 uses the GTK 3.10 release.  Also, some required packages may be
missing.  You may need to install the first or all of these packages in Ubuntu
before gotk3:

```bash
$ sudo apt-get install libgtk-3-dev
$ sudo apt-get install libcairo2-dev
$ sudo apt-get install libglib2.0-dev
```

## TODO
- Add bindings for all of GTK+
- Add tests for each implemented binding
- Add examples for intent

## License

Package gotk3 is licensed under the liberal ISC License.
