Day2 learning ocaml
--------------------

naming of modules and finding modules inside a dune project by module 
inside a dune project when we use dune utop get put into prompt

bin/main.ml -- top down execute these instructions 
lib/util.ml -- some helper functions in here
both directories inside day16 dune project

#show Main
module Main : sig end

#show Day16.Util
module Util :
  sig
    exception InvalidHexCharacter
    val hex_to_bools : char -> int list
    val print_int_list : int list -> unit
    val string_to_chars : string -> char list
    val hex2bools : string -> int list
  end

filename of project becomes the module name in ocaml
the Main module from bin/main.ml
the Day16.Util module lib/util.ml

notice not the plain Util module but Day16.Util, why.

we can keep doing dune utop for as many times as we reload ? 
not live coding

save us typing Day16.Util.... each time we can open the util module
#open Day16.Util;;

emacs integration with dune utop does not work , autocomplete on tab is broken 
- broken tools 

utop how do we show entire output of a long string without it being truncated ?

since utop is running in a shell , theres no emacs editor integration , again 
broken tooling


# opening a module 

pollute the namespace but makes development a tad easier as less typing

```open Day16.Util``` rather than ```#open Day16.Util``` the latter is an err

# large integer arithmetic

opam install zarith

now we have a module called Z 

now we need to update dune file to know about zarith library 

(executable
 (name factorial)
 (libraries zarith))

confusingly zarith makes a module called Z rather than Zarith ?

# from ocaml prompt use zarith

```
#use "topfind"

- : unit = ()
Findlib has been successfully loaded. Additional directives:
  #require "package";;      to load a package
  #list;;                   to list the available packages
  #camlp4o;;                to load camlp4 (standard syntax)
  #camlp4r;;                to load camlp4 (revised syntax)
  #predicates "p,q,...";;   to set these predicates
  Topfind.reset();;         to force that packages will be reloaded
  #thread;;                 to enable threads

- : unit = ()
```

```
#require "zarith";;
```

# factorial aside 

```
let rec factorial n =
  if n <= 1 then Z.one
  else Z.mul (Z.of_int n) (factorial (n - 1))

let rec foo2 (n : int) (p : Z.t) = 
  let p2 = Z.mul (Z.of_int n) p in 
  Printf.printf "Foo %d = %s\n" n (Z.to_string p2) ; 
  if n > 1000 then () else foo2 (n + 1) p2 

(* lets see what foo_demo does *)
let foo2_demo () = foo2 1 Z.one

```

and the output at end of foo2_demo ()

```
Foo 1001 = 4027896...000   # truncated was 2571 digits long  
```

here is an important point , should i be concerned with using a number like an integer and 
let the computer handle if it overflows to convert to a big integer automatically , 
or should i worry about fixnums and be surprised when it overflows and have to rewrite all the 
code ?

# overflow warnings ocaml

do i get a warning overflow occurred ? 

