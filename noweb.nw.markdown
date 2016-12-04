The following literature is modified from the original version.  You
may see the original version either from previous version of this
file in Git (commit 81cd901a7377d0021f84f38b00afd633fe2009c2) or here:

  `http://jonaquino.blogspot.com/2010/04/nowebpy-or-worlds-first-executa
  ble-blog.html`

  (shorten url: https://goo.gl/OMK80Y )

The modification I made to this file are as follows:

  * Wrapping line to have at most 72 characters per line.  And using 2
    spaces after each period (.).  I like it this way personally.

  * To accomplish the above change, I have to shorten long URL using
    Goo.gl (URL Shortener Service from Google).  The script I use to
    call Goo.gl is from:
    
      https://github.com/dragontortoise/google_shortener

  * Replacing the word "I" with the original author's name which is
    "JonathanAquino" so that I can use "I" to refer to myself.

  * Modifying and adding more information as I see appropriate.

  * Modifying Python code from Python 2 to Python 3.  This is done
    very easily as there was only 1 place to fix which is the print
    statement in Python 2 which is changed to be function in Python 3.

This executable document first appeared as a blog post on:

  `http://jonaquino.blogspot.com/2010/04/nowebpy-or-worlds-first-executa
  ble-blog.html`

  (shorten url: https://goo.gl/fPgdxH )

JonathanAquino has recently been interested in the old idea of 
[literate programming](https://goo.gl/mZ11) (full url: `http://en.wikipe
dia.org/wiki/Literate_programming`).

Basically, you have a document that describes in detail how a program
works, and it has embedded chunks of code. It allows you to see the
thoughts of the programmer as he explains how he writes the program
using prose. A tool is provided that you can use to extract the working
program from chunks of code in the document.

Here's the thing: *what you are reading right now is a literate
program*.

Yes, you can copy this blog post into a file and feed it into the tool,
and it will spit out a program.  Q: Where do I get the tool?  A: That's
the program that this document spits out.  This document will produce a
script that you can use to extract code from
[noweb](http://en.wikipedia.org/wiki/Noweb)-format literate programs.

Why do we need to make a new tool if the
[noweb](http://en.wikipedia.org/wiki/Noweb) tool already exists? Because
the noweb tool is hard to install.  It's not super-hard, but most people
don't want to spend time trying to compile it from source.  There are
Windows binaries but you have to [get](https://goo.gl/QNkkqP)
(full url: `http://web.archive.org/web/*/http://www.literateprogramming.
com/noweb/nowebinstall.html`) them from the Wayback Machine.

Anyway, the noweb tool doesn't seem to do very much, so why not write a
little script to emulate it?

And that is what we will do now.

# DOWNLOAD

If you are just interested in the noweb.py script produced by this
document, you can [download](https://goo.gl/lT3W3M) (full url: `http://g
ithub.com/JonathanAquino/noweb.py/raw/master/noweb.py`) it from GitHub.

# USAGE

The end goal is to produce a Python script that will take a literate
program as input (noweb format) and extract code from it as output.  For
example,

~~~
  noweb.py -Rhello.php hello.noweb > hello.php
~~~

This will read in a file called hello.noweb and extract the code
labelled "hello.php".  We redirect the output into a hello.php file.

Or in our case, to generate noweb.py itself, it will be:

~~~
  noweb.py -Rnoweb.py noweb.py.txt > noweb.py
~~~

# READING IN THE FILE

In a literate program, there are named chunks of code interspersed
throughout the document.  Take the chunk of code below.  The name of it
is "Reading in the file".  The chunk ends with an @ sign.

Let's start by reading in the file given on the command line.  We'll
build up a map called "chunks", which will contain the chunk names and
the lines of each chunk.

~~~
<<Reading in the file>>=
file = open(filename)
chunkName = None
chunks = {}
OPEN = "<<"
CLOSE = ">>"
for line in file:
    match = re.match(OPEN + "([^>]+)" + CLOSE + "=", line)
    if match:
        chunkName = match.group(1)
        # If chunkName exists in chunks, then we'll just add to the
        # existing chunk.
        if not chunkName in chunks:
            chunks[chunkName] = []
    else:
        match = re.match("@", line)
        if match:
            chunkName = None
        elif chunkName:
            chunks[chunkName].append(line)
@
~~~

# PARSING THE COMMAND-LINE ARGUMENTS

Now that we have a map of chunk names to the lines of each chunk, we
need to know which chunk name the user has asked to extract.  In other
words, we need to parse the command-line arguments given to the script:

~~~
  noweb.py -Rhello.php hello.noweb
~~~

For simplicity, we'll assume that there are always two command-line
arguments: in this example, "-Rhello.php" and "hello.noweb".  So let's
grab those.

~~~
<<Parsing the command-line arguments>>=
filename = sys.argv[-1]
outputChunkName = sys.argv[-2][2:]
@
~~~

# RECURSIVELY EXPANDING THE OUTPUT CHUNK

So far, so good.  Now we need a recursive function to expand any chunks
found in the output chunk requested by the user.  Take a deep breath.

~~~
<<Recursively expanding the output chunk>>=
def expand(chunkName, indent):
    chunkLines = chunks[chunkName]
    expandedChunkLines = []
    for line in chunkLines:
        match = re.match("(\s*)" + OPEN + "([^>]+)" + CLOSE + "\s*$", \
          line)
        if match:
            expandedChunkLines.extend(expand(match.group(2), indent + \
              match.group(1)))
        else:
            expandedChunkLines.append(indent + line)
    return expandedChunkLines
@
~~~

# OUTPUTTING THE CHUNKS

The last step is easy.  We just call the recursive function and output
the result.

~~~
<<Outputting the chunks>>=
for line in expand(outputChunkName, ""):
    print(line, end="")
@
~~~

And we're done.  We now have a tool to extract code from a literate
programming document.  Try it on this blog post!

# APPENDIX I: GENERATING THE SCRIPT

To generate noweb.py from this document, you first need a tool to
extract the code from it. You can use the original
[noweb](https://goo.gl/ghNSXF) (full url: `http://www.cs.tufts.edu/~nr/n
oweb/`) tool, but that's a bit cumbersome to install, so it's easier to
use the Python script [noweb.py](https://goo.gl/lT3W3M) (full url: `http
://github.com/JonathanAquino/noweb.py/raw/master/noweb.py`).

Then you can generate noweb.py from noweb.py.html as follows:

~~~
  noweb.py -Rnoweb.py noweb.py.txt > noweb.py
~~~

# APPENDIX II: SUMMARY OF THE PROGRAM

Here's how the pieces we have discussed fit together:

~~~
<<noweb.py>>=
#!/usr/bin/env python

# noweb.py
# By Jonathan Aquino (jonathan.aquino@gmail.com)
#
# This program extracts code from a literate programming document in
# "noweb" format.  It was generated from noweb.py.txt, itself a literate
# programming document.
# For more information, including the original source code and
# documentation, see https://goo.gl/fPgdxH (full url: http://jonaquino.
# blogspot.com/2010/04/nowebpy-or-worlds-first-executable-blog.html

import sys, re
<<Parsing the command-line arguments>>
<<Reading in the file>>

<<Recursively expanding the output chunk>>

<<Outputting the chunks>>
@
~~~
