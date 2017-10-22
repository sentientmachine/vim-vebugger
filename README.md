INTRODUCTION
============

Screencast - https://vimeo.com/95775461

Vebugger is yet another debugger frontend plugin for Vim, created because I
wasn't happy with the other debugger plugins I found. Vebugger currently
supports:

 * Tracking the currently executed command in the source code
 * Debugger flow commands - step-in, set-over, set-out and continue
 * Breakpoints management
 * Evaluating expressions in the current executed scope
 * Messing with the program's state (changing values, calling functions)

Vebugger is built as a generic framework for building frontends for
interactive shell debugger, and comes with implementations for:

 * GDB - doesn't need introduction...
 * LLDB - debugger based on LLVM for C-family languages
 * JDB - a Java debugger
 * Mdbg - a .NET debugger(Windows only)
 * PDB - a Python module for debugging Python scripts
 * RDebug - a Ruby command line option for debugging Ruby scripts

Other implementations can be added with ease, and I will accept pull requests
that add such implementations as long as they use Vim's |license|.

Vebugger is built under the following assumptions:

 * While command line debuggers share enough in common to make the creation
   of such a framework as Vebugger possible, the differences between them are
   too great to be expressed with regular expression. To support them all at
   least some code has to be written.
 * Unlike IDE users, Vim users tend to understand the tools the operate behind
   the scenes. While Vebugger automates the common features, it allows you to
   "open the hood" and interact with the debugger's shell directly so you could
   utilize the full power of your debugger.
 * I have no intention to aim for the lowest common denominator. If one
   debugger has a cool feature I want to support, I'll implement it even if the
   other debuggers don't have it.

Vebugger is developed under Linux. It doesn't work properly under Windows due
to lack of PTY support. I have neither plans nor means to support OSX, but I
will accept pull requests that add OSX support.

The features that don't work under windows are:

 * RDebug.
 * Displaying output from the debugged program.

REQUIREMENTS
============

Vebugger requires the vimproc plugin, obtainable from:
https://github.com/Shougo/vimproc.vim.  Notice that vimproc needs to be built -
there are instructions in the GitHub page.

In order for Vebugger to use a debugger, that debugger must be installed and
it's executable must be either be in the PATH or set with a global variable
(see `help vebugger-configuration`). In case of RDebug and PDB, which are used
from the Ruby and Python modules, the interpreter(`ruby` or `python`) is the
one that must be installed and in the path.

USAGE
=====

Run `help vebugger-launching` from Vim to learn how to launch the debugger.

Run `help vebugger-usage` from Vim to learn how to operate the debugger.



Eric Instructions, Install
=====

1.  git clone the project into /home/el/bin/
2.  Good news we already have vimproc installed in ~/.vimrc
3.  Check it out, make sure everything in there is kosher
4.  move it to vim plugins: mv vim-vebugger /home/el/.vim/bundle/
5.  Add this line to ~/.vimrc Plugin 'idanarye/vim-vebugger'
6.  Install plugins: vim +PluginInstall +qall
7.  In vim, run `:help vebugger-launching`  read that.
8.  Start a vim instance on a program.
9.  :VBGstartPDB %
10.  Move to a different line :VBGtoggleBreakpointThisLine
11.  type :VBGcontinue
12.  :VBGeval myvariablename
13.  woah



Eric Instructions, do python
=====

These commands are good for ~/.vimrc

      let mapleader=","
      "comma stp stands for start, these will need to be custom made for each kind of file.
      "There are parameters for python2 or python3
      :nnoremap <leader>stp :VBGstartPDB %<cr>

      "Toggle a breakpoint for the current line
      :nnoremap <leader>b :VBGtoggleBreakpointThisLine<cr>
      "Continue the execution and don't stop uless you see breakpoints.
      :nnoremap <leader>c :VBGcontinue<cr>
      "Evaluate word under cursor.  WOW!
      :nnoremap <leader>e :VBGevalWordUnderCursor<cr>
      "Evaluate and print the expression supplied as argument.
      :nnoremap <leader>E :VBGeval
      "execute this line in normal mode
      :nnoremap <leader>x :VBGexecute<cr>
      "execute this line in normal mode
      :nnoremap <leader>X :VBGeval<cr>
      "toggle terminal buffer
      :nnoremap <leader>t :VBGtoggleTerminalBuffer<cr>
      "Continue the execution, stopping at the next statement.
      :nnoremap <leader>o :VBGstepOver<cr>
      "Continue the execution until the end of the current function
      :nnoremap <leader>O :VBGstepOut<cr>
      "Same as VBGstepOver but stepps into functions.
      :nnoremap <leader>i :VBGstepIn<cr>
      "Clear all breakpoints.
      :nnoremap <leader>B :VBGclearBreakpoints<cr>

      ":VBGkill Terminates the debugger
      :nnoremap <leader>k :VBGkill<cr>

      "Select mode only, raw write selected text
      :nnoremap <leader>r :VBGrawWriteSelectedText<cr>
      "Prompt for an argument for VBGrawWrite
      :nnoremap <leader>R :VBGrawWrite<cr>

1.  Make a python file something.py
2.  Assign some variables and print some lines
3.  press ,stp  to start debugging
4.  press ,b  over a line to make a breakpoint
5.  Press ,c  to continue
6.  Press ,e to in spect a variable under the cursor
7.  Press ,k to kill debugging session




Eric Instructions, do Java
=====

These commands are good for ~/.vimrc

The -g option includes debugging info into the out file.

      let mapleader=","
      "comma stj stands for start, these will need to be custom made for each kind of file.
      :nnoremap <leader>stj :!javac -g Main.java<cr><cr>:call vebugger#jdb#start('Main',{'classpath':'.', 'srcpath':'.', 'args':['hello','world']})<cr>

      "Toggle a breakpoint for the current line
      :nnoremap <leader>b :VBGtoggleBreakpointThisLine<cr>
      "Continue the execution and don't stop uless you see breakpoints.
      :nnoremap <leader>c :VBGcontinue<cr>
      "Evaluate word under cursor.  WOW!
      :nnoremap <leader>e :VBGevalWordUnderCursor<cr>
      "Evaluate and print the expression supplied as argument.
      :nnoremap <leader>E :VBGeval
      "execute this line in normal mode
      :nnoremap <leader>x :VBGexecute<cr>
      "execute this line in normal mode
      :nnoremap <leader>X :VBGeval<cr>
      "toggle terminal buffer
      :nnoremap <leader>t :VBGtoggleTerminalBuffer<cr>
      "Continue the execution, stopping at the next statement.
      :nnoremap <leader>o :VBGstepOver<cr>
      "Continue the execution until the end of the current function
      :nnoremap <leader>O :VBGstepOut<cr>
      "Same as VBGstepOver but stepps into functions.
      :nnoremap <leader>i :VBGstepIn<cr>
      "Clear all breakpoints.
      :nnoremap <leader>B :VBGclearBreakpoints<cr>

      ":VBGkill Terminates the debugger
      :nnoremap <leader>k :VBGkill<cr>

      "Select mode only, raw write selected text
      :nnoremap <leader>r :VBGrawWriteSelectedText<cr>
      "Prompt for an argument for VBGrawWrite
      :nnoremap <leader>R :VBGrawWrite<cr>

1.  Make a java file Main.java
2.  Assign some variables and print some lines
3.  press ,stj  to compile the .class file and start debugging
3a.  There's some complexity here, the compile and run is a two step command here:

    :!javac -g Main.java<cr><cr>:call vebugger#jdb#start('Main',{'classpath':'.', 'srcpath':'.', 'args':['hello','world']})<cr>
 
4.  press ,b  over a line to make a breakpoint
5.  Press ,c  to continue
6.  Press ,e to in spect a variable under the cursor
7.  Press ,k to kill debugging session




