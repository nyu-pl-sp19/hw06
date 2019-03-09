# Homework 6 (20 Points)

The deadline for Homework 6 is Tuesday, March 19, 6pm. The late
submission deadline is Saturday, March 23, 6pm.

## Getting the code template

Before you perform the next steps, you first need to create your own
private copy of this git repository. To do so, click on the link
provided in the announcement of this homework assignment on
Piazza. After clicking on the link, you will receive an email from
GitHub, when your copy of the repository is ready. It will be
available at
`https://github.com/nyu-pl-sp19/hw06-<YOUR-GITHUB-USERNAME>`.  
Note that this may take a few minutes.

* Open a browser at `https://github.com/nyu-pl-sp19/hw06-<YOUR-GITHUB-USERNAME>` with your Github username inserted at the appropriate place in the URL.
* Choose a place on your computer for your homework assignments to reside and open a terminal to that location.
* Execute the following git command: <br/>
  ```git clone https://github.com/nyu-pl-sp19/hw06-<YOUR-GITHUB-USERNAME>.git```<br/>

The code template is provided in the file

```
src/hw06.ml
```

Simply replace all occurrences of `failwith "Not yet implemented"` by
your implementation of the corresponding function.

There are also some unit tests in

```
src/hw06_spec.ml
```

to help you test your code.

The root directory of the repository contains a shell
script [`build.sh`](build.sh) that you can use to compile
your code and the unit tests. Simply execute

```bash
./build.sh
```

and this will compile everything. You will have to install
`ocamlbuild` and `ounit` for this to work. Follow the [OCaml setup instructions](https://github.com/nyu-pl-sp19/ocaml-setup/#installation-build-tools-and-ides) to do this. Assuming there are no compilation
errors, this script will produce an executable file
`hw06_spec.native` in the root directory:

To run the unit tests, simply execute
```bash
./hw06_spec.native
```

Note that the tests will initially fail with an error message
`"Not yet implement"` for each unit test.

The root directory of the repository also contains a
file [`.merlin`](.merlin), which is used by
the [Merlin toolkit](https://github.com/ocaml/merlin) to provide
editing support for OCaml code in various editors and IDEs. Assuming
you have set up an editor or IDE with Merlin, you should be able to
just open the source code files and start editing. Merlin should
automatically highlight syntax and type errors in your code and
provide other useful functionality. 

**Important**: Run `build.sh` once immediately after cloning the
repository. This is needed so that Merlin is able to resolve the
dependencies between the different source code files.

## Submitting your solution

Once you have completed the assignment, you can submit your solution
by pushing the modified code template to GitHub. This can be done by
opening a terminal in the project's root directory and executing the
following commands:

```bash
git add .
git commit -m "solution"
git push
```

You can replace "solution" by a more meaningful commit message.

Refresh your browser window pointing at
```
https://github.com/nyu-pl-sp19/hw06-<YOUR-GITHUB-USERNAME>/
```
and double-check that your solution has been uploaded correctly.

You can resubmit an updated solution anytime by reexecuting the above
git commands. Though, please remember the rules for submitting
solutions after the homework deadline has passed.

# Problem 1: Higher-Order Functions (7 Points)

1. Reimplement the function

   ```ocaml
   unzip: ('a * 'b) list -> 'a list * 'b list
   ```
   
   from Homework 5 without recursion, using instead the function
   `List.fold_right` from OCaml's
   [`List`](https://caml.inria.fr/pub/docs/manual-ocaml/libref/List.html) module. You
   should not use any other predefined functions. **(2 Points)**
   
1. Implement the function `fold_right` using `List.fold_left`. Do not use
   any auxiliary recursive functions or any predefined functions other
   than `List.fold_left`. However, you are allowed to call
   `List.fold_left` more than once. **(2 Points)**

1. Implement a higher-order function 

   ```ocaml
   in_relation: ('a -> 'a -> bool) -> 'a list -> bool
   ```
   
   that takes a predicate `p` and a list `xs` and checks whether `p x
   y` evaluates to `true` for all consecutive elements `x` and `y` in
   `xs`. In particular, if we call `in_relation` with `(<)` for `p` on
   a list of integers, it will check whether the list is strictly
   sorted in increasing order.
   
   Examples:
   
   ```ocaml
   # in_relation (<) [1; 2; 3; 5; 6] ;;
   - : bool = true
   # in_relation (<) [1] ;;
   - : bool = true
   # in_relation (<) [] ;;
   - : bool = true
   # in_relation (<) [1; 3; 2] ;;
   - : bool = false
   # in_relation (=) [1; 1; 1] ;;
   - : bool = true
   # in_relation (=) [1; 1; 2] ;;
   - : bool = false
   # in_relation (fun x y -> y = x + 1) [1; 2] ;;
   - : bool = true
   # in_relation (fun x y -> y = x + 1) [1; 2; 4] ;;
   - : bool = false
   ```
   
   You are allowed to define auxiliary recursive functions but you are
   not allowed to use predefined recursive functions other than
   `List.fold_left`. Using `List.fold_left` is not mandatory. However,
   your implementation must be tail-recursive. **(3 Points)**


# Problem 2: Algebraic Data Types (13 Points)

1. Consider the following algebraic data type which we can use to
   describe nested lists over some type `'a`:
   
   ```ocaml
   type 'a nlist =
     | NList of ('a nlist) list
     | Atom of 'a
   ```
   
   Write a function 

   `flatten: 'a nlist -> 'a list` 
   
   
   that takes a nested list `xss` and returns the flattened list of
   atoms contained in `xss`. Examples:

   ```ocaml
   # flatten (Atom 1) ;;
   - : int list = [1]
   # flatten (NList [Atom 1; Atom 2]) ;;
   - : int list = [1; 2] ;;
   # flatten (NList [Atom 1; NList []; 2]) ;;
   - : int list = [1; 2] ;;
   # flatten (NList [Atom 1; NList [Atom 3; Atom 4]; Atom 5]) ;;
   - : int list = [1; 3; 4; 5]
   # flatten (NList [Atom 1; NList [Atom 2; Atom 3; 
       NList [Atom 4; Atom 5]; NList [Atom 6; Atom 7; NList [Atom 8];
         Atom 9]; Atom 10]]) ;;
   - : int list = [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
   ```

   Note that the order of the atoms in the (nested) lists should be
   preserved in the output list. 
   
   Your implementation must be tail-recursive to obtain full
   points. The only predefined functions and operators on lists that you
   are allowed to use are `List.rev`, and `@` (you won't
   necessarily need all of them). **(5 Points)**

   Hints:

   * You may want to first implement a simpler non-tail-recursive version
     before you attempt the tail-recursive version.

   * To obtain a tail-recursive implementation, build up the result
     list in reverse order, then use `reverse` at the end to get the
     expected output list.

1. Consider the following ADT for describing binary search trees:

   ```ocaml
   type tree =
     | Leaf
     | Node of int * tree * tree
   ```

   a. Write a function 
   
      ```ocaml
      fold: ('a -> int -> 'a) -> 'a -> tree -> 'a
      ```
      
      that takes an operation `op`, an initial value `z`, and a tree
      `t`. Assuming an in-order traversal of `t` yields the list of
      integer values `x1,...,xn` in that order, then `fold op z t`
      should compute `op (op... (op z x1)...) xn`. **(3 Points)**
      
   b. Use `fold` to implement a function
   
      ```ocaml
      list_of_tree: tree -> int list
      ```
      
      that computes the list of values stored in a tree in-order. You
      are additionally allowed to use the predefined function
      `List.rev` but no other predefined functions. In particular,
      your implementation should run in linear time in the size of the
      input tree. **(2 Points)**

   c. Use `fold` to implement a function
   
      ```ocaml
      is_sorted: tree -> bool
      ```
      
      that checks whether the given tree is strictly sorted in
      increasing order. You may assume that the tree does not contain
      the values `min_int` and `max_int`. **(3 Points)**
      
