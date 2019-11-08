# Reduce, et. al.

`Reduce`, `inject`, `fold`, `accumulate`, `aggregate` and `compress`. All
different names for the same [*higher-order* function][Wikipedia Reduce] that
iterates over an array[1], returning a single argument. Beginners and seasoned
programmers alike can get confused by its heady syntax. But this isn't just an
article about **how** and **when** to use this nugget; I also want to convince
you that you **should** use it, and **why**.

## Anatomy

In *Ruby* `inject` takes an initial value and a block; the block receives a memo
argument and the current value.

In *Javascript* `reduce` takes two arguments: a reducer function and an initial
value. The callback function in turn takes four arguments: an accumulator, the
current value, the current index, and the source array. In practice you normally
just use the accumulator and current value. Rarely is there a need for the
current index, and the source array is used even less often.

Let's take a look at an example (I'm using Ruby):

```ruby
my_nums = [3, 50, 17, 5]

evens = my_nums.inject([]) do |mem, num|
  mem << num if num % 2 == 0
  return mem
end
```

The value of the variable `evens` will be an array containing the number `50`
after this code runs. The `my_nums` variable remains untouched.

The value of `mem` starts as the initial value passed into inject. As it
iterates over the values, the value of `mem` *can* be updated by your code, and
*will* be updated by the return value of the current iteration. In the above
example, `mem` starts as `[]`. In each iteration, it conditionally pushes
the current number (`num`) into the array if it is even. Regardless, it then
returns the value of `mem`, which value gets used as the value of `mem` in the
next iteration.

| Iteration | mem  | num | return |
|----------:|------|----:|--------|
|         1 | []   |   3 | []     |
|         2 | []   |  50 | [50]   |
|         3 | [50] |  17 | [50]   |
|         4 | [50] |   5 | [50]   |

## Memo? Accumulator? Wat?

If you aren't sure about how to use `reduce`, but have seen it in others' code,
you've probably wondered why they didn't use a simple and easier-to-read `for`
loop. If you're a seasoned developer comfortable with `reduce` and its cousins,
you at least remember that uneasy feeling you got when you first encountered it.

![Meme: "Have more money than usual. Did I forget to pay a bill?"](./uneasy.jpg)

If you're interested in the benefits of using `reduce` and friends, keep
reading; that's coming later in the article. For now we will look at its
anatomy.

```ruby
my_nums = [3, 17, 5, 50]

evens = my_nums.inject() do |mem, num|
  mem << num if num % 2 == 0
end
```

What's with that? Even the greenest of devs should be able to make an educated
guess as to what that code is doing; but why write it that way if a clearer
`for` will do the job?

[Wikipedia Reduce]: https://en.wikipedia.org/wiki/Fold_(higher-order_function)
