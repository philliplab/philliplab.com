---
title: "Finding Things"
date: 2020-08-06
description: "File searching in Linux"
type: "post"
tags: [Linux]
draft: false
---

When looking for a misplaced file I always first try
```
locate something | grep "something else" | sort
```

As convenient as `locate` may be, `find` will always be the tool for serious searching. It is jam packed with features ([10k+ word man page](https://man7.org/linux/man-pages/man1/find.1.html)), has over [4 decades of development behind it](https://en.wikipedia.org/wiki/Research_Unix) and it does not fear [overful](https://unix.stackexchange.com/questions/120077/the-ls-command-is-not-working-for-a-directory-with-a-huge-number-of-files) [directories](https://unix.stackexchange.com/questions/38955/argument-list-too-long-for-ls).

The downside of all the features and the age of find is a ton of complexity and a quirky interface.

This post describes how to use find to:

- Restrict the search based on file name.
- Sort files based on size.
- Sort files based on [ma]time.

### Restrict the seach based on file name

The base find command will just list all files in the current folder and all its subfolders. By using the -name option, you can restrict your search.

```
find . -name '*.txt'
```

Using `-iname` instead will make it case insensitive.

### Sort the files based on size

`find` provides an option that allows you to format the output using a very rich set of directives. By printing one file per line and starting the line with the file size in bytes, the result is easily sortable.

```
find . -printf '%s\t%p\n' | sort -n
```

Explaining the command passed to `printf`:

- `%s` - The file size in bytes.
- `\t` - Separate the file size and name with a tab.
- `%p` - The file name. 
- `\n` - Put each file on its own line.

To force `sort` to sort based on numbers instead of alphabetically, use the -n flag.

### Sort the files based on access of modification time

```
find . -printf '%T+\t%p\n' | sort -n
```

Explaining the command passed to `printf`:

- `%T` - The file's last modification time. Change this to `%A` for access time ([not always supported](https://www.linuxquestions.org/questions/linux-software-2/what-is-noatime-how-can-i-mount-a-partition-with-noatime-393617/))
- `+` - Format for the time. `+` means the date and time separated by a +. Change this to `@` if you are worried about the date+time not sorting correctly.
- `\t` - Separate the time and name with a tab.
- `%p` - The file name. 
- `\n` - Put each file on its own line.

To force `sort` to sort based on numbers instead of alphabetically, use the -n flag.

## More information

[Find man page](https://man7.org/linux/man-pages/man1/find.1.html)

[opensource.com - How to use FIND](https://opensource.com/article/18/4/how-use-find-linux)


