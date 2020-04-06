---
layout: post
title: Thoughts on Go
date: 2020-03-25 14:00:00
summary: It's... mostly fine.
categories: programming go golang
---

### What I Like
I actually like Go just fine, despite the bulk of this post being about my gripes with the language. Go was designed by Google to build scalable web services that would be worked on by large teams with wide ranges of experience and skill, so it's no surprise that the things I like about Go are the things its designers set out to do well: concurrency and optimizing for reading code over writing it.

I'm a big fan of [CSP](https://www.cs.cmu.edu/~crary/819-f09/Hoare78.pdf), so Go's channels and goroutines feel very natural to me. The general approach is summed up in the Go community as "don't communicate by sharing memory; instead, share memory by communicating," which I think is capital-C Correct and is well explained in [this Go Blog post](https://blog.golang.org/codelab-share) and [this related Codewalk](https://golang.org/doc/codewalk/sharemem/).

While I don't particularly like Go's style conventions, I _really_ like that those conventions are enforced by `go fmt`. There are some minor annoyances (such as unused imports and variables being errors during development), but generally, I've found Go's opinions to be reasonable and informed by hard-won experience. I don't hate the `err != nil` idiom as much as I expected: I prefer errors as values, and I'm willing to trade the boilerplate typing for the requirement that I think through and explicitly handle each error.

The rest of this post is about the things I dislike about Go (which are, unsurprisingly, features that exist in other languages but were intentionally omitted by the Go designers<sup>1</sup>). These are, in rough order of decreasing dislike: the lack of parametric polymorphism (generics), the lack of algebraic data types, and the lack of functional constructs like `map`/`filter`/`reduce`. Part of this is my superhuman ability to attract off-by-one and nil pointer errors; the rest is probably that I'm used to having these constructs (I really like writing Clojure, Elixir, Haskell, Ruby, and TypeScript). If any of this resonates with you, read on!

### Parametric Polymorphism
Parametric polymorphism really just means "generics" (I'll use the two interchangeably); if you're familiar with Java or C<sup>#</sup>, you've likely come across this idea. If you haven't encountered generics before, the idea is to parameterize your type system so that instead of specifying that a certain function works on, say, "list of integer" or "list of string," you can write that function to work on "list of A," where `A` is whatever type you want; you get a common interface for multiple types (polymorphism) that takes the concrete type as a parameter (parametric).

Go doesn't have parametric polymorphism, so if you want to write a function that operates on slices of `n` arbitrary types, you have to write your function `n` times. [For example](https://repl.it/repls/RichCheeryWireframe):

```go
type IntList []int

func (list *IntList) Contains(desired int) bool {
	for _, element := range *list {
		if element == desired {
			return true
		}
	}

	return false
}

type StringList []string

func (list *StringList) Contains(desired string) bool {
	for _, element := range *list {
		if element == desired {
			return true
		}
	}

	return false
}

// &c, &c
```

In a language that _does_ have parametric polymorphism, such as Haskell, you can write `contains` [like so](https://repl.it/repls/SameSatisfiedListeners):

```haskell
contains :: Eq a => [a] -> a -> Bool
contains [] _ = False
contains (y:ys) x
  | y == x    = True
  | otherwise = contains (ys) x
```

The type parameter is `a` in this example, and this version of `contains` will work on any type where instances can be equal or unequal to each other: integers, characters, booleans, and so on. This syntax might look odd if you're unfamiliar with Haskell, so let's walk through it. The first line is the type signature, which says:

1. This function is called `contains`;
2. It has a certain type (`::` is read "has type");
3. That type includes a variable, `a`, which implements the typeclass `Eq` (this means "whatever `a` is, instances of `a` are equal or unequal to one another");
4. Given `[a]` (a list of `a`) and some candidate `a`, the function returns a `Bool`ean.<sup>2</sup>

The next few lines pattern match on the parameters. `contains [] _ = False` means "If you get the empty list, return false"; the `_` character means "I don't care what `a` you provide, since by definition, the empty list can't contain anything."

The final lines destructure the list into a head (`y`) and a tail (`ys`); if the head of the list equals what we're looking for (`x`), we return `True`, otherwise, we recurse on the rest of the list. (The base case is the empty list: if we get to an empty list, the list must not contain `x`, so we return `False`.)

If you're familiar with Java, a generic `contains` method looks [like this](https://repl.it/repls/ShabbyWickedTheory):

```java
public static <T> boolean contains(List<T> list, T desired) {
    for (T element : list) {
        if (element.equals(desired)) {
            return true;
        }
    }

    return false;
}
```

This strikes me as verbose compared to the Haskell version (the Haskell types are inferred, and while I included the type signature for clarity, it's optional). In many ways, the Go designers were [trying to build a better Java](https://golang.org/doc/faq#What_is_the_purpose_of_the_project), so in this sense, Go is unquestionably an improvement; however, from the point of view of [languages with more powerful type systems](https://softwareengineering.stackexchange.com/questions/279316/what-exactly-makes-the-haskell-type-system-so-revered-vs-say-java) (like Haskell), it's a bit lacking.

I'll touch on TypeScript a few times in this post, so here's what `contains` looks like [in TypeScript](https://repl.it/repls/CluelessStrikingTransversals):

```javascript
function contains<T>(list: T[], desired: T): boolean {
  for (let element of list) {
    if (element === desired) {
      return true;
    }
  }

  return false;
}
```

If you try to call `contains(contains([1, 2, 3], "quux")`, the TypeScript compiler will yell at you: `Argument of type '"quux"' is not assignable to parameter of type 'number'`.

To round out our examples: if you're coming from a language with duck typing, like Ruby, `contains` looks something [like this](https://repl.it/repls/DefiantMoccasinAdministrators):

```ruby
def contains?(collection, desired)
  collection.each do |element|
    return true if element == desired
  end

  false
end
```

We don't explicitly parameterize the type here because we don't need to: the idea of duck typing is that if it walks like a duck and quacks like a duck, it must be a duck; similarly, if `element` and `desired` both respond to the `==` message<sup>3</sup>, then that's good enough.

Okay! Back to Go. Some developers argue that the workaround for the language's lack of generics is to use the empty interface: `interface{}`. The implementation for `contains` could then be written [like this](https://repl.it/repls/AssuredThriftyTrees):

```go
package main

import (
	"fmt"
)

type Collection []interface{}
type Element interface{}

func (list *Collection) Contains(desired Element) bool {
	for _, element := range *list {
		if element == desired {
			return true
		}
	}

	return false
}

func main() {
	list := Collection{1, 2, 3}
	desired := "quux"
	fmt.Printf("%v\n", list.Contains(desired))
}
```

By doing this, however, we're effectively _opting out of type checking_. The equivalent TypeScript makes this more explicit:

```javascript
// Note the replacement of the type paramater `T` with
// `any`. You can make a list of any random combination
// of types and look for an element of literally any type!
// There are no rules! Human sacrifice, dogs and cats
// living together... mass hysteria!
function contains(list: any[], desired: any): boolean {
  for (let element of list) {
    if (element === desired) {
      return true;
    }
  }

  return false;
}
```

The TypeScript compiler no longer complains here (simply returning `false`), and the same is true of the Go compiler: it can no longer help us if we choose to use `interface{}`. Indeed, `list := Collection{1, 2, 3}; list.Contains("quux")` compiles, and at first blush seems to work:

```sh
$ go run List.go
false
```

...but what if we want to [add a function](https://repl.it/repls/FondHollowWorkplace) that appends a new `Element` to our `Collection`?

```go
// ...
func (list *Collection) Append(item Element) Collection {
	return append(*list, item)
}

func main() {
	// ...
	fmt.Printf("%v\n", list.Append(desired))
}
```

Our program cheerfully prints `[1 2 3 quux]`. While we've gained the ability to write single, generic functions for slices, we're no longer able to to guarantee that slices only hold a single type (as opposed to our Haskell `contains`, which would fail at compile time if we were to try to append a `"quux"` to a `[Integer]`).

There's been [some discussion around adding generics to Go](https://blog.golang.org/why-generics), but as far as I know, there's no plan to include them in upcoming releases of the language. For the time being, we're stuck either writing code generators to handle the "`n` implementation" problem or opting out of compile-time checks entirely via `interface{}`.

### Algebraic Data Types
Languages like Haskell and TypeScript have [algebraic data types](https://en.wikipedia.org/wiki/Algebraic_data_type). If you're not familiar with the idea, ADTs are a kind of composite data type that come with certain rules for their manipulation (hence "algebraic"). They're most commonly found in two kinds: product types and sum types.

Most languages have product types, and depending on your background, you might know these as records, named tuples, or structs. (They're called product types because the set of possible values for a given product type is the Cartesian product of the sets of all possible values of its composite field types.) In Go, they take the form of structs, and they look like this:

```go
type User struct {
	Id        uint64
	FirstName string
	LastName  string
	Email     string
}
```

The other algebraic data type, the sum type (also known as a discriminated union, tagged union, variant, or coproduct), doesn't exist in Go. The closest analogue would be enums using [iota](https://blog.learngoprogramming.com/golang-const-type-enums-iota-bc4befd096d3), which is _unbelievably_ clumsy compared to true sum types. Either way, the goal is the same: to specify a type that can only take on a fixed set of values. (They're called sum types because the set of all possible values is the sum of all possible values.)

For example, not too long ago I was working through the "Sublist" problem on [Exercism](https://exercism.io/), which involves determining whether one list is a sublist of another. The Go version expects the programmer to define a `Relation`, but just as an alias for `string`, with the implicit expectation that said string is one of `"equal"`, `"sublist"`, `"superlist"`, or `"unequal"`. The type system can't help you here if you somehow pass in another string: any valid `string` is a valid `Relation`. In Haskell, we might define `Relation` as a sum type like this:

```haskell
data Relation = Equal | Sublist | Superlist | Unequal
```

Here, the Haskell compiler enforces the possible values of `Relation`: it can only be `Equal`, `Sublist`, `Superlist`, or `Unequal`. They're not strings whose values we try to constrain with comments, test cases, or other hints to the programmer: any other value than the four delineated will cause a failure at compile time.

### The Functional Approach
As I mentioned earlier in this post, I'm a magnet for off-by-one errors and null pointer exceptions. While the `range` operator does obviate indexing errors, Go still has classic `for` loops, in which cases you have to be careful not to accidentally introduce a negative index or an index greater than `len(list) - 1`. Functions like `filter`, `map`, and `reduce` allow you to pipeline list operations (or operations on arbitrary collections&mdash;at least in languages with generics) without the risk of going out of bounds. This is especially nice when munging text; for example, here's [a function that canonicalizes an article title into a URL path in Go](https://repl.it/repls/TepidShorttermMaintenance):

```go
// Converts an article title (e.g. "A Fine Mahok to
// You All: The Robert U. Terwilliger, Jr. Story" to
// a URL path (e.g. "a-fine-mahok-to-you-all-the-
// robert-u-terwilliger-jr-story").
func ToURLPath(title string) string {
	title = strings.ToLower(title)
	var path strings.Builder

	for _, character := range title {
		if unicode.IsLetter(character) ||
			unicode.IsDigit(character) {
			path.WriteRune(character)
		}

		if unicode.IsSpace(character) {
			path.WriteRune('-')
		}
	}

	return path.String()
}
```

...and the same function [in Clojure](https://repl.it/repls/ImperfectDarkgreyComputationalscience):

```clj
(defn- url-printable?
  [c]
  (or (Character/isLetterOrDigit c)
      (Character/isSpace c)))

(defn to-url-path
  [title]
  (->> title
       (lower-case)
       (filter url-printable?)
       (replace {\space \-})
       (join "")))
```

Here I don't think there's much cost savings in either lines of text/cognitive load or time to compose (each implementation took me a couple of minutes), so it comes down to ease/familiarity and style. If you're not familiar with Clojure, the syntax (especially the threading macro, `->>`) can be disorienting; if you don't know Go or Java, the string builder pattern might be foreign to you.

### Conclusion
Overall, Go is fine. While I wouldn't necessarily pick it for small personal projects, I can see its merits as a production language for large teams building web services, and I think it's a language worth knowing.

&nbsp;

---
&nbsp;

1. When I'm feeling charitable, I think of this as removing footguns (obvious and non-); when I'm feeling less generous, I've got to admit it kinda feels like catering to the lowest common denominator.
2. You might wonder why the function takes two arguments and returns a result, but is written as `[a] -> a -> bool` (rather than, say, `([a] -> a) -> bool`). The reason is that all functions in Haskell are automatically [curried](https://wiki.haskell.org/Currying), so a function that appears to take two arguments is actually a function that takes a single argument, _which returns a function that takes a single argument and returns the end value_. The major theoretical advantage is that it makes formal proofs easier; the major practical advantage is it aids composition via partial application.
3. Ruby is heavily influenced by [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk#Messages).
