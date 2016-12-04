This is a readme file of project dragontortoise/noweb.py on GitHub.com .

Author: dragontortoise

Language: Tinglish

I fork the code from https://github.com/JonathanAquino/noweb.py .

noweb is a [literate programming](https://goo.gl/zJVqHB)
(full url: https://en.wikipedia.org/wiki/Literate_programming )
tool based on Donald E. Knuth's concept about Literate Programming.

JonathanAquino is the original author of noweb.py, an implementation
of noweb written in Python.  There are a few interesting forks
from his version, for example:

  - https://github.com/Kerrigan29a/noweb.py

    The code is too difficult for me to understand right now.  So I
    have to skip it for now.

  - https://github.com/lmc2179/literate-python

    It seems to generate document in LaTeX instead of Markdown as in
    JonathanAquino's version.

And there is also another project which is not a fork from
JonathanAquino.  It is:

  https://github.com/fiskr/lesen

*lesen* allows us to choose the documentation format to be either
LaTeX or Markdown.  It doesn't use *Recursion* to tangle (printing out
the code).

I also find people in the Internet talking about emacs's org-mode with
babel.  It seems to be good and easy to use too.  However, I was
converted to vi long time ago.  It will take time for me to switch
back to emacs so I will try it later.

After studying the above 4 noweb projects, I decided to fork from
JonathanAquino's version as I find his code is the simplest and easiest
to understand.  He uses *recursion* which is good for code readability
for this specific problem.

# TODO
- Writing how to install and run my version of noweb.py .
