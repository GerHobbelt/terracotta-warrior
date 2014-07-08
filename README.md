Terracotta Warrior
==================

Named after the sculptures of soldiers buried with the infamous chinese emperor Qin Shi Huang. Current estimates put the number at over 8,000 soldiers, 130 chariots (each with 4 horses) and 150 cavalry horses. No two sculptures were exactly identical.

More realistically, ```terracotta-warrior``` generates fake data. It does the usual bells and whistles that other fake data generators offer, with a couple of bonus features:

- **Support for randomized punctuation.** This makes things a lot more&hellip; realistic when you're generating paragraphs of text.
- **LaTeX/MathJax equations.** Exactly as advertised. terracotta-warrior generates fake terms, coefficients, and LaTeX commands, then strings them up to form a fake equation.
- **Markdown content.** This includes itemized and enumerated lists, quotes, and markup for fake images.

## Usage
Load all the files from the JS directory. For example:
```html
<script src="js/utils.js"></script>
<script src="js/type.js"></script>
<script src="js/user.js"></script>
<script src="js/equation.js"></script>
<script src="js/markdown.js"></script>
```
There are currently no build/test scripts included, but feel free to minify everything as you see fit.

### Utils

- ``RandUtils.randFullNumber(n)`` generates a number of n digits with padding zeros in front. For example, ``n=3`` could give you 009.
- ``RandUtils.randRange(min,max)`` generates an integer x such that min < x < max. For example, ``min = 1`` and ``max = 3`` could give you 2.
- ``RandUtils.randFromArray(arr)`` selects an element from the array ``arr``. For example, ``arr = ['a','b','c']`` could give you ``'a'``.
- ``RandUtils.randBoolean()`` generates either a true or false boolean value.
- ``RandUtils.randImage(width,height)`` generates a random image of size ``width`` and ``height``. If any parameter is not specified, it defaults to a value of 500px. Images are generated using [LoremPixel](http://lorempixel.com).

### Type

- ``Type.randLetter()`` generates a random letter from the alphabet.
- ``Type.randWord()`` generates a random word from the lorem ipsum dictionary. For example ``Type.randWord()`` could give you "voluptates".
- ``Type.randTitle(size)`` generates a random title of word length ``size``. If the argument is not specified, it defaults to a random number ranging from 3 to 10. The result is returned in title case.
- ``Type.randSentence(size)`` generates a random sentence of word length ``size``. If the argument is not specified, it defaults to a random number ranging from 10 to 25. The result is returned in sentence case, and punctuation is randomly inserted at intervals of 11 to 15 words so long there are at least five words to the end of the sentence.
- ``Type.randParagraph(size)`` generates a random paragraph of sentence length ``size``. If the argument is not specified, it defaults to a random number ranging from 4 to 10.

### Equation

- ``Equation.randTerm(n)`` generates n random variables. For example ``Equation.randTerm(3)`` could return ``7xyz``. If the argument is not specified, it defaults to producing not more than 3 variables. If variables, by random chance, repeat, they will be combined. For example if the function would have returned ``dxd``, it will instead return ``d^2x``. The variables in the result appear in sorted alphabetical order.
- ``Equation.randExpression(n)`` generates an expression containing n random terms. For example, ``Equation.randExpression(2)`` could give you ``3xy+4k``. If the argument is not specified, it defaults to producing not more than 3 terms.
- ``Equation.randFrac()`` generates a fraction. The numerator and denominator of the fraction are random expressions.
- ``Equation.randCommand()`` generates a random LaTeX command and accompanying expression. For example it could return ``\int_x^y 3abc da``.
- ``Equation.wrapMath(eqn)`` is a helper function that wraps ``eqn`` in LaTeX/MathJax compatible math display environment wrappers. That is to say it appends double dollar signs before and after the equation markup.

### Markdown

- ``Markdown.randImageMarkup()`` generates Markdown markup containing a random image.
- ``Markdown.randItemList(n)`` generates Markdown markup that produces an itemized list of sentences with n items in total. If the argument is not specified, it defaults to a value between 3 and 5.
- ``Markdown.randEnumList(n)`` generates Markdown markup that produces an enumerated list of sentences with n items in total. If the argument is not specified, it defaults to a value between 3 and 5.
- ``Markdown.randQuote()`` generates Markdown markup for a sentence placed within a quote block.
- ``Markdown.randContent(n)`` generates Markdown markup for n elements worth of content. Each element could be an enumerated list, itemized list, equation, paragraph text, image, or quote. If the argument is not specified, it defaults to a value between 3 and 7.

### Using Seeds

Seeds in JS are a little problematic because supporting them inevitably means writing a Mersenne Twister or something of that nature. Since not everyone needs them, I figured I'll leave them out by default. However if you do require that feature, someone's got us covered: [Seedrandom.js](https://github.com/davidbau/seedrandom)

Drop that in and specify a seed like so:
```javascript
Math.seedrandom('someSeed');
```
Everything just works. Someone give this guy a medal. Seriously.

## Bug Tracking and Feature Requests

Have a bug or a feature request? [Please open a new issue](https://github.com/kenlimmj/terracotta-warrior/issues).

Before opening any issue, please search for existing issues and read the [Issue Guidelines](https://github.com/necolas/issue-guidelines), written by [Nicolas Gallagher](https://github.com/necolas/).

## Contributing

Please submit all pull requests against *-wip branches. If you have something that everyone can use, I'll be more than glad to add it. If it's something really specific that only you will need, it's probably better (and easier) if you fork and implement it locally :)

## Author

**Kenneth Lim**
+ http://kenlimmj.com
+ http://twitter.com/kenlimmj
+ http://github.com/kenlimmj

## Copyright and License

The MIT License (MIT)

Copyright (c) 2014 Kenneth Lim

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
