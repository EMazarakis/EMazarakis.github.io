---
layout: post
title: "Generative AI's Achilles' Heel: The Challenge of Letter Counting"
date: 2024-09-24
categories: blog
author: "Eugene Mazarakis"
tags: [AI, GAI, LLMs, Gemini, chatGpt, Copilot]
---


## Introduction

The Devil's Dictionary is a satirical dictionary written by American journalist Ambrose Bierce, consisting of common words followed by humorous and satirical definitions. 

Today, we will try to write our own dictionary, The AI's dictionary.
This dictionary will consist of common words in which AI models failed to accurately count the occurrences of a certain letter within a word.
We will examine three "LLMs": Gemini, chatGPT, Copilot.


## The question

Let’s define the prompt for the models:

Prompt: How many times does the letter **“x”** appear in the word **“XXXX”** ?

XXXX: Word
X: Letter

This is the prompt we provide to Gemini, Copilot, and ChatGPT. We aim to evaluate their ability to count the occurrences of the letter 'x' within the word XXXX.

## Examples

### English Words
1. How many times does the letter “c” appear in the word “acceptance” ?
2. How many times does the letter “r” appear in the word “grapefruit” ?
3. How many times does the letter “r” appear in the word “strawberry” ?

### Greek Words
1. Πόσες φορές εμφανίζεται το γράμμα “σ” μέσα στη λέξη “λυσσασμένος”?

   
## Dictionary per model
Word , Letter

### chatGPT
1. acceptance , c
2. grapefruit , r
3. strawberry , r 

### Gemini
1. acceptance , c
2. agreement , e
3. aggressiveness , s
4. assassinate , s
5. grapefruit , r
6. keeper , e
7. strawberry , r
8. weeded , e
9. weekend , e


### Copilot
1. acceptance , c
2. dumbfounded , d
3. grapefruit , r
4. strawberry , r

