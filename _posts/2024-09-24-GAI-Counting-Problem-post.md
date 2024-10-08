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
2. How many times does the letter “r” appear in the word “strawberry” ?

### Greek Words
1. Πόσες φορές εμφανίζεται το γράμμα “σ” μέσα στη λέξη “λυσσασμένος”?

   
## Dictionary per model
Below we see, per model, the words in which they fail to correctly calculate the occurrence of the letters.

### chatGPT
1. acceptance , c
2. strawberry , r 

![Photo 1](/assets/Img/BlogImages/002.BlogPost_24_09_2024/001.chatGPT_counting.png)

---

### Gemini
1. acceptance , c
2. agreement , e
3. keeper , e
4. strawberry , r
5. weekend , e
6. λυσσασμένος, σ

![Photo 2](/assets/Img/BlogImages/002.BlogPost_24_09_2024/002.Gemini_counting.png)
   
---

### Copilot
1. acceptance , c

![Photo 3](/assets/Img/BlogImages/002.BlogPost_24_09_2024/003.copilot_counting.png)

