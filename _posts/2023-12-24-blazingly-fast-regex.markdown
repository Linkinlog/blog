---
layout: post
title:  "Blazingly fast file searching ft. Regex"
date:   2023-12-24 18:33:05 -0500
categories: regex files
---

Today I wanted to talk about some quick and easy tricks we can utilize to gain an impressive amount of speed when searching through files / text. Often, regex is seen in a negative light when used in code bases as it can far from straightforward to read and can lead to some unplanned results. However, as intended we can use regex for much better things, such as file / text searching.

Regex doesn't have to be confusing. In my personal experience I would say that regex should be avoided at all costs in production code, as the closer you get to the perfect regex for the use case, the farther we get from readability and maintainability, however for file searching we can afford to get a little dirty, as 10 extra results in the file picker is a lot less problematic than 10 extra results in an email filter. When searching through files we can get quite far with just `.*`, as we will see. Also, with regex being built in to most modern IDE/editors (and even in the terminal), we are never far from utilizing it either.

Today I will go my top simple regex patterns that have helped me countless times when trying to find something in a codebase. We will start out with the big and bad 

## The infamous `.*`

`.*`, simply put grabs anything and everything. You see, the `.` character represents any character except a newline. When we pair it with `*` it says we want any character except a newline, *and* we want at least 0 instances and as many instances until the next search pattern. While we are on the topic, let's discuss `+`, `?`, and `{}`. With these we can get a little more granular than `*`. `+` means at least 1, `?` means 0 or 1, and with `{}` you can specify how many there should be, such as `bo{2}ks` will search for 2 `o`s. Now, the ways this will become useful are vast, yet a quick example would be when you know the beginning and end of the text but not the middle.

Say you are looking at your frontend and need to find the text, "Pay $5.00/mo for 5 mo today". If we were to search "Pay", we might find a large amount of results, same as if we searched for "today". Assuming our page is dynamic, the price and frequency isn't likely to even exist in our codebase. Here comes `.*` to the rescue. With `Pay.*today` we are able to narrow down our results drastically. Now, let's also assume that the logic for the price and frequency are handled in line and just inserted into the price string with some form of concatenation, we should be able to find the code we're looking for with ease.

## Grouping with `[]`

`[]` allows us to group characters that we do (or don't) to match. Let's say we're working in a codebase with...less than uniform coding styles. Brian uses snake_case, Peter uses kebab-case, and now you need to find all instances of `my_var` and `my-var`. With regex this is easily tackled with `[]`, we can employ `my[-_]var` which will find all results of both cases. For a bonus, if another developer uses `myvar` for some reason, we can use the `*` we learned about already for a final `my[-_]*var` pattern.

Now I mentioned we can group characters we don't want to match, this is achieved via the `^` character. Let's say there's another developer who just wants to watch the world burn, they use every symbol possible as their casing, except for spacing as "languages don't like that" they say. We can throw a `my[^ ]var` at it and achieve just that, match any character that isn't a space. For more flexibility, most of the time we can also utilize some regex built in's such as `\s` which symbolizes a space character and when we change the case, `\S` symbolizes excluding a space character, resulting in `my[\S]var`. We can go a step further with our previous example and say that on top of not being a space, it also wouldn't be a digit or character a-z, right? That is as easy as `my[\S\W\D]var`, `\S` we know, `\W` translates to any character that isn't an alphanumeric character, and `\D` translates to any character that isn't a digit.

## This or that with `|`

`|` is a quick one, it simply allows us to do a logical or when searching. One easy example would be if you have some contributors using "color" and others using "colour". With regex this is yet again a breeze. We can attack this with `color|colour`, simple enough right? This simple trick can be quite helpful when dealing with less than clean data.

## Setting boundaries with `\b`

`\b` is another quick and helpful one, let's say we want to find "file search", yet we have a hilarious amount of "profile search" blocks. That can be tackled with a simple `\bfile search\b` which will block out all results of "profile search". Another useful example would be prefixes/suffixes, we may want to search for "pre" as in "preliminary" yet want to negate things like "apprehensive", we can employ `\bpre` for that, and the same for suffixes, if we want to find "fix" like in "suffix" and not in "fixed" we can use `fix\b`.

## Grouping with `()`

`()` can be quite in depth, it allows us to grab certain aspects of the match and utilize it, usually for replacing. Personally, I really find it useful when dealing with casing. Let's say you need to find all instances of "Hello World", and "hello world". We can do that by grouping the case-sensitive characters, a simple example would be `(.)ello (.)orld`. This translates to "grab the first character of the pattern `.ello`, and `.orld`", which `.` translates to any character as we discussed. `.` may be a little greedy, yet again this isn't production code, we can afford to be a little greedy. From there we can pair it with our next skill, replacing.

## Replacing characters with `/`

When searching with regex, often we are able to replace the match with another by adding a `/`, this combined with the power of grouping means we can get into some fancy stuff. Depending on where you are executing this regex we can use `$X` or `\X`, where we replace `X` with the number that corresponds to the match. Let's change all instances of "Hello World" and "hello world" with "Howdy Walter" and "howdy walter" respectively. This is simply achieved with `(.)ello (.)orld/$1owdy $2alter`.

## Other goodies

We found in the last section that we can add a `/` to do replacing, what else might we be able to do after we specify the match? We can set `/i` to ignore case, `/g` to make the search global, and `/c` to confirm before each replace. Therefore, we can combine this to be `(.)ello (.)orld/$1owdy $2alter/igc/`. Also, we can use `$` and `^` to dictate the end and beginning of lines, the `$` symbol matches the end of the line, and the `^` symbol matches the beginning. 

With these few tricks you will surely be flying through the codebase and finding things faster than ever before. The world of regex is vast and confusing, however with just a few patterns on our tool belt we can achieve blazingly fast results. Keep on hacking!
