---
layout: theme:post
title: "The Syntax Error That Killed Our Build for a Week."
date: 2015-12-12T14:02:10-08:00
---
Back in the day when I wrote in C and we used X11 with Motif, I was creating a new module based on an existing one, so I just
copied and pasted the file from one xterm to another:

```C
main() {
/* ... */
  event_loop();
}
^L
void event_loop() {
  /* ... */
}
```

When I tried to compile it, *BOOM*. It complained of a syntax error at the form-feed (that's what the `^L` is, ASCII 0xc), 
which, back in the old days, told the line printer to eject the (gasp) paper page, and start a new one. 

Nowadays, ASCII calls this character "New Page", but no matter what you call it, it's whitespace. Compilers don't even see them.
The lexer should have removed it. But there it was, breaking our build, day after day, after day.

Finally my CTO figured it out. (I'm sorry, Jeff. Obviously, I still bear the scars of this.)

When I copied the file from one xterm to another, it didn't copy ASCII 0xc, instead it copied the rendered escape characters,
`^` and `L` (0x5e 0x4c), which of course, is not parseable.
