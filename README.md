# notes
> Shell script for compiling pdf notes with pandoc.

## Usage

    notes [year|"all"] [module]

## Examples

    notes
       compiles notes for current year/module

    notes all
       compiles all notes

    notes 1
       compiles notes for year 1

    notes 1 3
       compiles notes for year 1 module 3

## Dependencies
    * pandoc
    * LaTex
    * ghostscript

## Explanation

This is a script I wrote to help compile my medical school notes with pandoc. As such, it's catered to my needs, but you should be able to adjust it for yours quite easily. Notes are organized in a directory specified by the environment variable `$NOTES_DIR`. Inside this directory there are years and inside the years there are modules. It also uses environment variables `$YEAR` and `$MODULE` for the current year and module. You could define these in the script itself if you'd like. The directory structure looks like this...

    /home/connermcd/Dropbox/Courses/Notes
    ├── MS1
    │   ├── MS1.pdf
    │   ├── mod1
    │   │   ├── img
    │   │   └── Pharm - Example.txt
    │   ├── mod2
    │   ├── mod3
    │   └── ...
    ├── MS2
    │   ├── MS2.pdf
    │   ├── mod1
    │   └── ...
    ├── about.md
    ├── fonts.tex
    ├── html.md
    └── slidy.html

Each file has a header indicating the title and date. For instance...

    # Pharm - Example
    > Date: 04-24-13  
    > Instructor: McDaniel

    ![This is an image](img/image.jpg)\ 

This is used by the script to sort the files chronologically in the exported pdf. Images can be stored in the `img` directory for the corresponding module.

## Vim/bash integration

I use vim and so I have some handy shortcuts that I use for notes that I will include here. These use `ack` and the `Ack.vim` vim plugin.

    let g:year = system('echo -n "$YEAR"')
    let g:module = system('echo -n "$MODULE"')
    command! -nargs=1 Nack Ack -i --text --nohtml "<args>" $NOTES_DIR/*/*/*.txt
    command! -nargs=1 Note exe "e! " . fnameescape($NOTES_DIR . "/MS". g:year . "/mod" . g:module . "/<args>.txt")

    nnoremap <leader>[ :Nls 
    nnoremap <leader>] :Note 

    augroup markdown
       au!
       au BufNewFile,BufRead,BufWrite *.txt,*.md,*.mkd,*.markdown,*.mdwn setl ft=markdown
       au BufRead,BufNewFile,BufEnter   */mod*/*.txt let &complete="k$NOTES_DIR/**/*.txt"
       au BufRead,BufNewFile,BufEnter   */mod*/*.txt lcd %:h
       au BufRead,BufWrite,InsertChange */mod*/*.txt syn match ErrorMsg '\%>77v.\+'
    augroup end

I also use a bash version of the `:Nack` command so I can search my notes from the command line.

    function nack {
       vim -c "Nack $*"
    }

## MIT License

Copyright (C) 2013 copyright holder

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the "Software"),
to deal in the Software without restriction, including without limitation
the rights to use, copy, modify, merge, publish, distribute, sublicense,
and/or sell copies of the Software, and to permit persons to whom the 
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
