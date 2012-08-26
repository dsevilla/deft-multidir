deft-multidir
=============

Deft mode with multiple directories. This is work adapted from the
original Deft mode by Jason Blevins:
[http://jblevins.org/projects/deft/](http://jblevins.org/projects/deft/).

# Original Copyright #

    Copyright (C) 2011-2012 Jason R. Blevins <jrblevin@sdf.org>
    All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions are met:
    1. Redistributions of source code must retain the above copyright
       notice, this list of conditions and the following disclaimer.
    2. Redistributions in binary form must reproduce the above copyright
       notice, this list of conditions and the following disclaimer in the
       documentation  and/or other materials provided with the distribution.
    3. Neither the names of the copyright holders nor the names of any
       contributors may be used to endorse or promote products derived from
       this software without specific prior written permission.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
    AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
    IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
    ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
    LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
    CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
    SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
    INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
    CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
    ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
    POSSIBILITY OF SUCH DAMAGE.

    Version: 0.4
    Author: Jason R. Blevins <jrblevin@sdf.org>
    Keywords: plain text, notes, Simplenote, Notational Velocity
    URL: http://jblevins.org/projects/deft/


# Original Documentation #

Deft is an Emacs mode for quickly browsing, filtering, and editing
directories of plain text notes, inspired by Notational Velocity.
It was designed for increased productivity when writing and taking
notes by making it fast and simple to find the right file at the
right time and by automating many of the usual tasks such as
creating new files and saving files.

Deft is open source software and may be freely distributed and
modified under the BSD license.  Version 0.4 is the latest stable
version, released on December 11, 2011.  You may download it
directly here:

  * [deft.el](http://jblevins.org/projects/deft/deft.el)

To follow or contribute to Deft development, you can either
[browse](http://jblevins.org/git/deft.git) or clone the Git
repository:

    git clone git://jblevins.org/git/deft.git

![File Browser](http://jblevins.org/projects/deft/browser.png)

The Deft buffer is simply a file browser which lists the titles of
all text files in the Deft directory followed by short summaries
and last modified times.  The title is taken to be the first line
of the file and the summary is extracted from the text that
follows.  Files are sorted in terms of the last modified date, from
newest to oldest.

All Deft files or notes are simple plain text files where the first
line contains a title.  As an example, the following directory
structure generated the screenshot above.

    % ls ~/.deft
    about.txt    browser.txt     directory.txt   operations.txt
    ack.txt      completion.txt  extensions.txt  text-mode.txt
    binding.txt  creation.txt    filtering.txt

    % cat ~/.deft/about.txt
    About

    An Emacs mode for slicing and dicing plain text files.

![Filtering](http://jblevins.org/projects/deft/filter.png)

Deft's primary operation is searching and filtering.  The list of
files can be limited or filtered using a search string, which will
match both the title and the body text.  To initiate a filter,
simply start typing.  Filtering happens on the fly.  As you type,
the file browser is updated to include only files that match the
current string.

To open the first matching file, simply press `RET`.  If no files
match your search string, pressing `RET` will create a new file
using the string as the title.  This is a very fast way to start
writing new notes.  The filename will be generated automatically.
If you prefer to provide a specific filename, use `C-RET` instead.

To open files other than the first match, navigate up and down
using `C-p` and `C-n` and press `RET` on the file you want to open.

Press `C-c C-c` to clear the filter string and display all files
and `C-c C-g` to refresh the file browser using the current filter
string.

By default, Deft filters files in incremental string search mode,
where "search string" will match all files containing both "search"
and "string" in any order.  Alternatively, Deft supports direct
regex filtering.  Pressing `C-c C-t` will toggle between these two
modes of operation.  Regex mode is indicated by an "R" in the mode
line.

Static filtering is also possible by pressing `C-c C-l`.  This is
sometimes useful on its own, and it may be preferable in some
situations, such as over slow connections or on older systems,
where interactive filtering performance is poor.

Common file operations can also be carried out from within Deft.
Files can be renamed using `C-c C-r` or deleted using `C-c C-d`.
New files can also be created using `C-c C-n` for quick creation or
`C-c C-m` for a filename prompt.  You can leave Deft at any time
with `C-c C-q`.

Files opened with deft are automatically saved after Emacs has been
idle for a customizable number of seconds.  This value is a floating
point number given by `deft-auto-save-interval' (default: 1.0).

Getting Started
---------------

To start using it, place it somewhere in your Emacs load-path and
add the line

    (require 'deft)

in your `.emacs` file.  Then run `M-x deft` to start.  It is useful
to create a global keybinding for the `deft` function (e.g., a
function key) to start it quickly (see below for details).

When you first run Deft, it will complain that it cannot find the
`~/.deft` directory.  You can either create a symbolic link to
another directory where you keep your notes or run `M-x deft-setup`
to create the `~/.deft` directory automatically.

One useful way to use Deft is to keep a directory of notes in a
Dropbox folder.  This can be used with other applications and
mobile devices, for example, Notational Velocity or Simplenote
on OS X, Elements on iOS, or Epistle on Android.

Customization
-------------

Customize the `deft` group to change the functionality.

By default, Deft looks for notes by searching for files with the
extension `.txt` in the `~/.deft` directory.  You can customize
both the file extension and the Deft directory by running
`M-x customize-group` and typing `deft`.  Alternatively, you can
configure them in your `.emacs` file:

    (setq deft-extension "txt")
    (setq deft-directory "~/Dropbox/notes")

You can also customize the major mode that Deft uses to edit files,
either through `M-x customize-group` or by adding something like
the following to your `.emacs` file:

    (setq deft-text-mode 'markdown-mode)

Note that the mode need not be a traditional text mode.  If you
prefer to write notes as LaTeX fragments, for example, you could
set `deft-extension' to "tex" and `deft-text-mode' to `latex-mode'.

If you prefer `org-mode', then simply use

    (setq deft-extension "org")
    (setq deft-text-mode 'org-mode)

For compatibility with other applications which take the title from
the filename, rather than from first line of the file, set the
`deft-use-filename-as-title` flag to a non-nil value.  This also
changes the default behavior for creating new files when the filter
is non-empty: the filter string will be used as the new filename
rather than inserted into the new file.  To enable this
functionality, simply add the following to your `.emacs` file:

    (setq deft-use-filename-as-title t)

You can easily set up a global keyboard binding for Deft.  For
example, to bind it to F8, add the following code to your `.emacs`
file:

    (global-set-key [f8] 'deft)

The faces used for highlighting various parts of the screen can
also be customized.  By default, these faces inherit their
properties from the standard font-lock faces defined by your current
color theme.

Incremental string search is the default method of filtering on
startup, but you can set `deft-incremental-search' to nil to make
regex search the default.

The title of each file is taken to be the first line of the file,
with certain characters removed from the beginning (hash
characters, as used in Markdown headers, and asterisks, as in Org
Mode headers).  The substrings to remove are specified in
`deft-strip-title-regex`.

More generally, the title post-processing function itself can be
customized by setting `deft-parse-title-function`, which accepts
the first line of the file as an argument and returns the parsed
title to display in the file browser.  The default function is
`deft-strip-title`, which removes all occurrences of
`deft-strip-title-regex` as described above.

Acknowledgments
---------------

Thanks to Konstantinos Efstathiou for writing simplnote.el, from
which I borrowed liberally, and to Zachary Schneirov for writing
Notational Velocity, which I have never had the pleasure of using,
but whose functionality and spirit I wanted to bring to other
platforms, such as Linux, via Emacs.

History
-------

Version 0.4 (2011-12-11):

* Improved filtering performance.
* Optionally take title from filename instead of first line of the
  contents (see `deft-use-filename-as-title`).
* Dynamically resize width to fit the entire window.
* Customisable time format (see `deft-time-format`).
* Handle `deft-directory` properly with or without a trailing slash.

Version 0.3 (2011-09-11):

* Internationalization: support filtering with multibyte characters.

Version 0.2 (2011-08-22):

* Match filenames when filtering.
* Automatically save opened files (optional).
* Address some byte-compilation warnings.

Deft was originally written by Jason Blevins.
The initial version, 0.1, was released on August 6, 2011.

# Multiple Directories #

TBD
