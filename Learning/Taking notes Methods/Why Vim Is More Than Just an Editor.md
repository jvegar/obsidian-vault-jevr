Throughout my time as a developer, I've used VS Code, Sublime, Notepad++, TextMate, and others.But shortcuts like `cmd(+shift)+end` and jumping `option+arrow-key` from word to word needed to be faster at some point.

I was hitting my limits. Everything I was doing I did decently fast, but I didn't get any faster.
I've since learned that Vim is the only editor that you get faster using with time.

[Vim] is based solely on shortcuts. When I discovered that and played around a bit, I felt numb and a little stupid, having not learned the shortcuts (called Vim language) much earlier in my career.

I realized there was a keystroke to get to any specific position I wanted to jump. It was like a game. seeing if I could use fewer shortcuts to accomplish a particular edit. It's where many Vim users get a lot of pleasure from coding and writing. It felt liberating, moving my cursor with the precision of a surgeon.

Although speed is a smaller benefit, it got me started when I saw others navigating in Vim. After climbing the steep learning curve, it's still one of the most powerful skills I've ever learned in my career, working for a living on a computer.

Let's debunk the myth of Vim and learn how it's possible to remember all the shortcuts using the specific Vim language. We'll see how to move with vim motions, and I'll share what  I've learned so far, and why you might give Vim a try as well.

# Learning The Vim Language
Lots of things have been said about Vim - how fast it is, how only Linux nerds use it, and that it's impossible to exit Vim.

For myself, I fell in love with then "Vim language". You see, I'm bad at remembering anything and thought that Vim was not for me. But this wasn't the case for one specific reason: Vim **motions** and its language.

I learned that there's a grammar behind the editor. With it, you express what you want to do first, how many times, and then what you want it to apply.

Let's get deeper into Vim and the language behind it.

#### How the Vim Language and Motions Work
Vim has a terrific language or grammar behind its shortcuts. Instead of remembering a thousand shortcuts, you can learn a couple and combine them.

These are often called the Vim language or Vim motions for moving around. This has nothing to do with the editor yet - these are universal and available in other editor as well.

For example, there's VSVim for VSCode, IdeaVim for the JetBrains products, Vintage Mode for Sublime, and so on. But there are laso Browser extensions like Vimium of Firenvim, and Gmail eve adapted some of Vim's shortcuts for navigation (`j`, `k` for moving, `g` for jumping).

Everyone who types on a computer eight hours a day should learn the Vim language. Yes, it's hard in the beginning, but that's the case with everything new and different. But getting better every day and having more fun coding or writing should be motivation enough. You're not too busy to learn - you'll learn as you go. 
![[Pasted image 20250613014741.png]]
#### Vim Grammar
Just as spoken language **grammar** has verbs, subjects, and objects, so does the Vim language. The grammar has different **verbs** to begin with. Copying (or yanking) in Vim with `y`, deleting with `d`, pasting with `p`, changing with `c`, and so on.

For example, the easiest shortcut is copying a line with `yy`. In this case, yank is the verb and the second `y` is a synonym for `y_`. The `y` is doubled up which makes it easier to type since it's a joint operation.

Next, we can add movements. Each verb takes a **subject** for their movements. There are lots of movements (more in the next section) - the easiest is with numbers.

For example, to copy three lines, you add a 3 in front, such as `3yy`. You can do that with all verbs, like deleting three lines is `3dd`. Another would be `{` and `}` to move to the beginning or end of the paragraph, respectively.

In addition to verbs and subjects, the Vim language also has **objects**. For example, we can save text into different clipboards (called a register in Vim) with `"ay`. Here, we copy it into register a, which would be the object. We can paste it again by doing the same but using the verb paste instead of yank `"ap`.

There are even **adjectives** and **adverbs** with prefixes. Usually, you use a verb and an object. But instead of going down three lines with `3J`, which joins the following three lines, you could add `d5}`, which means "delete from the current line trough the end of the fifth paragraph down from here".

For me, the most magical thing about Vim is how you navigate and edit text - and it still has nothing to do with the editor.

Sure Vim was the one that introduced and perfected these actions, but again - you can get them anywhere else. This goes deep into the Vim language, yet we still need to touch on the editor. This is important to know.

I hope you've started seeing the power of such patterns, though. With a couple of verbs and objects, you can already know hundreds of combinations without memorizing each one individually.

You can watch a video on [Mastering the Vim Language](https://youtu.be/wlR5gYd6um0?ref=ssp.sh) or read a full exposition of the Vim language on this terrific [StackOverflow](https://stackoverflow.com/a/1220118?ref=ssp.sh) comment.