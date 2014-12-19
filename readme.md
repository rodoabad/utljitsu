# The Art of Writing Consistent, Self-documenting UTL

> ### "All code in any code-base should look like a single person typed it, no matter how many people contributed."
>*[idiomatic.js](https://github.com/rwaldron/idiomatic.js/)*

## What is a style guide?

A style guide is a set of standards when writing code that provides uniformity in code style and formatting, i.e. tabs versus spaces or variable and function naming conventions.

## Why should you use one?

> ### "Good code is it's own documentation."
>*[Steve McConnell](http://www.cc2e.com/Default.aspx)*

Having worked on a code-base with several other developers with each having their own style, it is very cumbersome to know what is going on in any given area of the source code. Following a consistent style helps improve the readability of the source code making it easy to understand and maintain. It makes it a lot easier to browse, locate, and fix bugs.

## Table of Contents

- [Whitespace](#whitespace)
- [Naming Convention](#nc-naming-convention)
 - [Global Variables](#nc-global-variables)
 - [Local Variables](#nc-local-variables)
 - [Keys in an Array](#nc-keys-in-an-array)
 - [Custom Properties](#nc-custom-properties)
 - [Macros](#nc-macros)
- [Beautiful Syntax](#nc-beautiful-syntax)

---

## Preface

The following sections outlines the practices that I use in all UTL code that I am have authored. Before you begin to use this style guide there is one thing that you have to remember, once you've chosen this style guide it is to be considered **law**. [Link](https://github.com/rodoabad/utljitsu) to this document to remind yourself and everyone else involved about your project's commitment to code style consistency, readability, and maintainability.

## UTL Manifesto

1. <a name="whitespace">Whitespace</a>

 - Never mix spaces and tabs.
 - When beginning a project, before you write any code or do anything, set your IDE to use soft indents (spaces) and set the size to 4.
 - Make sure that your IDE is showing invisible spaces. If it doesn't go get another one that supports it. The benefits of this practice are:
    - Enforced consistency
    - Eliminate end of line whitespace
    - Eliminate blank line whitespace
    - Commits and diffs are easier to read

2. <a name="naming-convention">Naming Convention</a>

 You are not a human code minifier/uglifier, do not try to be one. Leave it to the machines, there's plenty of whitespace for everyone.

 **Bad**
 ```
 bX = util_chkX('ast': this.asset, 'b': true);
 ```
 **Good**
 ```
 bSubscription = util_checkSubscription('oAsset' = this.asset, 'bToggle': true);
 ```

 A. <a name="nc-global-variables">Global Variables</a>

 If you do require a global variable, it should be in ALL CAPS. In general, if a module is available from the CMS that does the same thing, just use that. If the information you are attempting to store requires computation, a global variable can be used, following these examples:

 - `TEMPLATE_VERSION = '1.0';`
 - `IS_TEMPLATE = cms.request.param('template') | defaultval('no') | strbool;`

 B. <a name="nc-local-variables">Local Variables</a>

 Local variables should be camel cased with [Hungarian Notation](http://en.wikipedia.org/wiki/Hungarian_notation). Here are a couple of examples:

 - `bCheck`: variable is a *boolean* ('b')
 - `sUrl`: variable is a *string* ('s')
 - `aList`: variable is an *array* ('a'), also take note that the plural form is used instead of the singular form
 - `oPages`: variable is an *array of objects* ('o'), also take note that the plural form of the object is used instead of the singular form

 C. <a name="nc-keys-in-an-array">Keys in an Array</a>

 Array keys should be lowercase with underscores. Here's an example:

 ```
 aData = [
    'key_1': 'This is a string',
    'key_2': bCheck
 ];
 ```

 D. <a name="nc-custom-properties">Custom Properties</a>

 Custom properties should be lowercase with underscores. Here are a couple of examples:

  - `product_oauth_key`
  - `site_verification`

 E. <a name="nc-macros">Macros</a>

 Macros are namespaced according to the application/component that they belong to. For example, a macro that is located in `utilities` component would be namespaced as `util_getSomething()`.

3. <a name="beautiful-syntax">Beautiful Syntax</a>

 A. <a name="bs-open-close-utl">Opening and Closing UTL Statements</a>

 Whenever possible, minimize unnecessary whitespace and premature opening/closing of UTL.

 **Bad**
 ```
[%
    util_doSomething(this.asset);
%]
 ```
 **Bad**
 ```
[%
    util_doSomething(this.asset); %]
```
 **Bad**
 ```
[% util_doSomething(this.asset);
%]
 ```
 **Good**
 ```
[% util_doSomething(this.asset); %]
 ```
 **Bad**
 ```
[%
    /* Init variables */
    aData = [];
    bSubscription = util_checkSubscription(this.asset);

    util_doSomething(this.asset);
%]
 ```
 **Bad**
 ```
[% /* Init variables */
aData = [];
bSubscription = util_checkSubscription(this.asset);

util_doSomething(this.asset);
%]
 ```
 **Bad**
 ```
[%
    /* Init variables */
    aData = [];
    bSubscription = util_checkSubscription(this.asset);

    util_doSomething(this.asset); %]
 ```
 **Good**
 ```
[%
/* Init variables */
aData = [];
bSubscription = util_checkSubscription(this.asset);

util_doSomething(this.asset); %]
 ```
 B. <a name="bs-comments">Comments</a>

 Comments are extremely important as they usually describe the intended function of the code snippet, how it should be used, their limitation, and what they accept and return. Do not leave others guessing as to what the purpose of the non-obvious code is.
 - Place comments on a new line above the code snippet.
 - Multiline is good an is encourage.
 - Keep line-length to a sensible maximum, e.g., 80 columns.
 - End of line comments are prohibited.
 - Use "sentence case" comments and consistent identation.
 - JSDoc style is good, read next item.
 - Macro declarations must begin with a comment and must contain the following:
    - `@desc` - Describes what the intended function of the macro is
    - `@params` - Each parameter must have its own comment
    - `@returns` - What the macro returns e.g., an HTML, an array, etc

 **Bad**
 ```
 /**
  * @desc This is a macro that returns the title of an asset.
  * @params {Object} oArticle - Takes an article asset.
  * @params {Boolean} bfullWidth - Toggles the output depending on setting.
  * @returns {HTML} Returns the presentation mode of the article asset in HTML format.
  */
 macro asset_displayPresentation(oArticle = [], bfullWidth = false);

 end;
 ```

 **Good**
 ```
/**
 * @desc This is a macro that returns the title of an asset.
 *
 * @params {Object} oArticle - Takes an article asset.
 * @params {Boolean} bfullWidth - Toggles the output depending on setting.
 *
 * @returns {HTML} Returns the presentation mode of the article asset in HTML format.
 */
macro asset_displayPresentation(oArticle = [], bfullWidth = false);

end;
 ```

 **Bad**
 ```
oImages = asset_relatedImages(this.asset); /* Store all related images to oImages. */
 ```

 **Good**
 ```
/* Store all related images to oImages. */
oImages = asset_relatedImages(this.asset);
 ```

 **Good**

 ```
 /**
  * Loop through each video in oVideos and create a summary card
  * and add a related videos card as well.
  *
  * TODO: Improve card displays when video asset only has one related video.
  */
 foreach oVideos as oVideo;
    asset_videoSummaryCard(oVideo);
    asset_relatedVideos(oVideo);
 end;
 ```

 C. <a name="bs-macro-declarations-invocations">Macro Declarations and Invocations</a>

 You've already learned how to namespace UTL macros. Now it's time to learn how to declare and invoke them. Remember, each macro declaration must have a comment above it.

 **Bad**
 ```
macro asset_filterImages;

end;
 ```

 **Good**
 ```
/**
 * @desc Insert horizontal div.
 *
 * @params {None}
 *
 * @returns {HTML} Return an HTML element with a horizontal line.
 */
macro asset_horizontalDiv();

end;
 ```