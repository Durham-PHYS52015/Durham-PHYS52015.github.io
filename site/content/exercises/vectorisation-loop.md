---
title: "Vectorisation: loops with conditionals"
weight: 2
katex: true
---

# Vectorisation of a loop with conditionals

We'll be running these exercises on Hamilton or COSMA, so remind
yourself of how to log in and transfer code [if you need to]({{< ref
hamilton-quickstart.md >}}).



## Some expensive calculations

In this exercise, we're going to look at the ability of compilers to
vectorise a loop in the presence of conditionals inside the loop body.
The idea is to observe, and understand, what patterns permit (or do
not permit) vectorisation.

The code for this exercise is provided as a [tar archive]({{< code-ref
add_numbers.tgz >}}).

{{< hint info >}}

You can also clone the [entire course repository]({{< repo >}}), which
gives you access to the code for all the course exercises in the `code`
subdirectory.

I recommend that you create a new branch for each exercise and commit
your changes when editing code (since we'll reuse the same code
multiple times).

{{< /hint >}}

Download and unpack it with

```sh
$ wget {{< code-ref add_numbers.tgz >}}
$ tar zxvf add_numbers.tgz
```

This will create a new directory `add_numbers` with two
subdirectories.

```sh
$ cd add_numbers
$ ls
openmp serial
```

For now, let's look in the `serial` subdirectory.

```sh
$ cd serial
$ ls
Makefile      add_numbers.c io.c          main.c        proto.h
```

There is some source code and a
[`Makefile`](https://www.gnu.org/software/make/) that provides a
recipe for how to build the executable. It is just a text file and can
be edited with your favourite text editor. By running `make` you build
the executable.

Before we do this, we'll have to load the right compiler modules.
We'll use the intel compiler for this exercise, since it produces
better reports than gcc.

{{< tabs compiler-modules >}}
{{< tab Hamilton >}}
```sh
intel/xe_2018.2
gcc/9.3.0
```
{{< /tab >}}
{{< tab COSMA >}}
```sh
intel_comp/2018
```
{{< /tab >}}
{{< /tabs >}}

If we look at the `Makefile`

{{< code-include "add_numbers/serial/Makefile" "make" >}}

We can see that the compilation flags explictly turn off vectorisation
(the `-no-vec` part of `CFLAGS`).

After building the code, it produces an executable `add_numbers`. You
can run the code with
```sh
$ ./add_numbers N
```

This code performs some some floating point computations on an array
of random numbers and then sums them up.

{{< task >}}
Run the code in serial, it reports the time it needs to perform the
calculations. Record this.
{{< /task >}}

{{< task >}}
Switch on vectorisation, and turn on vectorisation reports, in the
`Makefile` and recompile.
{{< /task >}}

{{< question >}}
Does the compiler report whether it successfully vectorised any loops?

Run the code again, is it faster than the unvectorised version?
{{< /question >}}

Now we are going to edit the main computational loop to see under what
circumstances the compiler can still vectorise the loop.

{{< task >}}
Edit the main loop in `add_numbers.c` and modify it so that `result_i`
is only added to the final `result` if it is greater than zero.
{{< /task >}}

{{< question >}}
Compile the code again, can the compiler still vectorise the loop?

What type of transformation is this an example of?
{{< /question >}}

{{< task >}}
Now edit the main loop in `add_number.c` further and add an additional
conditional that exits the loop _early_ as soon as `result` exceeds
\\(10^{20}\\). 

{{< details title="Hint" >}}
You can exit a loop with a `break` statement. For example
```c
for (i = 0; i < 100; i++) {
  if (i*i > 10) {
    break;
  }
}
```
Will exit the loop as soon as `i*i` is larger than 10.
{{< /details >}}
{{< /task >}}

{{< question >}}
Compile this new version of the code. Is vectorisation of the loop
still possible?

If not, why is this?
{{< /question >}}