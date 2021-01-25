# Authoring with Vim

__The REAL writing, not _code_ writing 😆.__

_This post reflects only the thoughts of its author, Thomas Alcala._

_If you don't know vim, I recommend trying [Vim
Adventures](https://vim-adventures.com/), a game-like vim tutorial._  

## Introduction  

It was already the case before with tools like WordPress, but with the
surge of headless CMS, serverless platforms, JAMstack, Wix, writing and
publishing your own blog is easier than ever.  

There's a lot of value in browser editors too (the famous WYSIWYG, What
You See Is What You Get) that let users click text formatting buttons,
and the HTML code is generated for them. No need to learn web design,
and with browser plugins like the Grammarly plugin, you also get the
spelling,  grammar corrections you'd get in an office software.  

So you ask __"is there a reason to try and write with vim?"__

Vim is not the most beginner-friendly editor, so this is mostly for
people already familiar with vim, but the answer is _**YES!**_  

Let's try to get some cool writing features, to prove it!  

## Requisites

As you may know, the vim configuration is located in `~/.vimrc`.
Technically, you can call `vi -u /path/to/some/other/vimrc` to use
another configuration file, so it should be possible to use different
configurations for code and writing. I prefer to maintain only one file, so I use a plugin manager for vim.  

1. [vim-plug](https://github.com/junegunn/vim-plug)  
2. [Go](https://golang.org/dl/) for some plugins  

## Formatting

For text formatting, I use
[vim-pencil](https://github.com/reedes/vim-pencil). It automatically
breaks lines in Markdown files, hide formatting code, etc. It helps
keeping the paragraphs clean.  
I use the basic implementation with no configuration, the only trick
I made is with vim-plug to conditionally import it, so it doesn't break
lines and stuff when I write code:  

```
Plug 'reedes/vim-pencil', { 'for': ['text', 'notes', 'markdown', 'mkd'] }
```

## Spell Check  

For spell checking, I use another excellent plugin by reedes:
[vim-lexical](https://github.com/reedes/vim-lexical). You can also
configure a dictionary and a thesaurus, for completion, for technical
terms, etc. Very powerful.  

## Quality Assurance  

Last but not least, by reedes, there's a plugin for writing quality,
called [vim-wordy](https://github.com/reedes/vim-wordy).  

It's got a bunch of dictionary rings for the English language, and you
can switch between them. For example, it detects  
* redundant words
* jargon
* problematic terms
* lazy or passive voice
* manipulative (weasel) language
* colloquialisms and idioms
* etc... 

I'll run it for fun after a first draft of this post, this should be
somewhat of an eye opener 😆.

## Completion  

Completion is kind of done by the lexical plugin, this is more about
**_prediction_ of the next word** in a sentence!

Would you like an example?  

![Text prediction](assets/images/prediction.gif)

For a non-native English speaker like me, this is very useful, most of
all for some idiomatic expressions.  

This is done by combining some data, scripts and vim plugins:  

1. Get the data from
   [the nextword dataset release page](https://github.com/high-moctane/nextword-data/releases), there's a small and a large dataset, pick the one you like
1. Define an environment variable in your shell for the dataset location
```
export NEXTWORD_DATA_PATH=/path/to/nextword-data
```
1. Get the nextword script (you must have installed Go before, as
   mentioned in the prerequisites)  
```
go get -u github.com/high-moctane/nextword
```
1. Install
   [asyncomplete-nextword](https://github.com/high-moctane/asyncomplete-nextword.vim), that means adding the `asyncomplete` and `async` vim plugins too, it's all in the plugin installation instructions  
   Here I use the same trick as for the pencil plugin, and write
   ```
   Plug 'high-moctane/asyncomplete-nextword.vim', { 'for': ['text', 'notes', 'markdown', 'mkd'] }
   ```
   in my `.vimrc` file, so this plugin doesn't try to complete code
   with grammarily correct words  


## Emojis  

With very technical or outlandish posts, like one trying to sell writing
 blog posts with vim, it's almost mandatory to put
some emojis to lighten the tone. I use a couple of plugins for that:
[vim-emoji](https://github.com/junegunn/vim-emoji) and
[asyncomplete-emoji](https://github.com/prabirshrestha/asyncomplete-emoji.vim), that takes advantage of the asyncomplete plugin already installed in the completion segment of this post.  

The asyncomplete plugin autocompletes stuff starting with colon, like
`:thumbsup:`, then I mapped a `vim-emoji` command in my `.vimrc` file that uses `sed` to translate all emojis
to unicode 👍!  
```
nnoremap <Leader>e :s/:\([^:]\+\):/\=emoji#for(submatch(1), submatch(0))/g<CR>  
nnoremap <Leader>E :%s/:\([^:]\+\):/\=emoji#for(submatch(1), submatch(0))/g<CR>  
```

So now, `<Leader>` (comma `,` in my configuration) + `e` rewrites all
`:emoji:` to their symbol, lowercase `e` for the current line, uppercase
for the whole buffer.  

## Distraction-free Mode  

I also use [goyo.vim](https://github.com/junegunn/goyo.vim) and
[limelight.vim](https://github.com/junegunn/limelight.vim), even though
it may be even more distracting until you get used to it! 😆  

![Goyo](assets/images/goyo.gif)

## Bonus  

Not for vim but for screen recording and making gifs out of videos, I
use:  
* simplescreenrecorder to record the screen or part of it  
* ffmpeg to make a gif out of the mkv video  
```
ffmpeg -i ~/Videos/simplescreenrecorder-2020-03-29_19.22.57.mkv -r 15 -vf scale=720:-1 assets/images/goyo.gif  
```

## Conclusion  

Turns out it's not completely crazy to author stuff in vim! 😉
