All credits go to Brenner. I just adjust what feels wrong to me.

Changes:
------

* **clean search history**:

    expansion of templates doesn't pollute the search history as no more search via `/` or `?` is used

* **expansions are undoable**

    Break the undo sequence for expansions. 
    This allows e.g. mappings like `inoremap <C-B> <C-o>u<SPACE>` to undo the expansion and insert a space instead. 
    In math mode this might prove anoying when you have a lot of expansions.

* **better default dictionaries** (to my mind)

    Consitent Greek letters (in the same fashion as [Vimtex](https://github.com/lervag/vimtex))

## QuickTex is a template expander for quickly writing LaTeX

Before anything else, here's a real-time demonstration of what QuickTex can do:
<img src="http://brennier.com/static/pictures_original/vim_latex_plugin.gif">
<img src="https://user-images.githubusercontent.com/22542812/32899699-f3c63a16-caeb-11e7-969d-81b0a5215a77.jpg">

Basically, QuickTex allows you to set keywords which activate arbitrary Vim code whenever they are typed in insert mode. The expansions are filetype specific and are triggered by pressing space. In most respects, you can think of it like a much-improved version of Vim abbreviations.

## How is QuickTex different from UltiSnips or Vim abbreviations?

The main points are simply

1. QuickTex has a separate namespace for math mode expansions. This will help you type math in Latex faster than you ever thought possible. My math mode dictionary has hundreds of keywords in it, which allow me to pratically type in English and have it converted into pure Latex in real-time.

2. QuickTex keywords are automatically triggered after a space. This allows you to have seamless expansions that don't slow you down and allow you to type keyword after keyword in rapid sucession.

3. QuickTex is very fast. Since the code is written completely in Vimscript, QuickTex expands keywords instanteously. Programming similar functionality into a snippets plugin would be significantly slower, especially when you include the context dependence for math mode.

#### Here's a little table that displays some of the main differences:
| Features            | QuickTex          | UltiSnips         | Abbreviations     |
| ------------------- | ----------------- | ----------------- | ----------------- |
| Default Trigger Key | `<Space>`         | `<Tab>`           | Any non-word character |
| Cursor Placement    | Yes               | Yes               | No                |
| Default Jump Key    | `<Space><Space>`\*| `<C-J>`           | N\A               |
| Placeholders        | `<++>`            | Invisible         | N\A               |
| Available Modes     | Only Insert Mode  | Only Insert Mode  | Any mode          |
| Math Mode Context?  | Yes, very fast    | Possible, but slow| Very difficult to implement |
| Speed Ranking       | Fastest           | Slowest           | In the middle     |
| File Type Specific? | Yes               | Yes               | No                |

\* Requires adding the entry `\' '` from the default dictionaries to your dictionary, which is highly recommended.

## Installation

I personally use vim-plug, but here's the various install commands for a variety of plugin managers:
```vim
" vim-plug
Plug 'DerWeh/quicktex'
" NeoBundle
NeoBundle 'DerWeh/quicktex'
" Vundle
Plugin 'DerWeh/quicktex'
```

## Configuration

The keywords and their expansions are recorded in various dictionaries. Each filetype has its own dictionary, which should be named in the form of `g:quicktex_<filetype>`. There is also an additional dictionary that you can use called `g:quicktex_math` which is used whenever you are inside math delimiters of a Latex file. This example dictionary would give you all the functionality you need for the above gif to work:

```vim
let g:quicktex_tex = {
    \'m'   : '\( <+++> \) <++>',
    \'prf' : "\\begin{proof}\<CR><+++>\<CR>\\end{proof}",
\}

let g:quicktex_math = {
    \'fr'   : '\mathcal{R} ',
    \'eq'   : '= ',
    \'set'  : '\{ <+++> \} <++>',
    \'frac' : '\frac{<+++>}{<++>} <++>',
    \'one'  : '1 ',
    \'st'   : ': ',
    \'in'   : '\in ',
    \'bn'   : '\mathbb{N} ',
\}
```

A few things to note here. If there is a `<+++>` anywhere in the expansion, then your cursor will automatically jump to that point after the expansion is triggered.
If you want to jump to the next `<++>` by hitting `<SPACE>` after as space, copy the first entry of the default dictionaries with key ` `.
You may think this would be annoying to map double space to this, but it's actually extremely useful and doesn't get in the way as much as you'd think. 
Using this entry, you can put `<++>`'s in your other expansions to jump around very easily.

Keywords can be any string without whitespace. 
Expansions can either be a literal string (using single quotes) or a string with keypress expansions (using double quotes). 
Keypress expansions are things like `\<CR>`, `\<BS>`, or `\<Right>` that one would find in vim remappings. 
Keep in mind that `\`'s need to be escaped (i.e. `\\`) when using double quoted strings and that you need a `\` at the beginning of each line of your dictionary.

For more ideas about what to include your dictionary, please take a look at the default dictionaries in `ftplugin/tex/default_keywords.vim`. 
It is highly recommended that you make your own custom dictionaries, as the default dictionaries may change without warning.

For more information, read the full documentation using `:help quicktex`.


Todo:
-----

* [ ] make math mode more flexible to be expandable to other environments (maybe even insets like code of another language)
* [ ] Write better and more consistent default dictionaries
  * [X] Redo Greek letters
* [ ] Offer in Vim help and maybe a cheat-sheet to find available abbreviations.
* [ ] Allow local files to include expansions for your custom commands.
