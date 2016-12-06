# noweb.py/utils/

utils/ directory contains utilities which can be useful to you when
you code in literate programming.

## vim/

For those who use vim as their favourite text editor, there is a plugin
to do syntax highlighting for your noweb file.  Under noweb.py/utils/vi
m/ directory, you will find SyntaxRange-1.02.vmb file.  This file
comes from:

  http://www.vim.org/scripts/script.php?script_id=4168

To install it, you open the SyntaxRange-1.02.vmb file with vi and then
use command ":UseVimball" inside vi to install it to ~/.vim/ .

After that you can integrate it into vi so that it will be called
automatically every time you open a file with extension .markdown by
doing as follows:

  - Create a file named SyntaxInclude.vim and place it inside:

      ~/.vim/after/syntax/markdown/

  - Put this code inside this SyntaxInclude.vim file:

    ~~~
    call SyntaxRange#Include('__begin=python__', '__end=python__',
      \ 'python', 'NonText')
    ~~~

Now, in your .markdown file, when you want to write Literate Python
program, you begin your Python code with:

  ~~~
  __begin=python__
  ~~~

and end your Python code with:

  ~~~
  __end=python__
  ~~~

You can see an example at:

  noweb.py/utils/vim/example.nw.markdown

