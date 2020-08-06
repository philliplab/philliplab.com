---
title: "Finding Things"
date: 2020-08-06
description: "File searching in Linux"
type: "post"
tags: [Linux]
draft: true
---

When looking for a misplaced file I always first try
```
locate something | grep "something else" | sort
```

As convenient as `locate` may be, `find` will always be the tool for serious searching. It is jam packed with features (10k+ word man page), has over [4 decades of development behind it](https://en.wikipedia.org/wiki/Research_Unix) and it does not fear an [endless horde](https://unix.stackexchange.com/questions/120077/the-ls-command-is-not-working-for-a-directory-with-a-huge-number-of-files) [of files](https://unix.stackexchange.com/questions/38955/argument-list-too-long-for-ls).

The downside of all the features and the age of find is a ton of complexity and a quirky interface.

This post describes some of the find commands that I find most useful:

- Restricting the search based on file name.
- Sorting files based on size.
- TODO: Sorting files based on [cma]time.
- TODO: Filtering files based on [cma]time.

### Restricting the seach based on file name

The base find command will just list all files in the current folder and all its subfolders. By using the -name option, you can restrict your search.

```
find . -name *.txt
```

Using `-iname` instead will make it case insensitive.

### Sorting the files based on size

`find` provides an option that allows you to format the output using a very rich set of directives. By printing one file per line and starting the line with the file size in bytes, the result is easily sortable.

```
find . -name '*.txt' -printf '%s\t%p\n' | sort
```

The `%s` directive specifies the file size in bytes, while `%p` is the file name. These two are separated by a tab and different files are separated by a newline.
