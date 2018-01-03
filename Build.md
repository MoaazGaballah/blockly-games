Developers of [Blockly Games](https://blockly-games.appspot.com/) need to build
the project in order to make changes.

## Get the Code

First, download the source code.  Git is the easiest:

    git clone https://github.com/google/blockly-games.git

Or use Subversion:

    svn checkout https://github.com/google/blockly-games

Or just download a ZIP:

    https://github.com/google/blockly-games/archive/master.zip

## Get the Dependencies

Enter the Blockly Games directory you just created, and get/build
the dependencies (Closure is the main one):

    cd blockly-games/
    make deps

## Build English

The next step is to build all the English versions of the applications:

    make en

Expect to see many compiler warnings, but there shouldn't be any errors.

## Build all Languages

Optionally, you might wish to build all the languages, not just English. Be
warned that this takes approximately *5 hours*, so this may be something you
should do overnight.

    make languages

## Build one Game

While developing a game, it is nice to be able to quickly recompile only the
English version of a single game. Here are the build commands for the existing
games:

    make index-en
    make puzzle-en
    make maze-en
    make bird-en
    make turtle-en
    make movie-en
    make music-en
    make pond-docs-en
    make pond-tutor-en
    make pond-duck-en

The previously mentioned `make en` is just a shortcut for all the above commands.

## Test Locally

Point a browser at `blockly-games/appengine/index.html?lang=en` and you should
see all the games.

You may notice in the browser's console that there's a failed request for
`file:///common/storage.js` on every game page.  That's normal, storage is only
available when run on App Engine.

## Debug Mode

To avoid having to recompile after small changes, visit
[/debug](https://blockly-games.appspot.com/debug)
(or `blockly-games/appengine/debug.html` if being served directly off a file system)
to switch to uncompressed mode.

Now you can just press reload in the browser to get the
latest version after most changes. Note that edits to the Closure Templates
(`*.soy`) and changes to dependencies (`goog.require`) still require a
recompile.