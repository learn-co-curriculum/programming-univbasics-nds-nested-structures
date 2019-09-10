# Nested Structures

## Learning Goals

- Identify nested structures can be mixed

## Introduction

You've spent the last few lessons learning about three basic nested structures:

* `Array` of `Array`s
* `Array` of `Hash`es
* `Hash` of `Hash`es
* `Hash` of `Array`s

By using these basic nested data structures we've gained a foundation for
modeling many things in the world: vending machines, fictional characters,
the genealogy of a Hollywood family, and a medical record result-set.

But brace yourself, here comes something astounding:

***We can nest nested data structures in other nested data structures***

Your `Array` of `Hashes` (AoH) can have keys that point to `Array`s of
`Array`s (AoA). Or your nested node structure of `Hash`es of `Hash`es
can have a key that points to an `Array` of `Hash`es.

In this lesson we're going to talk you through how to represent a
vending machine. Get a pen and scratch paper so that you can draw 
pictures to help drive these points home. We mentioned that we learn
NDS' because describing how they operate as paragraphs is challenging.

## Identify Nested Structures Can Be Mixed

As we try to more faithfully model the world around us, we'll see that it's
very helpful to mix and match NDS to provide a better model of reality.

Let's update our model of a grid-based vending machine again. Remember,
they look like this:

![Grid-based vending machine](https://curriculum-content.s3.amazonaws.com/programming-univbasics-5/nested-arrays-lab/vending_grid.png)

### Array of Array of Array or Hashes...

Ultimately, in order to model a real grid-based vending machine (C20 is a candy bar
gum, B50 is a drink), we think the right NDS is:

```text
Array of Array
...of Array
...of Hashes
...with keys `:name` and `:pieces` where :name points to a `String` and :pieces points to another Array
```

We're giving you the answer so that you can think about how this might work for
yourself. Draw it out on paper and see if you can think how our explanation
will work. Teach your friend, your cat, or your imaginary friend Hogarth how ***you***
think things work and then read the explanation below.

### Explaining the Vending Machine

Many vending machines use a grid to identify which snack one wants. The keyword
"grid" should immediately suggest an AoA.  Using a coordinate like `[3][2]` we
might pick the third element on the 4th row (remember, indexes start at 0 in a
Ruby vending machine).

### Explaining the Coordinate to Array


Here's an empty picture of what that might look like in code:

```ruby
vending_machine = [
   [
     ["Vanilla Cookies", "Pistachio Cookies", "Chocolate Cookies", "Chocolate Chip Cookies"],
     ["Tooth-Melters", "Tooth-Destroyers", "Enamel Eaters", "Dentist's Nighmare"],
     ["Gummy Apple", "Gummy Apple", "Gummy Moldy Apple"]
   ],
   [
     ["Grape Drink", "Orange Drink", "Pineapple Drink"],
     ["Mints", "Curiously Toxic Mints", "US Mints"]
    ]
  ]
  
#=> [[["Vanilla Cookies", "Pistachio Cookies", "Chocolate Cookies", "Chocolate Chip Cookies"], ["Tooth-Melters", "Tooth-Destroyers", "Enamel Eaters", "Dentist's Nighmare"], ["Gummy Apple", "Gummy Apple", "Gummy Moldy Apple"]], [["Grape Drink", "Orange Drink", "Pineapple Drink"], ["Mints", "Curiously Toxic Mints", "US Mints"]]]
```

### Explaining the Coordinate as Pointing to an Array

Most grid-based vending machines actually have multiple instances of the
snack at that location. The structure above has _only one_
`String` at each coordinate doesn't match reality well enough.

> **NOTE**: No one tells us programmers "you've modeled this well" or
> you've modeled this well-enough so stop. Part of the job is knowing
> how to model the data so you can work with it.

Most often, each coordinate points to a corkscrew-shaped spinner that rotates
enough to "pop" the first snack container off the spinner. This suggests that each coordinate should point to an `Array`.

So each coordinate should point to an `Array`.

### Explaining Each Snack Items as Hashes

So on this "spinner" `Array`, how should we represent each snack item? Well a
simple `String` would tell us what the snack's name is ***and that might
be good enough for your purposes***! But when _we_ close our eyes and imagine
a snack, we imagine a _name_ for the snack and a collection of _pieces_, together.

When bundling multiple properties and values together, the structure we want to
think of is a `Hash`. Our spinner `Array` should contain`Hash`es.

Each `Hash` should have a `:name` key pointing to a `String` and a `:pieces` key
pointing to an `Integer` of pieces in the snack.

### Explain the Entire Vending Machine from the Bottom Up

We just finished a "top-down" model of a vending machine. Let's try a
"bottom-up" model:


<pre>
A snack count of pieces in the package represented as `Integer`s
Each snack has a name stored as a `String`
Each snack is represented by a `Hash` that has keys `:name` and `:string` that point to the name `String` and pieces `Array` as just described

Multiple snacks are stored in an indexed collection, an `Array`

There are many of these `Array`s and they are accessible within an AoA "grid"
</pre>


A real version of this data structure is the following:

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
   {:name=>"US Mints", :pieces=>99}]]]
] #=> [[[{:name=>"Vanilla Cookies", :pieces=>3}, {:name=>"Pistachio Cookies", :pieces=>3}, {:name=>"Chocolate Cookies", :pieces=>3}, {:name=>"Chocolate Chip Cookies", :pieces=>3}], [{:name=>"Tooth-Melters", :pieces=>12}, {:name=>"Tooth-Destroyers", :pieces=>12}, {:name=>"Enamel Eaters", :pieces=>12}, {:name=>"Dentist's Nighmare", :pieces=>20}], [{:name=>"Gummy Sour Apple", :pieces=>3}, {:name=>"Gummy Apple", :pieces=>5}, {:name=>"Gummy Moldy Apple", :pieces=>1}]], [[{:name=>"Grape Drink", :pieces=>1}, {:name=>"Orange Drink", :pieces=>1}, {:name=>"Pineapple Drink", :pieces=>1}], [{:name=>"Mints", :pieces=>13}, {:name=>"Curiously Toxic Mints", :pieces=>1000}, {:name=>"US Mints", :pieces=>99}]]]

```

## Conclusion

NDS serve to help us model complex data structures. As you seek to model the
real world you'll build hybrids of the three simple data structures we taught
you. With time you'll find ways to make lookups in those structures faster.
You'll learn to optimize these structures and tune them to make updating and
retrieving data fast.

By the way, programs that excel at updating and retrieving information from data
structures are called _databases_.

Additionally, you might be able to guess that having to key in these NDS' in order
to run your program is a little bit un-fun. Programmers have learned to 
store representations of their NDS in files that they read in at runtime. We call
these _data files_. You'll learn more about reading in complex data later on!
