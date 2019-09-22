# Vim

Here I present my study notes that I put together when I get hands on Vim. In fact this markdown file is written with Vim. This write up covers basic to intermediate level of Vim, walking through how I leverage Vim as my new IDE.

## Navigation

At the very beginning, navigation in Vim is not that intuitive since I have been used to a development environment with mouse. Vim is aimed for a mouse-free workflow so we'd better spending time getting familiar with it. 

### Moving in the document

`h,j,k,l` moves the cursor left, down, up, right respectively, which could be combined with number key. For example, `5j` moves the cursor 5 lines down while `3h` moves 3 characters left, etc.

Moving on a character level might be too granular and involves too many key strikes. So Vim provides even more flexibility in terms of moving within the line or cross different lines. 

* `^` moves to the beginning of the line.
* `$` moves to the end of the line.
* `b` moves back to the beginning of the previous word. 
* `w` moves to the beginning of the next word. 
* `e` moves to the end of the next word.  
* `t` followed by a character moves the cursor up to the next matching character.
* `f` followed by a character moves the cursor to next matching character. 
* `%` moves to the corresponding closing bracket. 
* Note that the capitalized version, `B`, `E` and `W` skip punctuations. 
* `{` or `}` moves a block of code up or down. This feature is super useful when coding.
* 'NUMBER' followed by `G` moves to line NUMBER where NUMBER is just the line number, which is very convenient when you have `set number` on in your `.vimrc` file to show the line numbers. Note that `b` and `e` could also be combined with NUMBER while the functionality is self-explained.   

To move pagewise, `G` jumps to the end of the file and `gg` hops up to the beginning of the file.

## Editing

### Inside Text Object

Vim has the notion of Text Object. It knows the environment of your cursor and locate the word/sequence. THen what if you are in the middle of some sequence? `i` for inside some sequence could help. 

* `diw` deletes the word that the cursor is in.
* `di'` deletes what ever in the quote. 
* `ci(` changes whatever in the brackets. 
* `cip` changes the content in the paragraph. 
* `cit` changes the content in the tag, very useful for HTML. 

### Deleting

The easist way to delete something in your file is by pressing `x` to get rid of the character the cursor is hovering over. After I activate insert mode, the most common operations when I am editing are possibly deleting and undo. Don't laugh. People are hesitate about what move to make. Fortunately Vim provides the shortcuts `dd` to delete one line and `u` to undo the change. If you still want to make the change, `ctrl+r` will do. Another magic is `.` (period). It repeats the last operation. If you just want to delete a paragraph, again, `d` plus a moving command such as `}` deletes a paragraph. 

### Copying and Pasting

The other common operation is copy and paste. If you just want to copy and paste one line, you can move to that line, press `yy` then move to where you want to paste that line and hit `p`. `p` will paste the copied content one line below the cursor while `P` will paste one line above. 

To have more control on the content to be copied, visual mode comes into play. `v` enters visual mode for characters and `V` enters visual mode for blocks or paragraph. Pressing `v` activates visual mode. The paragraphs in visual mode get highlighted after which you can press `y` to copy. The beauty of visual mode is that you can use whatever command to move in the document. For example, when you are in visual mode, you can use `}` to put the following code block into highlight and `d` to delete it or `y` to copy it. 

### Content Creating and Changing

Of course `i` activates insert mode. `I` activates insert mode at the beginning of this line. `A` brings insert mode at the end of this line. For adding new lines, `o` adds one new line below the cursor and `O` adds one above. But for most of the time, we want to change existing code: `c` is the trick. `cw` will change a word into something you just typed. However, the real power of `c` comes with the combination with other motion keys. For example, `c$` will change all things to the end of this line or you can use `C`. `ct)` will change all things up to the next ')'. 

### Searching and Looking up

When the cursor is hovering over a word/function, `*` finds you the next instance of this word. This is quite useful when you search for the places calling the hovered function. The more former searching function that supports regular expression is `?` or `/` followed by things you want to search in the current file. 

### Miscellaneous

`~` swaps lower case to capital case and vice versa. `.` repeats your last command. `r` lets you replace one letter and `R` replaces with all you type in. 

## Macro

Macro is a super powerful tool which enables you to record a series of commands into some letter and reuse afterwards. For example, let's say we have the following peice of code.

```python
var = [one, two, three, four, five, six, seven, eight]

```

We want to add quotation for each word in the series. What we can do is to record the following series of command into a letter, let's say `T`, then use `@T` to call the macro, just like calling a function. 

0. `qT` to start recording the macro and save it to letter 'T'. 
1. Move to the one character before the beginning of word, which is '['.
2. Insert "'" right before 'o': `a'` to insert "'" after '[', `esc` to exit insert mode, `a'` to insert "'", and `esc` to exit insert mode.
3. `w` to move to next word, `h` to move to the one character right before the beginning of 'two'. 

Then `@T` to call the macro and call the macro several times by NUMBER `@T` until all the words are quoted.  

```python
var = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight']
```
