---
layout: post
title: "Handling Corrupted File Names in Linux"
date: 2025-05-24
categories: blog
author: "Eugene Mazarakis"
tags: [Linux, display files, cat]
---


# Introduction
Many times in your job as a data engineer, you need to manage files on Linux. Sometimes these files may have strange names, such as 'test file name' or '-fileTest000'. 
How can you display these files in Linux?

In the linux folder we have the following two files:
1. -fileTest000 (dash in front of the name)
2. test file name (spaces between the words)

![Photo 0](/assets/Img/BlogImages/010.BlogPost_24_05_2025/Working_Directory.PNG)   


## Solution

**-fileTest000** 

When you run the command cat -fileTest000, it doesn't display the contents of the file. It interprets the '-' as an option for something, as shown below.
So, if you want to display the contents of the file, you should provide the full file path. One of the ways to do that is the following:

**cat "$(pwd)/-fileTest000"**

![Photo 1](/assets/Img/BlogImages/010.BlogPost_24_05_2025/cat_print_file.PNG)
