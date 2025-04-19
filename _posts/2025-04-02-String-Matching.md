---
layout: post
title: "String Matching"
description: "This post provides an overview of string matching algorithms covered in the SC2001 course in NTU, including a straightforward solution, the Rabin-Karp Algorithm and the Boyer-Moore Algorithm"
date: 2025-04-02
feature_image: images/string-matching.png
tags: ['algorithm-design']
---

String matching is a fundamental problem in computer science, which involves **finding the first occurrence of a given pattern in a text**. This problem arises in many applications, such as searching for a character string in a text, finding a pattern in DNA sequences, decoding graphical or audio data, or searching for a sublist in linked lists. In this post, we will explore three different approaches to solving the string matching problem: a straightforward solution, the Rabin-Karp Algorithm, and the Boyer-Moore Algorithm. Also, this post is a lecture notes of the `SC2001` course in NTU, covering common string matching algorithms.

<!--more-->

Before we start, let's define some notations:
- $T$ is the text string
- $P$ is the pattern string
- $n$ is the length of $T$, $k$ is the position index of $T$
- $m$ is the length of $P$, $j$ is the position index of $P$

## Table of Contents
- [SimpleScan Algorithm](#simplescan-algorithm)
- [Rabin-Karp Algorithm](#rabin-karp-algorithm)
- [Boyer-Moore Algorithm](#boyer-moore-algorithm)


---

# SimpleScan Algorithm

```cpp
int SimpleScan(char P[], char T[], int n, int m) {
    int i = 0;  // the current guess where P begins in T
    int j = 0;  // the index of the current character of T
    int k = 0;  // the index of the current character of P

    while (j < n) {
        if (T[j] != P[k]) {     // mismatch
            j = ++i;            // skip to the next guess
            if (i > n - m) {    // not have enough characters
                break;
            }
            k = 0;              // reset the index of P
        } else {                // match
            j++;
            k++;
            if (k == m) {       // found
                return i;
            }
        }
    }
    return -1;
}
```

Comparison starts with $k=0$ and $j=i$. When $k$ reaches $m$, all characters have been compared and matched.

When a mismatch happens, shift the pattern right one position: `j=++i, k=0`

The worst case happens when the pattern appears right at the end of the string, and each character in the text must be compared with each character in the pattern. This results in a time complexity of $O(mn)$, where $m$ and $n$ are the lengths of the pattern and the text respectively. For example, if the pattern is "aaaa" and the text is "aaaaaac".

Furthermore, we can eliminate variable $i$ by using the fact $i = j - k$. This modification also mentioned in the tutorial.

---

# Rabin-Karp Algorithm

---

# Boyer-Moore Algorithm

---