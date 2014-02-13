---
layout: post
title:  "Syntax Highlighting on StackOverflow"
categories: stackoverflow
---

I just had someone edit a posting of mine on StackOverflow, specifically this [one](https://stackoverflow.com/questions/21522946/how-to-handle-resume-for-libgdx-image-with-pixmap-texture). Honestly I find most of the edits I get to be people being a little overzealous about grammar, very few edits add any real clarity. But the edit to this post bugged me enough that I wanted to undo it. The editor added the `java` tag to my post about LibGDX (an Android game development framework).

Personally I refer to Android programming as using the Java language even if everyone doesn't agree completely (missing libs, no TCK cert, etc). What bothered me was that the post wasn't about Java at all. True, it used a snippet of code written in the Java language but the question was specifically about how to accomplish something with LibGDX. I've seen several LibGDX questions tagged as Java *AND* LiBGDX and people attempting to answer questions don't look at tags as an intersection, they look at them as a union. If their expertise falls in the union they try to answer the question which unsurprisingly results in some really poor answers. Additionally, people looking to find pure Java question now have to wade through LibGDX questions.

So why did the editor add the Java tag? His comment on the edit (thanks for that) let me know he did it in order to enable color syntax highlighting for my code blocks. I hadn't even noticed but before the edit my code blocks lacked any pretty colors. It turns out that SO will use the tags of a question to determine which language should be used for color syntax highlighting. Don't ask me what happens if someone tags a post with two different languages, I don't know.

Instead of adding tags to a post just to enable color syntax highlighting you can use SO specific markdown within the post itself. In this case I added `<!-- language-all: java -->` to my post which specifies that all code blocks should be highlighted using Java syntax. You can also use `<!-- language: java -->` immediately before a code block to specify the language for the following block. For more information, take a look at <https://stackoverflow.com/editing-help#syntax-highlighting>. The list of languages can be found at <https://meta.stackoverflow.com/questions/184108/what-is-syntax-highlighting-and-how-does-it-work>.
