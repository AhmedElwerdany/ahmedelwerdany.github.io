---
title: 👀 Watch many in VueJs !
author: Ahmed
date: 2022-10-24 11:33:00 +0800
categories: [Blogging, Demo]
tags: [javascript,tips,vuejs]
math: true
mermaid: true
# image:
#   path: /commons/devices-mockup.png
#   width: 800
#   height: 500
#   alt: Responsive rendering of Chirpy theme on multiple devices.
---

ok, Let me start by giving you a quick headache.

```js
export default {
    name: 'Form',
    data: () => ({...}),
    watch: {
        'shoes_price' : function () {
            this.total = this.shoes_price + this.short_price + this.hat_price
        },
        'short_price' : function () {
            this.total = this.shoes_price + this.short_price + this.hat_price
        },
        'hat_price' : function () {
            this.total = this.shoes_price + this.short_price + this.hat_price
        },
    }
}
```

you still holding, tough person aren't you?  ok let me try again ?