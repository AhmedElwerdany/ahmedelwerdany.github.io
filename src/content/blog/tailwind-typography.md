---
title: Tailwind Typography Plugin
author: Sat Naing
pubDatetime: 2022-07-05T02:05:51Z
featured: true
draft: true 
tags:
  - TypeScript
  - Astro
description: "EXAMPLE POST: About Tailwind Typography Plugin and how you can use it effectively."
---

Did you ever come across a feature when you have to use `try-catch` statment for all the methods in your class? No? Bro!! C'Mon.  

Alright, I did and here's my code .

## The problem

```javascript
  class Repository {
    constructor (name) {...}

    async getContent (repo, owner, path) {
      try {
        const response = await fetch('/repos', { repo, owner, path })
        const { data } = response
        const { content } = data

        return content
      } catch(err) {
        if (err.status === 403) {
          await delay(10) // wait 10 seconds before you try again.
          return await this.getContent(repo, owner, path) // run the function again
        }
      }
    }

    async getIssueCount (repo) {...}
    async getPrCount (repo) {...}
    async getLastActive (user) {...}
    ...
  }
```
Let me explain what I'm trying to do here;
This is a Class _(you know that of course)_ that contains some methods to help me get important data of a spcific  **GitHub Repository** using [GitHub Api](https://docs.github.com/en/rest)

And there's an important thing that they share in common; **Rate Limit**.

### Retry

One way to solve the rate limit issue _(and the bad way too🤫 )_ is to keep retrying.
So, I did.

```js
catch(err) {
	if (err.status === 403) {
	
	  // wait 10 seconds before you try again.
	  await delay(10)
	  // run the function again
	  return await this.getContent(repo, owner, path)
	  
    }
}
```
If the status is _403_ then you should try again. _(keep in mind that this in not a good way to check the error, but **you should** check the message too)_; if the error is not a rate limit, I could retry for ever. 

Ok, Snap back to reality, ope there goes gravity  Ope, the...  ok i'll stop 😔 

So, should you _copy-paste_ the the code all over the others methods? did you ever consider how whould they feel about it ? 

The answer is *No*; becuse there's a better way to handle it.

and here it is.

### Proxy

```javascript
// target : your shiny class
function catchUs (target) {
	return new Proxy(target, classHandler)
}
```
That EASY ? - of course No 🙄  
Let me explain what `Proxy` is, then we will write the  `classHandler` togother.

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy): 
> The `Proxy` object allows you to create an object that can be used in place of the original object, but which may redefine fundamental `Object` operations like getting, setting, and defining properties.

**Q**: What dose this can help you for ?   
**A**: You can tell your beatiful ✨ object when someone ask you for a property.   
![x](/assets/posts/one-catch-to-rule-them-all/proxy.png)
_poor proxy 😢_
- check the date first, is it 9AM in the morning ? I am in a good mood give him what he want. is it  1PM afternoon.. hmmm it's hot i am not in good mood for this !
- check the localization.<br />is it `ar` ? then return the number in arabic format e.g. (١, ٢, ٣).<br> is it `en` ? then return it in the english format e.g. (123)

And you can do All the funny & intersting stuff when someone tries to set a property too. 
But How? 
> You create a  `Proxy`  with two parameters:  
	•  `target`: the original object which you want to proxy  
	•  `handler`: an object that defines which operations will be intercepted and how to redefine intercepted operations.

```javascript
const classHandler = {
	construct (Target, args) {
		return new Target()
	}
}
```

