---
layout: post
title: a week of clojure as a total beginner
date: 17-03-2023
description: >
  Googles 'how to install closure'. Clicks on 'Show instead "how to install clojure"'.
tags: clojure learning technology beginner journey
categories: clojure-7-days
giscus_comments: true
---

### Context

- I have never used `clojure` before.
- I will commit at least 1 hour per day to learning.
- I will keep a log of how much time I am spending.
- I will be honest about my approach and thoughts.

### Setup

Where to start?

1. **Google "how to install closure"**
2. **Click on "Show instead 'how to install clojure'"**

After poking around the many different ways to get **clojure** installed - and some failed attempts with WSL2 🥴 - I found that vscode has an extension/environment - [Calva](https://calva.io/).

This was a no-brainer. Though I am also interested in learning [Emacs](https://www.gnu.org/software/emacs/), [Vim](https://www.vim.org/), and the numerous other editors out there (and I will, I promise), I am already familiar with [vscode](https://code.visualstudio.com/), and I'm here to give **clojure** my best effort and full attention.

Installing Calva is easy. Below is the one-liner, or you could use the UI.

{% highlight bash %}
code --install-extension  betterthantomorrow.calva
{% endhighlight %}

It is also admirably beginner friendly, with an interactive getting-started guide that has a witty style. To start this, press`ctrl`+`shift`+`p`, type: `calfig`, press `enter` (you will learn more about these tasks in the guide!).

Wow. This looks nerdy 🤓. I'll wait to get stuck in; first, some reading:

- <https://clojure.org/guides/getting_started>
- <https://clojure.org/guides/structural_editing>
- <https://clojure.org/guides/repl/introduction>

Okay, I wish I didn't read them - even though I know it is essential, it made little sense at this point 🤷‍♂️. It took a lot to follow, especially for a newbie in this type of programming. In retrospect - *this blog was written up a day later* - I should have completed the getting-started guide first. A little warm-up is advised.

### Actually getting started

Ah, this is better.

My initial thoughts are this is ~~beginner~~ friendly 🙌. Can programming languages be inclusive for newbies? 🤔 I already feel like I am being lured in and want to learn more. As I spend the next ~40 minutes working my way through the introduction.

There are lots of shortcuts worth learning. Below 👇 are the top 4 that I most use so far. I'll continue to post these as I discover them.

- `alt`+`enter`: evaluate top level form.
- `ctrl`+`enter`: evaluate current form.
- `ctrl`+`alt`+`c`, `enter`: load / reload current file.
- `escape`: remove all evaluated results.

### Wrap up

The introductory journey is longer than I thought. It's been a very late hour. I'm still here and even more invested. Have I learned anything? Only that I understand less. I have no idea what this would ever be used for - but that's okay. Trust the process.

Let me know your thoughts and recommendations! Apologies if I mistype or misunderstand. I am always open to feedback and corrections.

I'll check back in tomorrow, thanks for reading 🙏
