# Nested Structures

## Learning Goals

- Identify nested structures can be mixed

## Introduction

You've spent the last few lessons learning about three basic nested structures:

* `Array` of `Array`s, or "AoA"
* `Array` of `Hash`es, or "AoH"
* `Hash` of `Hash`es, or "HoH"
* `Hash` of `Array`s, or "HoA"

By using these basic nested data structures, we've gained a foundation for
modeling many things in the world: vending machines, fictional characters,
the genealogy of a Hollywood family, and a medical record result-set.

But brace yourself, here comes something astounding:

***We can nest nested data structures in other nested data structures***

Your `Array` of `Hashes` (AoH) can have keys that point to `Array`s of
`Array`s (AoA). Or your nested node structure of `Hash`es of `Hash`es (HoH)
can have a key that points to another `Array` of `Hash`es (AoH). From what
we now know about NDS, we should be able to see that these NDS' can become
very complex!

In this lesson, we're going to walk you through an improved vending
machine model. Get a pen and some paper so that you can draw
pictures to help drive these points home. Using sketches will really
help you "get it." This is why developers in professional settings are
always sketching their NDS' whiteboards. It really works!

## Identify Nested Structures Can Be Mixed

As we try to more accurately model the world around us, we'll see that it's
very helpful to mix and match NDS to provide a better model of reality.

Let's update our model of a grid-based vending machine again. Remember,
they look like this:

![Grid-based vending machine](https://curriculum-content.s3.amazonaws.com/programming-univbasics-5/nested-arrays-lab/vending_grid.png)

We're going to work "top-down" and then re-work the modeling "bottom-up."
Neither method is more correct than the other. The problem of modeling sometimes
requires us to be flexible. "Top-down" means: describe the outermost structure
and then fill it in, and fill in any structures that were just added until you
reach the smallest structure. "Bottom-up" means: describe the innermost structure
and "wrap it" with a container repeatedly until the outermost container is reached.

### Array of Array of Array or Hashes...

Ultimately, in order to model a real grid-based vending machine (C20 is a candy bar
gum, B50 is a drink), we think the right NDS is:

```text
Array of Array
...of Array
...of Hashes
...with keys `:name` and `:pieces` where
......:name points to a `String`
......:pieces points to an Integer count of pieces
```

We're giving you the answer so that you can think about how this might work for
yourself. Draw it out on paper and see if you can predict how our explanation
will work. Teach your friend, your cat, or your imaginary friend Hogarth how ***you***
think things work **and then** read the explanation below. Don't lose this chance
to train your brain!

### Explaining the Vending Machine

Many vending machines use a grid to identify which snack one wants. The keyword
"grid" should immediately suggest an AoA.  Using a coordinate like `[3][2]` we
might pick the third element on the 4th row (remember, indexes start at 0 in a
Ruby vending machine).

### Explaining the Coordinate to Array

Here's a picture of what we just described in code:

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

This first step was good, but it misses an important detail about 
_real_ vending machines: most grid-based vending machines actually have multiple instances of the
snack at that coordinate. The structure above has _only one_
`String` at each coordinate. Also, each snack at a coordinate has some
properties. This model isn't quite accurate enough.

Most often, each coordinate points to a corkscrew-shaped spinner that rotates
enough to "pop" the first snack container off the spinner. This suggests that
each coordinate should point to an `Array` instead of a simple `String`. Thus
we're up to an "AoAoA" (`Array` of `Array`s of `Array`s).

### Explaining Each Snack Items as Hashes

So in the `Array` that represents the "spinner" device, how should we represent
each snack item?

A simple `String` would tell us what the snack's name is ***and that might
be good enough for your purposes***! But when _we_ close our eyes and imagine
a snack, we imagine a _name_ for the snack and a count of _pieces_, together.

We'll leave out the price for the moment. Snacks in our vending machine
are free like on _Star Trek_.

When bundling multiple properties and values together, the structure we want to
think of is a `Hash`. Our spinner `Array` should contain `Hash`es.

Each `Hash` should have a `:name` key pointing to a `String` and a `:pieces` key
pointing to an `Integer` of pieces in the snack.

Thus our NDS model is:

```text
Array of Array
...of Array
...of Hashes
...with keys `:name` and `:pieces` where
......:name points to a `String`
......:pieces points to an Integer count of pieces
```

Put your pencil down. You should have sketched out a profile of how this
vending machine's NDS works. We've taken a top-down approach: describing
the outer-most NDS and then describing the inner structures that make it.
To check our reasoning, let's reverse the process and describe from the
inner-most NDS and "wrap" it until we're back at the vending machine.

### Explain the Entire Vending Machine from the Bottom Up

Here's a bottom-up view ofthe same NDS:

<pre>
A count of pieces in the package represented as an `Integer`
Each snack has a name stored as a `String`
Each snack is represented by a `Hash` that has keys `:name` and `:count` that point to the name `String` and pieces `Integer` as just described

Multiple snacks are stored in an indexed collection, an `Array` that represents
the "spinner" device. Each "spinner" is accessible by a coordinate within an AoA "grid."
</pre>

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
   {:name=>"US Mints", :pieces=>99}]]]
] #=> [[[{:name=>"Vanilla Cookies", :pieces=>3}, {:name=>"Pistachio Cookies", :pieces=>3}, {:name=>"Chocolate Cookies", :pieces=>3}, {:name=>"Chocolate Chip Cookies", :pieces=>3}], [{:name=>"Tooth-Melters", :pieces=>12}, {:name=>"Tooth-Destroyers", :pieces=>12}, {:name=>"Enamel Eaters", :pieces=>12}, {:name=>"Dentist's Nighmare", :pieces=>20}], [{:name=>"Gummy Sour Apple", :pieces=>3}, {:name=>"Gummy Apple", :pieces=>5}, {:name=>"Gummy Moldy Apple", :pieces=>1}]], [[{:name=>"Grape Drink", :pieces=>1}, {:name=>"Orange Drink", :pieces=>1}, {:name=>"Pineapple Drink", :pieces=>1}], [{:name=>"Mints", :pieces=>13}, {:name=>"Curiously Toxic Mints", :pieces=>1000}, {:name=>"US Mints", :pieces=>99}]]]

# Get a "spinner"
vending_machine[0][0] #=> > [{:name=>"Tooth-Melters", :pieces=>12}, {:name=>"Tooth-Destroyers", :pieces=>12}, {:name=>"Enamel Eaters", :pieces=>12}, {:name=>"Dentist's Nighmare", :pieces=>20}]

# Get a spinner's first snack's pieces count
vending_machine[1][1][0][:pieces] #=> 13

# Work with a single snack
test_snack = vending_machine[0][1][0]
test_snack[:pieces] #=> 12
test_snack[:name] #=> "Tooth-Melters"

# Print out some fun data
puts "I'm definitely thinking about buying #{test_snack[:name]} and sharing my #{test_snack[:pieces]}"
```

### Next Step: Working with the Nested Data Structure

You still might not be certain of how we'll work with NDS'. Let's ask a
question that this NDS might help us understand:

***How many pieces total are in this vending machine?***

_Modeling_ the vending machine in a predictable way that NDS' require us
to use means that we can write code to answer this question in less than
a second! While our example is candy-in-a-vending-machine, this same structure
might be...

* Employees in divisions of a company
* Monsters in various rooms of a dungeon in a video game
* Zombies in map coordinates in the city

_Modeling_ is the first step. _Deriving insight_ is why we really learn
NDS'. We need them as models on which to write code to produce important
_insights_ like

* "How many candies do we need to bring when we restock the machine?"
* "How many lunches should we order for the company lunch (one per employee)"
* "Do we have enough magic points and a strong enough sword to defeat (number of monsters)?"
* "If we only have 5 bullets and a machete, what's the best way through a zombie city?"


....By the way, there are `1192` pieces in the vending machine.
Let's learn how to calculate this using the Ruby we know and NDS'!

## Conclusion

NDS' serve to help us model complex data structures. As you seek to model the
real world, you'll build hybrids of the four simple data structures we taught
you. In time, you'll find ways to make lookups and updates in those structures
faster. Programs that excel at updating and retrieving information from data
structures are called _databases_. We didn't want to scare you, but learning
to build NDS' is your first step toward learning to write _databases_.

Later you'll learn how to store your NDS outside of your Ruby code (typically
in a "data file"), but for now it's OK to keep the data you work on and the
code with which you work on it in the same file.
