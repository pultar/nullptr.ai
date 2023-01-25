+++
date = "2020-01-01T18:30:00Z"
lastmod = "2020-01-01T18:30:00Z"
author = "felix"
title = "Overview commands"
subtitle = "This document summarizes the available commands."
feature = "image/page-default.png"
tags = ["alkaloids", "polyketides"]
categories = ["artificial intelligence", "machine learning", "total synthesis"]
maxWidthTitle = "max-w-4xl"
maxWidthFeature = "max-w-4xl"
maxWidthContent = "max-w-4xl"
math = true
draft = true
+++


This article offers a sample of basic Markdown syntax that can be used in Hugo content files, also it shows whether basic HTML elements are decorated with CSS in a Hugo theme.

## Headings

The following HTML `<h1>`—`<h6>` elements represent six levels of section headings. `<h1>` is the highest section level while `<h6>` is the lowest. 

# H1
## H2
### H3
#### H4
##### H5
###### H6

Headings `<h3>`-`<h6>` are all sized the same.

## ChemDraw

{{< figure src="image/chemdraws/saxitoxin.svg" width="100%" imgclass="mb-2 mx-auto leading-none shadow-none" >}}


## LaTeX

{{< math >}}
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
{{< /math >}}


{{< math >}}
\begin{bmatrix}

& 1. & 1. \
& 1. & 1. \ \end{bmatrix} + \begin{bmatrix}
& 2. & 3. \
& 5. & 6. \ \end{bmatrix} = \begin{bmatrix}
& 3. & 4. \
& 6. & 7. \ \end{bmatrix}

{{< /math >}}



## GIST

{{< gist spf13 7896402 >}}

## Colab

{{< gist lzhou1110 2a30a81cb8c175514ed627bc18016774 >}}


## Jupyter
{{< gist flavourflex 409f5a41fff241bafb287cbfdbc7d379 >}}


## Paragraph

Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur?

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Blockquotes

The blockquote element represents content that is quoted from another source, optionally with a citation which must be within a `footer` or `cite` element, and optionally with in-line changes such as annotations and abbreviations.

Blockquote without attribution:

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use *Markdown syntax* within a blockquote.

Blockquote with attribution:

> You can trade hours for dollars or ideas for millions.<br>
> — <cite>Cactus Jack[^1]</cite>

[^1]: [The Shark Tank](https://www.example.com/).

## Tables

Tables aren't part of the core Markdown spec, but Hugo supports them.

   Name | Age
--------|------
    Bob | 27
  Alice | 23

Using Markdown inside table cells:

| Italics   | Bold     | Code   |
| --------  | -------- | ------ |
| *italics* | **bold** | `code` |

## Code Blocks

Hugo's built-in Chroma syntax highlighting with Axiom's custom colors:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Title</title>
</head>
<body>
  <p>Lorem...</p>
</body>
</html>
```

```c++
//
//  main.cc
//

#include <iostream>
#include <vector>
#include <string>
#include <memory>

class Entity {
public:
    Entity() {
        std::cout << "Entity created." << std::endl;
    }
    
    ~Entity() {
        std::cout << "Entity deleted." << std::endl;
    }
};

int main(int argc, const char* argv[]) {
    std::string line = "Hello, World!";
    std::vector<int> vector1 = {1, 2, 3, 4, 5};
    std::cout << line << std::endl;
    for (const int& i : vector1) {
        std::cout << i << "  ";
      }
    std::cout << std::endl;
    std::unique_ptr<Entity> r (new Entity());
    std::cout << r << std::endl;
    std::unique_ptr<int> i (new int(5));
    std::cout << i << std::endl;
    std::cout << *i << std::endl;
    return 0;
}
```

```haskell
-- [[Type signature|Type annotation]] (optional, same for each implementation)
factorial :: (Integral a) => a -> a

-- Using recursion (with the "ifthenelse" expression)
factorial n = if n < 2
              then 1
              else n * factorial (n - 1)

-- Using recursion (with pattern matching)
factorial 0 = 1
factorial n = n * factorial (n - 1)

-- Using recursion (with guards)
factorial n
   | n < 2     = 1
   | otherwise = n * factorial (n - 1)

-- Using a list and the "product" function
factorial n = product [1..n]

-- Using fold (implements "product")
factorial n = foldl (*) 1 [1..n]

-- Point-free style
factorial = foldr (*) 1 . enumFromTo 1
```

## List Types

### Ordered List

1. First item
2. Second item
3. Third item

### Unordered List

* List item
* Another item
* Item with paragraph:

  Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam.

### Nested Lists

* ul > ul
    * First Sub-item
    * Second Sub-item

1. ol > ol
    1. First Sub-item
    1. Second Sub-item

* ul > ol
    1. First Sub-item
    1. Second Sub-item

1. ol > ul
    * First Sub-item
    * Second Sub-item

* ul > ul > ul
    * First Sub-item
    * Second Sub-item
        * Third Sub-Sub-item

1. ol > ol > ol
    1. First Sub-item
    1. Second Sub-item
        1. Third Sub-Sub-item

## Other Elements - abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.
