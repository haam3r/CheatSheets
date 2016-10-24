# Snippets abot doing stuff in vim

## Comment multiple lines

For those tasks I use most of the time block selection.

Put your cursor on the first # character, press CtrlV (or CtrlQ for gVim), and go down until the last commented line and press x, that will delete all the # characters vertically.

For commenting a block of text is almost the same: First, go to the first line you want to comment, press CtrlV, and select until the last line. Second, press ShiftI#Esc (then give it a second), and it will insert a # character on all selected lines. For the stripped-down version of vim shipped with debian/ubuntu by default, type : s/^/# in the second step instead.
[Source][1]

----------

## Indent multiple lines

Use the > command. To indent 5 lines, 5>>. To mark a block of lines and indent it, Vjj> to indent 3 lines (vim only). To indent a curly-braces block, put your cursor on one of the curly braces and use >%.

If you’re copying blocks of text around and need to align the indent of a block in its new location, use ]p instead of just p. This aligns the pasted block with the surrounding text.

Also, the shiftwidth setting allows you to control how many spaces to indent.

[Source][2]


  [1]: http://stackoverflow.com/questions/1676632/whats-a-quick-way-to-comment-uncomment-lines-in-vim
  [2]: http://stackoverflow.com/questions/235839/indent-multiple-lines-quickly-in-vi
