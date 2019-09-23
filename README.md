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

The other week we saw a vending machine in a hospital. After swiping your
credit card or using your phone's wallet, you entered a **grid coordinate**.
At each intersection (coordinate) in the vending machine, there was a
"spinner." Pay attention to this noun, "spinner." We're going to use it a lot
in the coming lessons. It's a device that looks like a corkscrew. It **holds
multiple snack packages** in it.  When you've paid, the spinner spins and
pushes the front-most snack off of the corkscrew where it falls to a retrieval
box.

On each snack's packaging a "pieces" count was clearly printed, in addition to
a name.

> **_Your_ Model May Vary**: It's certainly true that there are many other
> details that we could record in this model. Price, for example, might live on
> each snack or `total_calories`. We've preferred to stick with `:pieces` and
> `:name`. If you think each coordinate should be a `Hash` with a `:price` and
> an `Array` of snacks, that's **totally fine**. Discussion of how to model an
> NDS is what developers spend a great number of their meetings and pair
> programming sessions discussing!

## Identify that Nested Structures Can Be Mixed

Ultimately, in order to model a real grid-based vending machine (C20 is a candy
bar off of a spinner of gum packages, B50 is a drink off of a spinner of
drinks), we think the right NDS is:

```text
Array of Array
...of Array
...of Hashes
.....with keys `:name` and `:pieces` where
.......:name points to a `String`
.......:pieces points to an Integer count of pieces
```

We're giving you the answer so that you can think about how this might work for
yourself. Draw it out on paper and see if you can predict how our explanation
will work. Teach your friend, your cat, or your imaginary friend Hogarth how ***you***
think things work **and then** read the explanation below. Don't lose this chance
to train your brain!

### Explain the Entire Vending Machine from the Top Down

<pre>
We have a coordinate grid. That's an AoA
In each coordinate, there's a "spinner" with multiple snacks
Each snack has two important facts associated with it, a :name and a :piece
count
</pre>

### Explain the Entire Vending Machine from the Bottom Up

Here's a bottom-up view of the NDS:

<pre>
A count of pieces in the package represented as an `Integer`
Each snack has a name stored as a `String`
Each snack collects those facts in a `Hash` that has keys `:name` and `:count`.

Multiple snacks are stored in an indexed collection, an `Array` that represents
the "spinner" device. Each "spinner" is accessible by a coordinate within an AoA "grid."
The vending machine is the super-container AoA.
</pre>

Whichever approach feels more natural to you, feel free to use it. Sometimes
our brains find a logical "foothold" while working bottom-up. Sometimes our
brains are thinking in a big-picture sense first. Either way is OK! Oftentimes
the ability to hop from one end to the other is used in job interviews to test
your mental flexibility. Try to develop both approaches.

### Show the Data Structure as Ruby Code

A real version of this data structure is the following. We've included the
structure, as well as a few Ruby commands, to get some sample data out of
the NDS.

```ruby
vending_machine = [[[{:name=>"Vanilla Cookies", :pieces=>3},
   {:name=>"Pistachio Cookies", :pieces=>3},
   {:name=>"Chocolate Cookies", :pieces=>3},
   {:name=>"Chocolate Chip Cookies", :pieces=>3}],
  [{:name=>"Tooth-Melters", :pieces=>12},
   {:name=>"Tooth-Destroyers", :pieces=>12},
   {:name=>"Enamel Eaters", :pieces=>12},
   {:name=>"Dentist's Nighmare", :pieces=>20}],
  [{:name=>"Gummy Sour Apple", :pieces=>3},
   {:name=>"Gummy Apple", :pieces=>5},
   {:name=>"Gummy Moldy Apple", :pieces=>1}]],
 [[{:name=>"Grape Drink", :pieces=>1},
   {:name=>"Orange Drink", :pieces=>1},
   {:name=>"Pineapple Drink", :pieces=>1}],
  [{:name=>"Mints", :pieces=>13},
   {:name=>"Curiously Toxic Mints", :pieces=>1000},
   {:name=>"US Mints", :pieces=>99}]]] #=> [[[{:name=>"Vanilla Cookies", :pieces=>3}, {:name=>"Pistachio Cookies", :pieces=>3}, {:name=>"Chocolate Cookies", :pieces=>3}, {:name=>"Chocolate Chip Cookies", :pieces=>3}], [{:name=>"Tooth-Melters", :pieces=>12}, {:name=>"Tooth-Destroyers", :pieces=>12}, {:name=>"Enamel Eaters", :pieces=>12}, {:name=>"Dentist's Nighmare", :pieces=>20}], [{:name=>"Gummy Sour Apple", :pieces=>3}, {:name=>"Gummy Apple", :pieces=>5}, {:name=>"Gummy Moldy Apple", :pieces=>1}]], [[{:name=>"Grape Drink", :pieces=>1}, {:name=>"Orange Drink", :pieces=>1}, {:name=>"Pineapple Drink", :pieces=>1}], [{:name=>"Mints", :pieces=>13}, {:name=>"Curiously Toxic Mints", :pieces=>1000}, {:name=>"US Mints", :pieces=>99}]]]

# Get a "spinner"
vending_machine[0][0] #=> > [{:name=>"Tooth-Melters", :pieces=>12}, {:name=>"Tooth-Destroyers", :pieces=>12}, {:name=>"Enamel Eaters", :pieces=>12}, {:name=>"Dentist's Nighmare", :pieces=>20}]

# Get a spinner's first snack
vending_machine[1][1][0] #=> {:name=>"Mints", :pieces=>13}

# Get a spinner's first snack's pieces count
vending_machine[1][1][0][:pieces] #=> 13

# Work with a single snack
test_snack = vending_machine[0][1][0]
test_snack[:pieces] #=> 12
test_snack[:name] #=> "Tooth-Melters"

# Print out some fun data
puts "I'm definitely thinking about buying #{test_snack[:name]} and sharing my #{test_snack[:pieces]}"
#=> I'm definitely thinking about buying Tooth-Melters and sharing my 12
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

***How many pieces total are in this vending machine?***

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

...By the way, there are `1192` pieces in the vending machine.  Let's find out
how to calculate that _insight_.
