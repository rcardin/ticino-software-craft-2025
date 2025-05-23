---
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('./assets/kame-house.jpg')
author: Riccardo Cardin
lang: en
color: #111111
footer: "Riccardo Cardin - Ticino Software Crat - 2025-05-28"

marp: true
---

<style>
  header,footer {
    color:rgb(34, 34, 35);
  }
  h1 {
    color:rgb(47, 95, 4);
  }
</style>


<!-- _paginate: false -->
<!-- _footer: "" -->

# Do You Even Handle Effects?
### Direct Style Effect System in Scala

---

<!-- _class: lead -->

![](./assets/0-days.jpg)

---

# Agenda

___

# Who Am I?

* Hello there 👋, I'm **Riccardo Cardin**, 
  * An Enthusiastic Scala Lover since 2011 💯

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![w:300 h:300](./assets/github-qr.jpeg)&nbsp;&nbsp;&nbsp;&nbsp;![w:300 h:300](./assets/linkedin-qr.jpeg)&nbsp;&nbsp;&nbsp; ![w:300 h:300](./assets/blog-qr.jpeg)

---

<!-- _class: lead -->
# Effects and Side Effects

---

# Why We ❤️ Functional Programming

* We have the **substitution model** for reasoning about programs
```scala 3
def plusOne(i: Int): Int = i + 1
def timesTwo(i: Int): Int = plusOne(plusOne(i))
```
* The substitution model enables **local reasoning** and **referential transparency**
  * Original program and the substituted program are *equivalent*
* We call these functions *pure* functions

---

# We Live in an Imperfect World 💔

> Model a coin toss, but with a twist: the gambler might be too drunk and lose the coin

```scala 3
import scala.util.Random

def drunkFlip(): String = {
  val caught = Random.nextBoolean()
  val heads =
    if (caught) Random.nextBoolean()
    else throw new Exception("We dropped the coin")
  if (heads) "Heads" else "Tails"
}
```

---

# We Live in an Imperfect World 💔

* We can't use the substitution model for all programs
  * If the `drunkFlip` function throws an _exception_, the substitution model breaks

* Programs that interact with a context outside the function
  * The result of the `drunkFlip` function depends on the state of the world

* Multiple calls to `drunkFlip` can return different results

---

# Side Effects

* **Side Effect**: An _unpredictable change_ in the state of the world
  * *Unmanaged*, they just happen

```scala 3
// What happens if b is equal to zero?
def divide(a: Int, b: Int): Int = a / b
```

* We call `divide` an *impure* function
* The best we can do is to **track** and push them to the _boundaries_ of our system

---

# The Effect Pattern

When a side effect is _tracked_ and _controlled_ we call it an **effect**
  1. The _type_ of the function should tell us what effects it can perform and what's the type of the result
    - The `drunkFlip` deals with _non-determinism_ and _errors_
  2. We must separate the _description_ from making it happen
    - We want a _recipe_ of the program.
    - **Deferred execution**

The pattern lets us use the **substitution model** again 🚀 🎉

---

<style scoped>
    p {
      font-size: 20pt;
    }
</style>

# The Effect Pattern

![width:1130](/assets/effect-pattern.png)
© Adam Rosien, Essential Effects

---