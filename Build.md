Blockly Games needs to be built before it may be run.  This is in contrast with [Blockly](https://github.com/google/blockly/) which includes compiled binaries checked into the repository.  

## Get the Code

First, download the source code.  Go to this project's [home page](https://github.com/google/blockly-games) to get a copy of the code using Git, Subversion, or a ZIP.

## Get the Dependencies

Enter the Blockly Games directory you just created, and attempt to get and build the dependencies (Closure is the main one):

    cd blockly-games/
    make deps

If this works, great!  But it will probably die with this error:

    [javac] Compiling 375 source files to /home/fraser/blockly-games/closure-templates/build/classes
    [javac] warning: [options] bootstrap class path not set in conjunction with -source 1.6

In this case you need to edit `closure-templates/build.xml` and change all four instances of "1.6" to "1.7".  Here is the patch:

    --- build.xml	(revision 28)
    +++ build.xml	(working copy)
    @@ -75,8 +75,8 @@
         <!-- Java compilation. -->
         <javac srcdir="${java.src.dir}:${build.genfiles.dir}"
                destdir="${build.classes.dir}"
    -           source="1.6"
    -           target="1.6"
    +           source="1.7"
    +           target="1.7"
                includeAntRuntime="true"
                debug="${includeDebugInfo}">
           <classpath refid="classpath.path" />
    @@ -664,8 +664,8 @@
         <mkdir dir="${build.testclasses.dir}" />
         <javac srcdir="${java.tests.dir}"
                destdir="${build.testclasses.dir}"
    -           source="1.6"
    -           target="1.6"
    +           source="1.7"
    +           target="1.7"
                includeAntRuntime="false"
                debug="${includeDebugInfo}">
           <classpath refid="classpath.path" />

Rerun `make deps`, and everything should build properly.

## Build English

The next step is to build all the English versions of the applications:

    make en

## Build all Languages

Optionally, you might wish to build all the languages, not just English.  Be warned that this takes approximately *5 hours*, so this may be something you should do overnight.

    make languages

## Build one Game

While developing a game, it is nice to be able to quickly recompile only the English version of a single game.  Here are the build commands for the existing games:

    make index-en
    make puzzle-en
    make maze-en
    make bird-en
    make turtle-en
    make movie-en
    make pond-docs-en
    make pond-basic-en
    make pond-advanced-en

The previously mentioned `make en` is just a shortcut for all the above commands.

## Test Locally

Point a browser at `blockly-games/appengine/index.html?lang=en` and you should see all the games.

You may notice in the browser's console that there's a failed request for `file:///common/storage.js` on every game page.  That's normal, storage is only available when run on App Engine.

## Debug Mode

To avoid having to recompile after small changes, edit `appengine/common/boot.js` and change `compressed.js` to `uncompressed.js` near the end of that file.  Now you can just press reload in the browser to get the latest version after most changes.  Note that edits to the Closure Templates (`*.soy`) and changes to dependancies (`goog.require`) still require a recompile.

Do not use `uncompressed.js` for publicly-facing installations, since the browser needs to fetch hundreds of files and megabytes of data per page.  That's not something you want to do over a public network.