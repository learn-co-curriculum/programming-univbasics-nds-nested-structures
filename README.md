# Nested Structures

## Learning Goals

- Identify that nested structures can be mixed

## Introduction

You've spent the last few lessons learning about four basic nested structures:

* `Array` of `Array`s, or "AoA"
* `Array` of `Hash`es, or "AoH"
* `Hash` of `Hash`es, or "HoH"
* `Hash` of `Array`s, or "HoA"

By using these basic nested data structures, we've gained a foundation for
modeling many things in the world: vending machines, fictional characters,
the genealogy of a Hollywood family, and a medical record result-set.

But brace yourself, here comes something astounding:

***We can nest nested data structures in other nested data structures***

Your `Array` of `Hashes` (AoH) can have keys that point to `Array`s of `Array`s
(AoA). Or your nested node structure of `Hash`es of `Hash`es (HoH) can have a
key that points to an `Array` of `Hash`es (AoH). By using this mix-and-match
principle, our NDS' can become very complex!

In this lesson, we're going to walk you through an improved vending
machine model. Get a pen and some paper so that you can draw
pictures to help drive these points home. Using sketches will really
help you "get it." This is why developers in professional settings are
always sketching their NDS' whiteboards. It really works!

## Describe a Physical Vending Machine

![Grid-based vending machine](https://curriculum-content.s3.amazonaws.com/programming-univbasics-5/nested-arrays-lab/vending_grid.png)

The other week we saw a vending machine in a hospital waiting room. After
swiping your credit card or using your phone's wallet, you entered a **grid
coordinate**.  At each intersection (coordinate) in the vending machine, there
was a "spinner." Pay attention to this noun, "spinner." We're going to use it a
lot in the coming lessons. It's a device that looks like a corkscrew. It
**holds multiple snack packages** in it.  When you've paid, the spinner spins
and pushes the front-most snack off of the corkscrew "spinner." The snack then
falls to a retrieval box.

On each snack's packaging a price and a name.

We've preferred to stick with `:price` and `:name`. We've also chosen to keep
the price value simple integers for clarity.

## Identify that Nested Structures Can Be Mixed

Ultimately, in order to model a real grid-based vending machine (`C20` is a candy
bar off of a spinner of gum packages, `B50` is a drink off of a spinner of
drinks), we think the right NDS is:

```text
Array of Array
...of Array
...of Hashes
.....with keys `:name` and `:price` where
.......:name points to a `String`
.......:price points to an `Integer` price
```
> **_Your_ Model May Vary**: It's certainly true that there are many other
> details that we could record in this model. Nutritional value, brand,
> manufacturer, etc.
>
> If you think each coordinate should be a `Hash` with a `:price` and an
> `Array` of snacks, that also might be entirely appropriate. Discussion of how
> to model an NDS is what developers spend a great number of their meetings and
> pair programming sessions discussing!

We're giving you the answer so that you can think about how this might work for
yourself. Draw it out on paper and see if you can predict how our explanation
will work. Teach your friend, your cat, or your imaginary friend Hogarth how ***you***
think things work **and then** read the explanation below. Don't lose this chance
to train your brain!

### Explain the Entire Vending Machine from the Top Down

<pre>
We have a coordinate grid. That's an AoA
In each coordinate, there's a "spinner" with multiple snacks
Each snack has two important facts associated with it, a :name String and a :price
Integer
</pre>

### Explain the Entire Vending Machine from the Bottom Up

Here's a bottom-up view of the NDS:

<pre>
A price of the snack represented as an `Integer`
Each snack has a name stored as a `String`
Each snack collects those facts in a `Hash` that has keys `:name` and `:price`.

Multiple snacks are stored in an indexed collection, an `Array`, that represents
the "spinner" device. Each "spinner" is accessible by a coordinate within an AoA "grid."
The vending machine is the super-container AoA.
</pre>

Whichever approach feels more natural to you, feel free to use it. Sometimes
our brains find a logical "foothold" while working bottom-up. Sometimes our
brains are thinking in a big-picture sense first. Either way is OK!

### Show the Data Structure as Ruby Code

A real version of this data structure is the following. We've included the
structure, as well as a few Ruby commands, to get some sample data out of
the NDS.

```ruby
vending_machine = [[[{:name=>"Vanilla Cookies", :price=>3},
   {:name=>"Pistachio Cookies", :price=>3},
   {:name=>"Chocolate Cookies", :price=>3},
   {:name=>"Chocolate Chip Cookies", :price=>3}],
  [{:name=>"Tooth-Melters", :price=>12},
   {:name=>"Tooth-Destroyers", :price=>12},
   {:name=>"Enamel Eaters", :price=>12},
   {:name=>"Dentist's Nightmare", :price=>20}],
  [{:name=>"Gummy Sour Apple", :price=>3},
   {:name=>"Gummy Apple", :price=>5},
   {:name=>"Gummy Moldy Apple", :price=>1}]],
 [[{:name=>"Grape Drink", :price=>1},
   {:name=>"Orange Drink", :price=>1},
   {:name=>"Pineapple Drink", :price=>1}],
  [{:name=>"Mints", :price=>13},
   {:name=>"Curiously Toxic Mints", :price=>1000},
   {:name=>"US Mints", :price=>99}]]] #=> [[[{:name=>"Vanilla Cookies", :price=>3}, {:name=>"Pistachio Cookies", :price=>3}, {:name=>"Chocolate Cookies", :price=>3}, {:name=>"Chocolate Chip Cookies", :price=>3}], [{:name=>"Tooth-Melters", :price=>12}, {:name=>"Tooth-Destroyers", :price=>12}, {:name=>"Enamel Eaters", :price=>12}, {:name=>"Dentist's Nightmare", :price=>20}], [{:name=>"Gummy Sour Apple", :price=>3}, {:name=>"Gummy Apple", :price=>5}, {:name=>"Gummy Moldy Apple", :price=>1}]], [[{:name=>"Grape Drink", :price=>1}, {:name=>"Orange Drink", :price=>1}, {:name=>"Pineapple Drink", :price=>1}], [{:name=>"Mints", :price=>13}, {:name=>"Curiously Toxic Mints", :price=>1000}, {:name=>"US Mints", :price=>99}]]]

# Get a "spinner"
vending_machine[0][0] #=> > [{:name=>"Tooth-Melters", :price=>12}, {:name=>"Tooth-Destroyers", :price=>12}, {:name=>"Enamel Eaters", :price=>12}, {:name=>"Dentist's Nightmare", :price=>20}]

# Get a spinner's first snack
vending_machine[1][1][0] #=> {:name=>"Mints", :price=>13}

# Get a spinner's first snack's price value
vending_machine[1][1][0][:price] #=> 13

# Work with a single snack
test_snack = vending_machine[0][1][0]
test_snack[:price] #=> 12
test_snack[:name] #=> "Tooth-Melters"

# Print out some fun data
puts "I'm definitely thinking about buying #{test_snack[:name]} and sharing my $#{test_snack[:price]} investment"
#=> I'm definitely thinking about buying Tooth-Melters and sharing my $12 investment
```

### Next Step: Working with the Nested Data Structure

Recall the opening lesson of this section. We described John Snow building up
complex maps to discover who had died during a cholera outbreak in
mid-19<sup>th</sup> century London. At this point, we can build an NDS that
represents a series of facts just as John Snow's surveys did.

But that's not _insight_. _Insight_ is when we pair data structures and Ruby
programming to provide answers that enlighten us as _humans_.

So let's choose an _insight_ to pursue that will help guide the rest of this
module.  Our guiding _insight_ question for the next several lessons is this:

***What is the total retail value of all the snacks in this vending machine?***

## Conclusion

NDS' serve to help us model complex data structures. As you seek to model the
real world, you'll build hybrids of the four simple data structures we taught
you. In time, you'll find ways to make lookups and updates in those structures
faster. Programs that excel at updating and retrieving information from data
structures are called _databases_. We didn't want to scare you, but learning to
build NDS' is your first step toward learning to write _databases_.

Later you'll learn how to store your NDS outside of your Ruby code (typically
in a "data file"), but for now it's OK to keep the data you work on and the
code with which you work on it in the same file.

...By the way, the total value is $`1192` in the vending machine.  Let's
find out how to calculate that _insight_.
