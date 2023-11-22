---
author: Sat Naing
pubDatetime: 2023-01-30T15:57:52.737Z
title: AstroPaper 2.0
postSlug: astro-paper-2
draft: true
featured: false
ogImage: https://user-images.githubusercontent.com/53733092/215771435-25408246-2309-4f8b-a781-1f3d93bdf0ec.png
tags:
  - release
description: AstroPaper with the enhancements of Astro v2. Type-safe markdown contents, bug fixes and better dev experience etc.
---

Sorry, my sense of humor is Dying 😔, my skills aren’t tho.  
So, Let's start by giving you a quick headache 😌.

```js
export default {
    name: 'Form',
    data: () => ({...}),
    watch: {
        shoes_price : function (value) {
            if (typeof value !== 'number') {
                return
            }
            this.total = this.shoes_price + this.short_price + this.hat_price
        },
        short_price : function (value) {
            if (typeof value !== 'number') {
                return
            }
            this.total = this.shoes_price + this.short_price + this.hat_price
        },
        hat_price : function (value) {
            if (typeof value !== 'number') {
                return
            }
            this.total = this.shoes_price + this.short_price + this.hat_price
        },
        total : function () {
            this.total_with_disscount = this.total - (this.total * this.discount)
        }
    }
}
```
{: file='src/components/Checkout.vue'}

I Know what're you thiking about.  
**first**: this didn't give you a headache. which is great  
**second**: I could've done this logic with events like `@change` or whatever.  
**third**: `<input type='number'/>` whould solve the problem of checking the type of the givin value.  
but for the sake of demonstrating the problem and how to solve it. let's just stick with this code.

<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Question",
    "name": "How to watch Array of values with Vue.js?",
    "text": "How to watch multiple values with Vue.js v2 ?",
    "author": {
        "@type": "Person",
        "name": "Ahmed Elwerdany"
    },
    "answerCount": "1",
    "acceptedAnswer": {
        "@type": "Answer",
        "text": "```js
function watchMany (arrayOfKeys, value) {
    const object = {}
    for (let i = 0; i < arrayOfKeys.length; i++) {
        object[arrayOfKeys[i]] = value
    }

    return object
}
```",
        "author": {
            "@type": "Person",
            "name": "Ahmed Elwerdany"
        }
    },
    "suggestedAnswer": {
        "@type": "Answer",
        "text": "```js
function watchMany (arrayOfKeys, value) {
    const object = {}
    for (let i = 0; i < arrayOfKeys.length; i++) {
        object[arrayOfKeys[i]] = value
    }

    return object
}
```",
        "author": {
            "@type": "Person",
            "name": "Ahmed Elwerdany"
        }
    }
}
</script>

## The Question
How to watch Array of values with Vue.js ?

## The solution
1. make a function that can return an object which its keys are the name of your variables.
```js
function watchMany (arrayOfKeys, value) {
    const object = {}
    for (let i = 0; i < arrayOfKeys.length; i++) {
        object[arrayOfKeys[i]] = value
    }

    return object
}
```
    {: file='src/util/watchMany.js'}
2. Spread the result into the `watch` object.
```js
export default {
    name: 'Form',
    data: () => ({...}),
    watch: {
        ...watchMany(['shose_price', 'hat_price','short_price'], function (value) {
            if (typeof value !== 'number') {
                return
            }
            this.total = this.shoes_price + this.short_price + this.hat_price
        }),
        total : function () {
            this.total_with_disscount = this.total - (this.total * this.discount)
        }
    }
}
```
    {: file='src/util/watchMany.js'}
and that's was my solution 🥳, if you know a better one; feel free to submit it in a new [issue](https://github.com/ahmedelwerdany/ahmedelwerdany.github.io/issues/new) with the title of the post. 
