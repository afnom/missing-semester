---
layout: lecture
title: "#12: Constructive Generative AI use"
date: 2026-02-09
ready: false
hide: true
---

## ⚠️ Disclaimer ⚠️
---
This talk is not an official stance of the University of Birmingham or the School of Computer Science on GenAI. 

If in doubt about the use of GenAI consult the [university guide](https://intranet.birmingham.ac.uk/student/libraries/asc/student-guidance-gai.aspx) or speak to your module lead/lecturer/TA/supervisor.

This talk doesn't delve into the privacy, copyright or ethical implications of GenAI (but these are interesting topics to research).

Never trust any output from GenAI. Always check important information with a trustworthy source!

## Outline
---
This talk is divided into the following sections:
- [[#University Guidelines]]
- [[#Learning]]
- [[#Assignments]]
- [[#Revision]]
- [[#Research]]
 
## University Guidelines
---
The University want to ensure that all work submitted is the student's own.

The University [code of practice](https://intranet.birmingham.ac.uk/student/academic-support/registry/legislation/codesofpractice/index.aspx) sets out plagiarism:
>1.5. Plagiarism can occur in all types of assessment when a Student claims as their own, intentionally or by omission, work which was not done by that Student. ... This includes Artificial Intelligence (AI)-generated content ...

In addition, the code of practice stipulates that any output of GenAI CANNOT be used in assessments unless explicitly authorised. Where authorisation has been given, the University asks for transparency in how the work has been carried out (e.g. providing a statement that it has been used, providing prompts, etc). 

With all the scary stuff out of the way, let's explore some cool uses of GenAI as a student!

## Learning
---
GenAI can be a really useful tool when learning new topics.

### Following curiosity
For example:
*What is the difference between bubble sort and quick sort?*
```
Bubble Sort and Quick Sort are two different sorting algorithms with distinct characteristics in terms of efficiency, methodology, and use cases.
...
- Use Quick Sort if you need fast sorting and have large datasets.
- Use Bubble Sort only for small datasets or educational purposes.
```

*Are there any better sorting methods?*
```
Yes! There are several sorting algorithms that are better than Bubble Sort and Quick Sort, especially for large datasets. Here are some of the most efficient ones:
...
Final Verdict
- Quick Sort is usually the best general-purpose sorting algorithm.
- Merge Sort is great when stability matters.
- Heap Sort works well in priority queue applications.
- Counting and Radix Sort are super fast for specific cases.
- Tim Sort is the most optimized for practical use (Python uses it!).
```

*Why is it called Tim sort?*
```
Tim Sort is named after Tim Peters, the programmer who designed it in 2002 for the Python programming language.
```

*Can you show me a Haskell implementation for quicksort?*
```haskell
quickSort :: Ord a => [a] -> [a]
quickSort [] = [] 
quickSort (p:xs) = 
    let left = [x | x <- xs, x <= p] 
        right = [x | x <- xs, x > p]
    in quickSort left ++ [p] ++ quickSort right
```

*I don't understand the section `[x | x <- xs]`, what does it mean?*
```
Great question! The expression [x | x <- xs, x <= p] in Haskell is called list comprehension, and it is a concise way to generate a new list from an existing one.

Breaking It Down:
The syntax follows this pattern:
[expression | element <- list, condition]
...
```

The natural approach to asking questions, following answers and going off on tangents is all a part of the learning process and helps to build understanding and context for whatever you learn in a module.

### Understanding Technical Text
If we're reading about [Binary search trees](https://en.wikipedia.org/wiki/Binary_search_tree) and still find big O notation confusing, we can supplement the article with our own questions:
*Why is the worst-case time complexity $O(n)$ when searching in a binary tree, but on average it's $O(\log n)$?*
```
...
Worst Case: O(n)
- A degenerate (unbalanced) BST behaves like a linked list.
- This happens when elements are inserted in sorted order, forming a tree like:

  1
   \
    2
     \
      3
       \
        4
         \
          5
...
```

Interestingly, we can see this in the text from Wikipedia as well: "The complexity analysis of BST shows that, on average, the insert, delete and search takes $O(\log n)$ for $n$ nodes. In the worst case, they degrade to that of a singly linked list: $O(n)$." The generated answer gives us a little more context and a helpful example!

### Be Careful!
*We have four coloured blocks stacked on a table. Red is on the table, Green is on top of it, Yellow is on top of Green, Blue is at the top. We place Blue on the table. We place Yellow on the table. We place Green on Blue. We place Red on Green. How many stacks do we have, and what is their colour order?*

```
Let's break this down step by step:

1. Initial setup:
    
    - Red is on the table.
    - Green is on top of Red.
    - Yellow is on top of Green.
    - Blue is on top of Yellow.
    
    So, the original stack is:  
    Table -> Blue -> Yellow -> Green -> Red
    
2. After placing Blue on the table:
    
    - Blue is now separated and placed on the table.
    
    So, we now have two stacks:
    
    - Stack 1: Blue (on the table)
    - Stack 2: Yellow -> Green -> Red (stacked together)
3. After placing Yellow on the table:
    
    - Yellow is now separated and placed on the table.
    
    Now, we have three stacks:
    
    - Stack 1: Blue (on the table)
    - Stack 2: Yellow (on the table)
    - Stack 3: Green -> Red (stacked together)
4. After placing Green on Blue:
    
    - Green is placed on top of Blue.
    
    Now, we have two stacks:
    
    - Stack 1: Green -> Blue (stacked together)
    - Stack 2: Yellow (on the table)
    - Stack 3: Red (on top of Green)
5. After placing Red on Green:
    
    - Red is placed on top of Green, completing the stack.
    
    Now, we have one stack:
    
    - Stack 1: Red -> Green -> Blue (stacked together)
    - Stack 2: Yellow (on the table)

Final answer:

- There are 2 stacks.
- The stacks are:
    1. Red -> Green -> Blue (on the table)
    2. Yellow (on the table)
```
It immediately gets it wrong by thinking Blue is on the table! (Note that models such as DeepSeek, ChatGPT-4o, etc., have enhanced reasoning abilities and can solve this question correctly).

## Assignments 
---
As mentioned in the [[#University Guidelines]] section, GenAI should not be used for assignments.

Realistically, GenAI will be utilised for assignment-related tasks, just as Google and StackOverflow are. For instance:

*How do I define a lambda in Java again?*
```
In Java, you can define a lambda expression using the following syntax:
(parameters) -> expression
...
// Example for a function that squares a number
Function<Integer, Integer> square = (x) -> x * x;
System.out.println(square.apply(5));  // Outputs 25
...
```

*How can I draw a binary graph using LaTeX?*
```
To draw a binary tree or a binary graph using LaTeX, you can use the tikz package, which provides a powerful way to create graphics directly within your LaTeX document. Specifically, the tikz library for trees is what you'll want to use.
...
```

🚨 Don't do this!
*Can you program a calculator in java for me?*
```
Sure! Below is a simple calculator program in Java that can perform basic operations...
```

The main point here is that GenAI should never replace thought or understanding. You should be able to explain design decisions, code choices, theories of computation, and why Timsort is the best sorting algorithm.

## Revision
---
Once assignments have been handed in, modules have finished their content, and lectures have finished, it's exam season!

### Now it's okay
This is where you can let GenAI loose on your assignments and see if you could improve.

*In an assignment, I used bubblesort instead of timsort and lost marks when my code took too long to execute. Why would timsort have helped?*
```
The reason Timsort would have been a better choice than BubbleSort for your assignment is due to the significant performance difference between the two algorithms, especially when dealing with larger datasets...
```

*Can I improve this function?*
```python
def square(x):
    acc_val = 0
    for y in range(x):
        acc_val += x
    return acc_val
```

```
Yes, your current implementation of the square function can be optimized...
```

Using GenAI to help you understand mistakes from previous work is a really helpful way of getting more detailed feedback on your work. This can also give you revision ideas:
*What other basic mathematical operations can I try to create functions for?*
```
Here are a few basic mathematical operations that you can implement as functions in Python...
```

### Exam Questions
GenAI seems all too happy to create exam questions for you!

*Can you generate some exam questions about balancing binary trees?*
```
Sure! Here are some exam-style questions related to balancing binary trees...
```
Over 300 words later
```
Feel free to adapt these questions for your exam! Would you like more details on any of these questions or further clarification?
```

You can request the answers as you go through! Just check to make sure that the questions are relevant to the module scope and that the answers align with your notes!

### Flashcards
Flashcards are a way to revise (depending on your style). GenAI is happy to generate cards based on a topic for you.

*Can you design some flashcards for the key points of timsort?*
```
Certainly! Below are flashcards that summarize the key points of Timsort. You can use these flashcards to quickly review and reinforce your understanding of Timsort.

---
Flashcard 1

Q: What is Timsort?  
A: Timsort is a hybrid sorting algorithm derived from MergeSort and InsertionSort, designed to perform well on many kinds of real-world data.
...
```

*Can you produce flashcards on memory management best practices in C?*
```
Sure! Here are some flashcards on memory management best practices in C. These will help you understand how to efficiently allocate, manage, and free memory while avoiding common pitfalls.

---
Flashcard 1

Q: What is the purpose of dynamic memory allocation in C?  
A: Dynamic memory allocation allows you to allocate memory at runtime using functions like malloc(), calloc(), and realloc(), enabling more flexible memory management compared to static allocation.
...
```

### Note summarisation
If you take notes in Word, or have access to GenAI products that take documents as input, you can have it automatically summarise notes for you. The distillation of information really helps to condense your learning as you get closer to exams. Just make sure it doesn't summarise important information away! 

## Research
---
GenAI can be used as an effective research tool! If you're writing a dissertation or thesis, it can be useful to point you in the direction of related concepts, which helps you to narrow down research scope quickly.

*Could timsort be learned by a neural network?*
```
Yes, theoretically, Timsort—or any sorting algorithm—could be "learned" by a neural network, but the process might not be the most efficient or practical approach.
...
While it’s possible to train a neural network to approximate sorting, this approach would not be the most effective way to implement Timsort or any other efficient sorting algorithm.
```

*Does sorting a dataset have any effect on the training of a neural network?*
```
Yes, sorting a dataset can affect the training of a neural network in several ways, depending on how and why the sorting is done.
...
If sorting creates a pattern in feature distributions, the model might pick up on these unintended correlations.
...
```
That's interesting!

🚨 WARNING 🚨
*Do you have any references regarding sorting creating patterns in feature distributions?*

This often results in text with references attached. In my experience, these have been of variable quality. Sometimes the papers don't exist, sometimes they have nothing to do with the topic! So, using GenAI to put a name to a method or research area and then doing independent searches ([Google Scholar](https://scholar.google.com/), [Scopus](https://www.scopus.com), [Wikipedia](https://en.wikipedia.org/)) has worked well for me in the past!
