---
layout: post
title:  "Compiler Design Prereqs - Sets & Regular Expressions"
categories: compilers
---

I'm taking the [Compilers](https://www.coursera.org/course/compilers) online course from Coursera and in Lesson 03-03 Regular Languages I realized I didn't remember any of the set notation or rules for building regular expressions so I read up on them and made the following notes for myself, enjoy.

Warning: This post uses several Unicode characters. I've listed the Unicode values in most cases but if you can't see all the symbols correct try using a different font.

A programming language is a collection rules that govern the grammar and vocabulary of text documents which are authored by humans to create programs.

Regular expressions are a notation used to describe certain types of languages. Not all types of languages can be described by regular expressions, those languages that can be described by them are known as regular languages.

A regular expression represents patterns of text which can appear in a string. The regular expressions you find within programming languages such as Java or Perl have many advanced features, we only need a subset of those features for our purposes.

The most basic regular expressions are:

* Single character - If the only regular expression for a language was 'a' then you could have a single a which a text document. Not two a's or an 'a' and a 'b', just a single 'a'. Support for optional occurrences and multiplicity are shown below.
* Empty string - "". This is different than having nothing or no strings, this is an empty string.

Note: Often the Greek letter Epsilon (ε or Unicode U+3B5) is used to represent the empty string.

These basic regular expressions are the building blocks used by compound regular expressions. Compound regular expressions enhance the base regular expressions and add the ability for you to say things like "I want to allow 'a' or 'b'", "I want 'a' followed by 'b'", "I want 'a' followed by an unlimited number of b's".

Before discussing compound regular expressions that can be used to show something like "I'd be happy with an 'a' or a 'b'" you need to understand some of the notation and terminology that will be used to describe the behavior of a regular expression.

Basically the goal of a regular expression is to describe a collection of things (text in this case)  that you are interested in. The term for "a collection of things" that we will use is a "set". Set theory is actually a specific branch of mathematical logic (see <https://en.wikipedia.org/wiki/Set_theory>). An object that belongs to a set is said to be a "member" of that set.

In text, a set is often represented by curly braces. For example the set of the numbers  1, 3, and 5 would be shown as \{2,3,5\}. You can have sets of sets \{\{2,3\},\{3,5\}\} and empty sets \{\}.

Sets can be combined in different ways. For example you might want to say I accept anything that is in the \{2,3,5\} and \{5,9\} sets, meaning you accept \{2,3,5,9\}. You might say I only want to accept things that are in both the \{2,3,5\} and \{5,9\} sets, meaning you accept \{5\}. You may have seen these type of set relationships represented with Venn diagrams (see <https://en.wikipedia.org/wiki/Venn_diagram>) which use circles to represent sets. We will use text symbols to show the different relationship types.

Common relationships between sets include:

* Subsets - A set is a subset of another set when it has a smaller number of elements and all its elements appear in the larger set. The set \{2,3\} is a subset of the set \{2,3,9\}. Technically a subset doesn't have to be smaller, it could be exactly the same but most people mean smaller when discussing a subset. A "proper" subset must be smaller.
* Supersets - A set is a superset of another set when it has a larger number of elements and it contains all the elements that appear in the smaller set. The set \{2,3,9\} is a superset of \{2,3\}. Just like subsets, superset sets technically don't have to be bigger but that is what most people mean. A "proper" superset must be bigger.
* Power sets - If you took the time to list out all the possible subsets of a set and put each one of those sets in a new set, that new set would be a power set. For the set \{2,3,9\} the power set would be \{\{2,3,9\},\{2,3\},\{3,9\},\{2,9\},\{2\},\{3\},\{9\}, \{\}\}
* Intersections - Used when you want to capture "shared" elements that exist your sets, given the sets \{2,3,5\} and \{5,9\} the intersection is \{5\}.
* Union - Used when you want to capture any element in your sets whether they are shared or not, given the sets \{2,3,5\} and \{5,9\} the union is \{2,3,5,9\}.
* Symmetric differences - Used when you want to capture non-shared elements that exist in your sets, given the sets \{2,3,5\} and \{5,9\} the symmetric difference is \{2,3,9\}.
* Relative Complements - Used when you want to capture the elements that exist in one set that are not shared with another set(s).  The relative complement of \{2,3,5\} in \{5,9\} is the set \{9\}.
* Absolute complements - Used when you want to capture anything that is not is a set. Given the entire universe of integers, the absolute complement of \{2,3,5\} would be any integer that was not a 2, 3, or 5. The term "universe" is used in set theory to mean all possibilities.

Symbols used to convey set concepts:

* Empty set - Shown with \{\} but is often shown using the symbol ∅ or the Unicode value U+2205.
* Member of - ∈ or Unicode value U+2208. To say that 2 is a member of the \{1,2,4\} set, write 2 ∈ \{1,2,4\}.
* Not a member of = ∉ or Unicode value U+2209. To say that 3 is not a member of the \{1,2,4\} set, write 3 ∉ \{1,2,4\}.
* Subsets - ⊆ or Unicode value U+2286. \{2,3\} ⊆ \{2,3,9\}. For "proper" subsets use ⊊ or Unicode value U+228A.
* Supersets - ⊇ or Unicode value U+2287. \{2,3,9\} ⊇ \{2,3\}. For "proper" supersets use ⊋ or Unicode value U+228B.
* Power sets - Instead of listing out a set of all the possible subsets of \{2,3,9\} you just write P(\{2,3,9\}).
* Intersections - ∩ or Unicode value U+2229.
* Unions - ∪ or Unicode value U+222A.
* Symmetric differences - ∆ or Unicode value U+2206. The members of set A which are not in set B combined with the members of set B which are not in set A can be shown as A ∆ B = (A ∖ B) ∪ (B  ∖ A).
* Relative Complements - ∖ or Unicode value U+2216. The relative complement of A in B is B ∖ A. Meaning you want the things in B that are not also in A.
* Absolute complement - Given a set labeled A the absolute complement of A = everything in the "universe" except A. The "universe" is shown with a "U" in set notation (don't use it for anything else). So you can say the absolute complement of A = U ∖ A. Also shown with a superscript 'c' appended to the set [name].

When talking about the properties of a set you often select a symbol to use as an identifier for a set instead of having to repeatedly list out the members of a set. We could say that the symbol 'A' will represent \{2,3\} and the symbol 'B' will represent \{2,3,9\}. Now we can say that A ⊆ B and B ⊇ A. In this case the result of A ∩ B = A since A is a subset of B.

The curly braces used to contain a set's members come from set-builder notation (see <https://en.wikipedia.org/wiki/Set-builder_notation>). Set-builder notation is a mathematical notation used to describe the properties of a set. In set-builder notation you can use ellipses to represent an obvious sequence. For example \{2,3,5,7,11,...\} could represent the set of prime numbers while \{1,2,3,...,10\} could represent the numbers 1 through 10.

You can show the cardinality or count of a set members use vertical bars. If you have \{1,2,3,...,10\} assign A to identify the set or A = \{1,2,3,...,10\}, then |A| = 10 because there are ten members in the set.

If you want to support a single character of 'a' followed by the character of 'b', allowing 'ab', then you must use a compound regular expression. In the example of 'ab' the type of compound regular expression is called concatenation. Concatenation is shown simply placing one regular expression after another regular expression. The expressions used could be base regular expressions or they can be compound regular expressions.

Formal set-builder notation can be used to describe the behavior of compound regular expressions. A compound regular expression is used to combine regular expression to make more descriptive rules. In formal set-builder notation a set has three pieces. The first piece is a variable name, the second piece is the "such that" symbol, the third is the rule.

Set-builder notation follows the structure \{var|rule\} or \{var:rule\}. The vertical bar (pipe) and the colon both mean "such that".

There are three compound regular expressions:

* Unions - The + symbol can be used to create a union between two regular expressions. Given the base regular expression A and the base regular expression B, if you said A + B then you are saying you are looking for the union of the members of set A with the members of set B. In set-builder notation this is \{a|a∈A\} ∪ \{b|b∈B\}. In English; 'a' such that 'a' is a member of the 'A' set and 'b' such that 'b' is a member of the 'B' set.
* Concatenations - To require one matching regular expression followed by another you just follow one expression immediately with another, no symbol is used. AB = \{ab|a∈A ∧ b∈B\}. In English; 'a' followed by 'b' such that 'a' is a member of the 'A' language AND 'b' is a member of the 'B' language. The AND symbol (∧) is Unicode value U+2227.
* Iterations - The * or star symbol. The Kleene star or Kleene closure. A* means accept the union of A to any power or \{\{\},\{a|a∈A\},\{aa|a∈A\},\{aaa|a∈A\},...\}

To create a language you have to define the smallest components of the language, its alphabet. Once you define the alphabet of a language you then define the allowed patterns of the alphabet which can be used in the language. If you can use regular expressions to specify all the patterns of a language then that language is a regular language.

The Greek letter sigma (Σ) or Unicode value U+3A3 will often be used to represent the entire alphabet of a language. If you have an alphabet that contains only an '1' or a '2' we say that Σ=\{1,2\}. There is a special type of language that can allows any possible occurrence of its alphabet, Σ* is used to represent that type of language.
