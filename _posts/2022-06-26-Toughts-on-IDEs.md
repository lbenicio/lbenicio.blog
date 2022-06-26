---
layout: posts
title: Toughts of IDEs
icon: fa-comment-alt
tag: terminal, customization, theming, tutorial
categories: tutorial
---

Ok, we are, my second post, I never thought I would reach this milestone. Jokes aside, we are here to a brief post to talk about Text Editors on steroids. IDEs.

# Sections

1. [Web applications versus Native applications](#web-applications-versus-native-applications)

2. [Why you should go native](#why-you-should-go-native)

3. [Text editor Tweaks](#text-editor-tweaks)

4. [Theming](#theming)

5. [Plugins](#plugins)

5. [Conclusion](#conclusion)

# Web applications versus Native applications

I will say the hard truth here. Web based OSes applications are the worst performing ones we have nowadays. We need to state that first. All those apps have to span a "small" browser inside theirs apps to display content, or use some sort of HTML to native code conversion, and none of them have resolved the problem.

So, the answer here is pretty simple, we need a lightweight text editor to write our code. This text editor need to be scriptable, so we can add small pieces of code as we need it. For example, if you are developing some Java code and want easy IDE capabilities, you can add a plugin.

Ideally we would want a Free Software, or, at least, open source. But at the moment, all the mainstream IDEs fall into the category of being a web based application, like brackets.io, VSCode, Atom, and many others.

Unfortunately, my choose here is not a Free Software, nor an Open Source one, but it is free. Sublime Text.

![Sublime Text logo](/assets/imgs/sublimeTextLogo.svg){:class="img-fluid"}

[https://www.sublimetext.com/](https://www.sublimetext.com/)

Sublime text is a native text editor, super light weight, and has a plugin ecosystem. It is almost perfect.

# Why you should go native

It is pretty expensive for a text editor, it costs you around $100. But you can use it for free and just a pop-up appears every once in a while asking for a payment. Which is pretty ok, I guess.

But this post is not just about raging on web applications packed into native ones, and try to convince the reader to use a text editor instead of an IDE.

Actually, is it, I was lying. You should use a text editor, and, as little as possible, the features an IDE provides. It is like compiling code using terminal or pressing a button on an IDE. If you start your studying using an IDE that compiles to you, and start your career doing so as well, you might find it pretty hard to understand what the computer is doing "behind the scenes", and actually understand what is going on. And, when you face a situation of a big project, or a project you cannot compile pressing a button, but you have to run, or even write, a series of scripts and import and install decencies, you may find yourself in trouble.

# Text editor Tweaks

We are not going to just debate what is better here, let us do the part the all of you came here to do. The tweaks:

Go to Preferences → Settings:

```json
{
    "always_show_minimap_viewport": true, // show right side minimap of your code
    "draw_minimap_border": true, // show a border to the minimap
    "font_face": "Roboto Mono", // set a monospaced font
    "font_size": 16, // increase font size, you will be staring at code all day long
    "highlight_line": true, // highlight the line you are working on
    "ignored_packages": [
        "Vintage",
    ],
    "rulers": [ // Rullers, rullers everywhere. 
        80,
        120
    ],
    "save_on_focus_lost": true, // we need an auto-save feature unless we want to lost our wrok
    "spell_check": true, // no developer knows how to write properly non-code language
    "tab_size": 4, // 4 tab spaces is better then 2, prove me wrong
    "theme": "predawn-DEV.sublime-theme", // theming
    "translate_tabs_to_spaces": true, // yeah, space gang here.
    "word_wrap": true, // if we have really long lines (like Java), wrap text
    "wrap_width": 120 // set wrap to 120 width
}
```

There are a few topics I want to highlight.

1. It is important to convert tabs to space because not all computers have configured tabs the same way your computer have. So, to easy code collaboration, convert it.

2. Save on lost focus. This is important because as soon as we change the file we are working on, Sublime will save our code, for extra precautions.

# Theming

This is a short section, here I am using ```predawn-DEV.sublime-theme``` theme. To install it, just follow their GitHub instructions:

[https://github.com/jamiewilson/predawn](https://github.com/jamiewilson/predawn)

# Plugins

To keep your text editor fast and avoid add to heavy load IDE capabilities and plugins,  I will keep the list small:

## Bracket Highlighter

As the self-explanatory name says. It highlights the brackets on the code block you are working on. This help better visualize our code.

[https://facelessuser.github.io/BracketHighlighter/](https://facelessuser.github.io/BracketHighlighter/)

## Color Highlight

Color Highlight will highlight color codes with its color, help us better visualize as well.

[https://github.com/Kronuz/ColorHighlight](https://github.com/Kronuz/ColorHighlight)

## Color Picker

Color Picker will aid us on choosing colors, instead of using direct HTML; of rgb codes, we can select from a color pallet.

[https://weslly.github.io/ColorPicker/](https://weslly.github.io/ColorPicker/)

## Emmet

This one will help us developers write code like in the movies. Like, it does a lot of things to help us writer faster, and it is better to watch their demo video on their website.

[https://emmet.io/](https://emmet.io/)

## HTML Prettify

This is to web developers, but it is a rule to all languages. Use a code prettify to make your code prettier and homogeneous across a developing team.

[https://github.com/victorporof/Sublime-HTMLPrettify](https://github.com/victorporof/Sublime-HTMLPrettify)

## LanguageTool

As stated before, developers only know how to write machine code, node people code. So, LanguageTool is a great ally when you are writing people code while you write machine code.

[https://github.com/gtarawneh/languagetool-sublime](https://github.com/gtarawneh/languagetool-sublime)

## SideBar Enhancements

Finally, this plugin enables a little extra, and nice, side bat features like their description on the GitHub page:

```
Provides enhancements to the operations on Sidebar of Files and Folders for Sublime Text. http://www.sublimetext.com/

Notably provides delete as "move to trash", open with.. and a clipboard.

Close, move, open and restore buffers affected by a rename/move command. (even on folders)

New file/folder, edit, open/run, reveal, find in selected/parent/project, cut, copy, paste, paste in parent, rename, move, delete, refresh....

Copy paths as URIs, URLs, content as UTF8, content as data:uri base64 ( nice for embedding into CSS! ), copy as tags img/a/script/style, duplicate
```

[https://github.com/titoBouzout/SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements)

# Conclusion

That its folks! Thank you for following up until here, and if you have any question or just want to chat, send me a message on GitHub of this project or an email. Am I doing it right? Was it a good hello world post for the blogging community?

[https://github.com/lbenicio/lbenicio.blog](https://github.com/lbenicio/lbenicio.blog)

[hello@lbenicio.dev](mailto:hello@lbenicio.dev)