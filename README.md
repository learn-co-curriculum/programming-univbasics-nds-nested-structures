# Nested Structures

## Learning Goals

- Identify nested structures can be mixed

## Introduction

You've spent the last few lessons learning about three basic nested structures:

* `Array` of `Array`s
* `Array` of `Hash`es
* `Hash` of `Hash`es

And it would be a fair question to ask WHY we've been teaching you with
exercises with vending machines, fictional characters and the genealogy of
Hollywood royalty. Our goal is two-fold:

1. We want you to see that ***anything*** can be modeled in programming. From
   characters in a book to verb conjugation rules to customers and products. No
   topic is off-limits!
2. We also want you to see that modeling complex data with nested data
   structures is possible. Through nesting we can model very complex systems

Ultimately, at some point you will be called to build a program by a person who
knows very little about programming. They won't be able to tell you what kind
of data structure to create, ***you*** will have to create it. And you'll want
to create it in a sensible way that your collaborators (other humans) and
computers can both work with.

## Identify Nested Structures Can Be Mixed

As we try to more faithfully model the world around us, we'll see that it's
very helpful to mix and match NDS to provide a better model of reality.

Let's update the model of a grid-based vending machine again.

### Array of Array of Array or Hashes...

Ultimately, in order to model a real grid-based vending machine (J2 is chewing
gum, K3 is some mints), we think the right NDS is:

```text
Array of Array
...of Array
...of Hashes
...with keys `:name` and `:pieces`
...where :name points to a `String` and :pieces points to another Array
```

We're giving you the answer so that you can think about how this might work for
yourself. Draw it out on paper and see if you can think how our explanation
will work.

### Explaining the Vending Machine

Many vending machines use a grid to identify which snack one wants. The keyword
"grid" should immediately suggest an AoA.  Using a coordinate like `[3][2]` we
might pick the third element on the 4th row (remember, indexes start at 0 in a
Ruby vending machine).

But most vending machines that we know actually have multiple instances of the
snack at that location. It holds the snack in a row. Row should suggest another
data structure.

### Explaining the Coordinate to Array

Because the snacks are in an unlabeled, "row" on a little spiral twist device,
they're pretty much on an `Array`. So we have an Array of Array of Array.

Here's an empty picture of what that might look like in code:

```ruby
vending_machine = [
 # Row 0
 [
  # Column 0
  ["Cookies", "Cookies", "Cookies", "Cookies"],

  # Column 1
  ["Tooth-Melters", "Tooth-Melters"],

  # Column 2
  ["Gummy Apple", "Gummy Apple", "Gummy Moldy Apple"]
 ],

 # Row 1
 [
  # etc.
 ],

  # Row 2
 [
  # etc.
 ],

 # Etc.
] #=> => [[["Cookies", "Cookies", "Cookies", "Cookies"], ["Tooth-Melters", "Tooth-Melters"], ["Gummy Apple", "Gummy Apple", "Gummy Moldy Apple"]], [], []]
```

If you're having trouble envisioning this, try filling out this code more and
work with the data structure. You're not wasting time to practice building
structures like this. It's one of the most fundamental parts of data
processing.

### Explaining the Snack Items as Hashes

But what's in each slots of that spiral twist device? It's not really _just_ a
`String`, it's more like a packet with information (name) and also a certain
number of pieces. One instance of a snack.

A thing with properties with values...hmmm. Which structure to use?

That should trigger `Hash` in your mind.  A `:name` property should point to
`String`. The `:pieces` key should point to...well, a mixed collection of
pieces, that's probably an `Array`. Corn chip 0, corn chip 1, corn chip 2,
etc.; Chocolate nugget 0, chcolate nugget 1, etc.

But let's imagine that there are 3 snacks located at `[3][2]` thus:

```ruby
vending_machine[3][2] = [
  { name: "peanut butter cup", pieces: [ "cup 1", "cup 2", "cup 3"] },
  { name: "peanut butter cup", pieces: [ "cup 1", "cup 2", "cup 3"] },
  { name: "peanut butter cup", pieces: [ "cup 1", "cup 2", "cup 3"] },
]
```

And now let's pretend you just bought a snack from `vending_machine[3][2]`

`vending_machine[3][2]` now looks like (because you have a snack!):

```ruby
vending_machine[3][2] = [
  { name: "peanut butter cup", pieces: [ "cup 1", "cup 2", "cup 3"] },
  { name: "peanut butter cup", pieces: [ "cup 1", "cup 2", "cup 3"] },
]
```

...but your hand now contains:

```ruby
  { name: "peanut butter cup", pieces: [ "cup 1", "cup 2", "cup 3"] },
```

You might then decide:

```ruby
my_name = "Byron"
snack = machine[3][2].pop
friends = ["Maisie", "Harley", "Sadie"]
i = 0

while i < friends.length do
  puts "#{my_name} gives #{snack[i]} to #{friends[i]}
end
```

So ultimately you decide the right NDS for modeling a vending machine is:

AoAoAoH where the Hash has keys `:name` and `:pieces`

Obviously `:pieces` could be more than simple `Strings` (perhaps `Hash`es full
of nutritional data?), but at some point when we model reality we say "good
enough."

## Conclusion

NDS serve to help us model complex data structures. As you seek to model the
real world you'll build hybrids of the three simple data structures we taught
you. With time you'll find ways to make lookups in those structures faster.
You'll learn to optimize these structures and tune them to make updating and
retrieving data fast.

By the way, programs that excel at updating and retrieving information from data
structures are called _databases_.

## Resources
