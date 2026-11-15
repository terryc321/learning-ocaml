
# Day 1 ocaml learning

test


## installing ocaml and friends

```
# install opam
# initialise
opam init

# choose which switch to work on 
opam switch

# for now choose the default switch
eval $(opam env)
```

## create a first project

lets now create a project called day16 


```
dune init project day16
cd day16
```

here is how to rebuild project

```
dune build
```

here is how to run the project - or execute the project
```
dune exec day16
```

we can start a repl of sorts in the project directory
```
dune utop
```

but since we do not have any libraries we cannot load anything in
each file becomes its own module bin/main.ml file becomes Main module 

``` 
> #show Main;;
```

```
module Main : sig end
```

we can load an ocaml file directly
```
> #use "bin/main.ml";;
```

## emacs aside 

in emacs ansi-term and switch out / into terminal mode in ansi term to run dune utop interactively
```
C-c C-j
C-c C-k
alternatively keep a 2nd window open and C-x o out of it , but cannot use window like emacs window without C-j C-k
```

repl evaluations need double ```;;``` colons
```
> 123+456;;
```

# ocaml strings 

lets define a string 
"happy"

## length of a string
String.length "asdf";;

## first element of a string 
```ocaml
let s = "happy" in s.[0] 
```

```
#show String;;
i can see all components of the string datatype


# do something to each element of string ?
# s.[0] is 1st
# s.[n-1] is last item of string (provided string has length n)

# aside
# given a string of hexadecimals , convert to a list of binary bits 0 and 1s 
# hex2bin 

# want have a key value pair setup like this

0 = 0000
1 = 0001
2 = 0010
3 = 0011
4 = 0100
5 = 0101
6 = 0110
7 = 0111
8 = 1000
9 = 1001
A = 1010
B = 1011
C = 1100
D = 1101
E = 1110
F = 1111

exception InvalidHexCharacter

let hex_to_bools c =
  match c with
  | '0' -> [0;0;0;0]
  | '1' -> [0;0;0;1]
  | '2' -> [0;0;1;0]
  | '3' -> [0;0;1;1]
  | '4' -> [0;1;0;0]
  | '5' -> [0;1;0;1]
  | '6' -> [0;1;1;0]
  | '7' -> [0;1;1;1]
  | '8' -> [1;0;0;0]
  | '9' -> [1;0;0;1]
  | 'A' -> [1;0;1;0]
  | 'B' -> [1;0;1;1]
  | 'C' -> [1;1;0;0]
  | 'D' -> [1;1;0;1]
  | 'E' -> [1;1;1;0]
  | 'F' -> [1;1;1;1]
  | _   -> raise InvalidHexCharacter

# how do we try this out ?
# let s = "0123456789ABCDEF" in


when i see letter 'F' in a string , i can translate it to sequence 1111 bits

# append two lists together
[1;2;3] @ [4;5;6] ;;


# dune build
# dune exec day16
# keep having to kill utop each time change made to dune project source code

(* for each character in string , show what character we are testing and print the corresponding list *)
(* ['0';'1';'2';'3';'4';'5';'6';'7';'8';'9';'A';'B';'C';'D';'E';'F']  *)
let () = let hexchars = string_to_chars "0123456789ABCDEF" in
List.iter (fun x -> Printf.printf "testing %c => " x ;
	       Day16.Util.print_int_list (Day16.Util.hex_to_bools x)) hexchars

(* given a string of hexadecimal characters , take each list and append them all together *)
let () = List.iter (fun x -> Day16.Util.hex_to_bools x) "ABCD"

# List.iter is for () unit -- side effects ,
# List.map where we want results passed back

# eventually we can convert triple A to a sequence of booleans , here int either 1 or 0 
Day16.Util.hex2bools "AAA";;
- : int list = [1; 0; 1; 0; 1; 0; 1; 0; 1; 0; 1; 0]
```




