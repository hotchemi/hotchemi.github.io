---
title: "A polite way to open a bug report"
date: 2017-04-18
draft: false
tags: ["oss", "lang-en"]
---

I assume that as your GitHub repository becomes popular you’ll get more and more issues especially bug repot from unexpected angle. And the opposite is true, presumably you’ve opened several reports to libraries or tools that you use as you’ve been becoming being active as an open source enthusiast.

In this context what I wanna note is that opening a bug report roughly and innocently may consume project maintainer’s precious time, and even irritate them.
I guess nobody wants to make them being such but sadly it repeatedly happens in real life.

Say, what can you do to avoid such an unavailing ceremony? As an answer I’d like to share something what I always take care about and learnt from my open source maintain experience.

## Search by keyword before open

Okay, you just encountered a problem, and it basically means someone might have encountered the same problem before.
What you should do at first is searching by keyword on GitHub. GitHub’s search feature covers issue, pull request and even Wiki. You might find the answer pretty instantly without waiting any response from maintainer, why don’t you do?

ref: [https://help.github.com/articles/searching-issues/](https://help.github.com/articles/searching-issues/)

## Structure content

Secondly, you need to take care not to write a long and massive sentence in an issue.
It’s not place to put a novel, maintainer uses their limited time to improve their product and help user, so that you’ve got to make the content well structured and try to make it easy to understand in a glance.

I know there’s a lot of style, but personally I prefer the following format.

    ## Overview

    Describe a bug content briefly…

    ### Expected

    Note expected behavior…

    ## Environment

    Using library ver, OS ver and so on…

    ## Reproducible steps

    It’d be perfect to put sample project, but at least note steps…

Note that in addition to the user’s effort, maintainer team can provide issue format by adding issue template to the repository.

ref: [https://github.com/blog/2111-issue-and-pull-request-templates](https://github.com/blog/2111-issue-and-pull-request-templates)

## Format stack trace

Fortunately GitHub adopts markdown based powerful language as a standard format to describe something. It means that you can use block style to surround exception stack trace to make it readable like below.

    Caused by: java.lang.ClassCastException: Cannot cast class

    org.gradle.api.internal.changedetection.rules.DescriptiveChange to interface

    org.gradle.api.tasks.incremental.InputFileDetails

You might feel that’s a tiny point, but imagine that pretty long issue and most of the content is not formatted exception stack trace…who wants to read such an obfuscated spell? It depresses the motivation of maintainer and as a result the issue will become being left for a long time…yack!

So please make sure to use block style to put exception stack trace.

## Express your gratitude

How emotional this section is? But actually it’s so important because we don’t report AI to a bug.
Essentially contributing to an open source, even it’s just a bug report, is a communication between software developers. Who is gonna be depressed when being thanked by someone? Even he/she is a kind of wooden developer it’s the same. You’re getting incredible benefit from the product, it’s worth to express your gratitude right?

## Conclusion

By taking care of the following points your open source life will be getting exciting and fulfilling, even your report doesn’t hit the point.

* Search by keyword before open a report

* Structure content to make it readable

* Make sure to format exception stack trace

* Express your gratitude!

Happy coding!
