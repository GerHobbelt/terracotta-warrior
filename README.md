Terracotta Warrior
==================

Named after the sculptures of soldiers buried in the tomb of the (in)famous Chinese emperor Qin Shi Huang. Current estimates put the number of sculptures at over 8,000 soldiers, 130 chariots (each with 4 horses) and 150 cavalry horses. No two sculptures were exactly identical.

`terracotta-warrior` generates fake data. It does the usual bells and whistles that other fake data generators offer (random selections from arrays, random number ranges, yada), with a couple of bonus features:

- **Support for randomized punctuation.** This makes things a lot more&hellip; realistic when you're generating paragraphs of text. See that ellipse there?
- **LaTeX/MathJax equations.** Exactly as advertised. `terracotta-warrior` generates fake terms, coefficients, and LaTeX commands, then strings them up to form a fake equation.
- **Markdown content.** This includes itemized and enumerated lists, quotes, and markup for fake images, as well as combinations thereof.

To top things off, everything was designed to be customizable and swappable. Allergic to Lorem Ipsum? Provide your own word list! Generating data for a different language? Provide your own character list! Your MathJax deployment doesn't use `$$` delimiters? Provide your own!

## Usage
Install as you would any Node package:
```shell
npm install --save terracotta-warrior
```
Feel free to use `--save-dev` instead of `--save` if you are using `terracotta-warrior` to mock data only in development.

To get started, require `terracotta-warrior`, then initialize the factories which you need:
```js
var tw = require('terracotta-warrior');

// Create a common random generator for all our factories
var re = new tw.RandEngine();

var tf = new tw.TextFactory({ engine: re });        // Titles and Paragraphs
var uf = new tw.UserFactory({ engine: re });        // User Profiles
var ef = new tw.EquationFactory({ engine: re });    // LaTeX Equations
var mf = new tw.MarkdownFactory({ engine: re });    // Markdown Content
```

## `RandEngine`
The `RandEngine` namespace provides functions that assist with the creation and selection of random variables. In order to ensure reproducibility of the data generated, it overwrites `Math.random()` internally with a seeded RNG from David Bau's excellent [seedrandom](https://github.com/davidbau/seedrandom) package. He didn't pay me to say this, but do buy him a beer if you can.

### Constructor
The constructor for `RandEngine` has two configuration parameters, shown here with their default values:
```js
var opts = {
    imgProviderUrl: '//lorempixel.com', // Image provider URL
    seed: 4                             // A number or string of your choice
}

var re = new tw.RandEngine(opts);
```

### Methods
The following methods are available:
```js
re.randFullNumber(size);
re.randRange(min, max);
re.randBoolean();
re.randImage(width, height, category);
```

- `randFullNumber(size)` generates a number with `size` digits, and with padding zeros in front. For example, `size=3` could give you 009. If no input is provided, `size` defaults to 3.
- `randRange(min, max)` generates an integer `x` such that min ≤ x ≤ max. For example, `min = 1` and `max = 3` could give you 2. If `min` and `max` are the same number, `x` will always be that number as well. If `max` is smaller than `min`, they will be swapped at runtime to ensure that the function is always monotonically increasing. If no input is provided, `min` defaults to 0 and `max` defaults to 1.
- `randBoolean()` generates either a `true` or `false` boolean value.
- `randImage(width, height, category)` generates a random image of size `width` and `height` using the image provider described by `imgProviderUrl`. If either the width or height is not specified, it defaults to a value of 500px. If the category is not specified, it is not used. 

## `TextFactory`
The `TextFactory` namespace provides functions that assist with the generation of text in various forms. It starts from a single character and builds up to a full paragraph.

### Constructor
The constructor for `TextFactory` has six configuration parameters, shown here with their default values:
```js
var opts = {
    engine: new RandEngine(),           // An instance of RandEngine
    charList: ['a', 'b', ...],          // Letters in the English alphabet 
    wordList: ['lorem', 'ipsum', ...],  // Lorem Ipsum words
    endingPuncList: ['.', '?', '!'],    // End of sentence punctuation
    enhancedPunctuation: false,         // Use additional punctuation
    puncList: [',', ';', ' -', ':']     // Inter-word punctuation
}

var tf = new tw.TextFactory(opts);
```
Enabling enhanced punctuation will cause the generator to output "fancy" typographic elements such as em-dashes and ellipses. This is disabled by default because not all typesetting/display systems render these punctuation correctly.

### Methods
The following methods are available:
```js
tf.randLetter();
tf.randWord();
tf.randTitle(size);
tf.randSentence(size, puncSpacing, widowThreshold);
tf.randParagraph(size);
```

- `randLetter()` returns a letter chosen at random from the character list.
- `randWord()` returns a word chosen at random from the word list.
- `randTitle(size)` generates a title of length `size`. If `size` is not specified, it defaults to a random value between 3 and 10. The first character in the title will be capitalized.
- `randSentence(size, widowThreshold)` generates a sentence of length `size`, with the guarantee that punctuation will not appear `widowThreshold` words before the end of the sentence. If no input is specified, `size` defaults to a random value between 10 and 25, and `widowThreshold` defaults to a value of 5. Punctuation is inserted randomly every 11 to 15 words.
- `randParagraph(size)` generates a paragraph with `size` number of sentences. If `size` is not specified, it defaults to a random value between 4 and 10.

## `UserFactory`
The `UserFactory` namespace provides functions that assist with the generation of data related to a user.

### Constructor
The constructor for `UserFactory` has three configuration parameters, shown here with their default values:
```js
var opts = {
    engine: new RandEngine(),           // An instance of RandEngine
    firstNameList: [...]                // A list of first names
    lastNameList: [...]                 // A list of last names
}

var uf = new tw.UserFactory(opts);
```
The default values for `firstNameList` and `lastNameList` can be found in `UserFactory.js`. They are omitted here because they are too long.

### Methods
The following methods are available:
```js
uf.randUsername();
uf.randAvatarUrl();
```

- `randUsername(size)` generates a username by combining a random entry from `firstNameList`, a random entry from `lastNameList`, and a number of `size` digits, in that order. If `size` is not specified, it defaults to 3.
- `randAvatarUrl(width, height)` generates a URL to an image avatar. This is actually a wrapper for `randImage` from the `RandEngine` namespace with `category = 'abstract'`. If `width` and `height` are not specified, they each default to 500px.

## `EquationFactory`

- ``Equation.randTerm(n)`` generates n random variables. For example ``Equation.randTerm(3)`` could return ``7xyz``. If the argument is not specified, it defaults to producing not more than 3 variables. If variables, by random chance, repeat, they will be combined. For example if the function would have returned ``dxd``, it will instead return ``d^2x``. The variables in the result appear in sorted alphabetical order.
- ``Equation.randExpression(n)`` generates an expression containing n random terms. For example, ``Equation.randExpression(2)`` could give you ``3xy+4k``. If the argument is not specified, it defaults to producing not more than 3 terms.
- ``Equation.randFrac()`` generates a fraction. The numerator and denominator of the fraction are random expressions.
- ``Equation.randCommand()`` generates a random LaTeX command and accompanying expression. For example it could return ``\int_x^y 3abc da``.
- ``Equation.wrapMath(eqn)`` is a helper function that wraps ``eqn`` in LaTeX/MathJax compatible math display environment wrappers. That is to say it appends double dollar signs before and after the equation markup.

## `MarkdownFactory`

- ``Markdown.randImageMarkup()`` generates Markdown markup containing a random image.
- ``Markdown.randItemList(n)`` generates Markdown markup that produces an itemized list of sentences with n items in total. If the argument is not specified, it defaults to a value between 3 and 5.
- ``Markdown.randEnumList(n)`` generates Markdown markup that produces an enumerated list of sentences with n items in total. If the argument is not specified, it defaults to a value between 3 and 5.
- ``Markdown.randQuote()`` generates Markdown markup for a sentence placed within a quote block.
- ``Markdown.randContent(n)`` generates Markdown markup for n elements worth of content. Each element could be an enumerated list, itemized list, equation, paragraph text, image, or quote. If the argument is not specified, it defaults to a value between 3 and 7.

## Bug Tracking and Feature Requests

Have a bug or a feature request? [Please open a new issue](https://github.com/kenlimmj/terracotta-warrior/issues).

Before opening any issue, please search for existing issues and read the [Issue Guidelines](https://github.com/necolas/issue-guidelines), written by [Nicolas Gallagher](https://github.com/necolas/).

## Contributing

Please submit all pull requests against *-wip branches. If you have something that everyone can use, I'll be more than glad to add it with the appropriate attribution and credits. If it's something really specific that only you will need, it's probably better (and easier) if you fork and implement it locally :).

## Author

**Kenneth Lim**
+ http://kenlimmj.com
+ http://twitter.com/kenlimmj
+ http://github.com/kenlimmj

## Copyright and License

The MIT License (MIT)

Copyright (c) 2015 Kenneth Lim

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
