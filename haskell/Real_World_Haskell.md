- [Real World Haskell Codes](#real-world-haskell-codes)
  - [Overview](#overview)
  - [Chapter 3  - Defining Types, Streamlining Functions](#chapter-3----defining-types,-streamlining-functions)
    - [BookStore.hs](#bookstore.hs)
    - [BookStore2.hs](#bookstore2.hs)
    - [AlgebraicVector.hs](#algebraicvector.hs)
    - [ShapeUnion.hs](#shapeunion.hs)
    - [add.hs](#add.hs)
    - [BookStore3.hs](#bookstore3.hs)
    - [BookStore4.hs](#bookstore4.hs)
  - [Chapter 4  - Functional Programming](#chapter-4----functional-programming)
    - [InteractWith.hs](#interactwith.hs)
    - [FixLines.hs](#fixlines.hs)
    - [ch4.exercises.hs](#ch4.exercises.hs)
    - [IntParser.hs](#intparser.hs)
  - [Chapter 5  - Writing a Library: Working with JSON Data](#chapter-5----writing-a-library:-working-with-json-data)
    - [SimpleJson1.hs](#simplejson1.hs)
    - [SimpleJSon2.hs](#simplejson2.hs)
    - [PutJSON.hs](#putjson.hs)
    - [PrettyJSON.hs](#prettyjson.hs)
  - [Chapter 8  - Efficient File Processing, Regular Expressions, and Filename Matching](#chapter-8----efficient-file-processing,-regular-expressions,-and-filename-matching)
    - [ElfMagic.hs](#elfmagic.hs)
    - [HighestClose.hs](#highestclose.hs)
  - [Chapter 10 - Code Case Study: Parsing a Binary Data Format](#chapter-10---code-case-study:-parsing-a-binary-data-format)
    - [PNM.hs](#pnm.hs)
    - [Parse.hs](#parse.hs)
    - [TreeMap.sh](#treemap.sh)
  - [Chapter 13 - Data Structures](#chapter-13---data-structures)
    - [passwd-al.hs](#passwd-al.hs)
    - [buildmap.hs](#buildmap.hs)
    - [funcrecs.hs](#funcrecs.hs)
    - [funcrecs2.hs](#funcrecs2.hs)
    - [passwdmap.hs](#passwdmap.hs)
  - [Chapter 14 - Monads](#chapter-14---monads)
    - [Maybe.hs](#maybe.hs)
    - [MultiplyTo.hs](#multiplyto.hs)
    - [SimpleState.hs](#simplestate.hs)
    - [State.hs](#state.hs)
  - [Chapter 27 - Sockets and Syslog](#chapter-27---sockets-and-syslog)
    - [Requirements](#requirements)
    - [UDP Syslog Server and Client](#udp-syslog-server-and-client)
    - [TCP Syslog Server and Client](#tcp-syslog-server-and-client)

# Real World Haskell Codes<a id="sec-1" name="sec-1"></a>

## Overview<a id="sec-1-1" name="sec-1-1"></a>

It this section is provided Real World Haskell (RWH) files, testing
and running examples in Haskell v7.10.2. 

## Chapter 3  - Defining Types, Streamlining Functions<a id="sec-1-2" name="sec-1-2"></a>

### BookStore.hs<a id="sec-1-2-1" name="sec-1-2-1"></a>

-   <rwh/ch03/BookStore.hs>

```haskell
data BookInfo = Book Int String [String]
                deriving (Show)



data MagazineInfo = Magazine Int String [String]
                    deriving (Show)


type CustomerID = Int        --- Type synonym 
type ReviewBody = String 

data BookReview = BookReview BookInfo CustomerID String 

data BetterReviw = BetterReviw BookInfo CustomerID ReviewBody 

type BookRecord = (BookInfo, BookReview) 

myInfo = Book 9780135072455 "Algebra of Programming"
              ["Richard Bird", "Oege de Moor"]
```

Running:

```haskell
>>> :load "rwh/ch03/BookStore.hs"
[1 of 1] Compiling Main             ( rwh/ch03/BookStore.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> myInfo
Book 9780135072455 "Algebra of Programming" ["Richard Bird","Oege de Moor"]
>>> 
>>> :type myInfo
myInfo :: BookInfo
>>> 

>>> Book 0 "The Book of Imaginary Beings" ["Jorge Luis Borges"]
Book 0 "The Book of Imaginary Beings" ["Jorge Luis Borges"]
>>> 

>>> :type Book 1 "Cosmicomics" ["Italo Calvino"]
Book 1 "Cosmicomics" ["Italo Calvino"] :: BookInfo
>>> 

>>> let cities = Book 173 "Use of Weapons" ["Iain M. Banks"]

>>> cities
Book 173 "Use of Weapons" ["Iain M. Banks"]
>>>
```

### BookStore2.hs<a id="sec-1-2-2" name="sec-1-2-2"></a>

<rwh/ch03/BookStore2.hs> 

```haskell
type CardHolder = String
type CardNumber = String
type Address = [String]
type CustomerID = Int

data BillingInfo = CreditCard CardNumber CardHolder Address
                 | CashOnDelivery
                 | Invoice CustomerID
                   deriving (Show)
```

Loading 

```haskell
>>> CreditCard "2901650221064486" "Thomas Gradgrind" ["Dickens", "England"]
CreditCard "2901650221064486" "Thomas Gradgrind" ["Dickens","England"]
>>> 
>>> it
CreditCard "2901650221064486" "Thomas Gradgrind" ["Dickens","England"]
>>> 
>>> :type it
it :: BillingInfo
>>> 

>>> 
Invoice 10
>>> 

>>> CashOnDelivery
CashOnDelivery
```

### AlgebraicVector.hs<a id="sec-1-2-3" name="sec-1-2-3"></a>

<rwh/ch03/AlgebraicVector.hs>

```haskell
-- x and y coordinates or lengths.
data Cartesian2D = Cartesian2D Double Double 
                   deriving (Eq, Show)

--- Angle and distance (magnitude)
data Polar2D = Polar2D Double Double  
               deriving (Eq, Show)
```

Running 

```haskell
>>> Cartesian2D (sqrt 2) (sqrt 2)
Cartesian2D 1.4142135623730951 1.4142135623730951
>>> 
>>> Polar2D (pi / 4) 2
Polar2D 0.7853981633974483 2.0
>>> 

--- The (==)  operator requires its arguments 
--- to have the same type
---
>>> Cartesian2D (sqrt 2) (sqrt 2) == Polar2D (pi / 4) 2

<interactive>:58:34:
    Couldn't match expected type ‘Cartesian2D’
                with actual type ‘Polar2D’
    In the second argument of ‘(==)’, namely ‘Polar2D (pi / 4) 2’
    In the expression:
      Cartesian2D (sqrt 2) (sqrt 2) == Polar2D (pi / 4) 2
>>>
```

### ShapeUnion.hs<a id="sec-1-2-4" name="sec-1-2-4"></a>

-   <rwh/ShapeUnion.hs>

```haskell
type Vector = (Double, Double)

data = Shape = Circle Vector Double 
              | Poly [Vector]
```

### add.hs<a id="sec-1-2-5" name="sec-1-2-5"></a>

```haskell
sumList (x:xs) = x + sumList xs 
sumList []     = 0
```

Loading 

```haskell
>>> :load "rwh/ch03/add.hs"
[1 of 1] Compiling Main             ( rwh/ch03/add.hs, interpreted )
Ok, modules loaded: Main.
>>>

>>> sumList []
0
>>> sumList [1, 2, 3, 4, 5, 6]
21
>>>
```

### BookStore3.hs<a id="sec-1-2-6" name="sec-1-2-6"></a>

-   <rwh/ch03/BookStore3.hs>

```haskell
data BookInfo = Book Int String [String]
                deriving (Show)



data MagazineInfo = Magazine Int String [String]
                    deriving (Show)


type CustomerID = Int        --- Type synonym 
type ReviewBody = String 

data BookReview = BookReview BookInfo CustomerID String 

data BetterReviw = BetterReviw BookInfo CustomerID ReviewBody 

type BookRecord = (BookInfo, BookReview) 

bookID      (Book id title authors) = id
bookTitle   (Book id title authors) = title
bookAuthors (Book id title authors) = authors

myInfo = Book 9780135072455 "Algebra of Programming"
              ["Richard Bird", "Oege de Moor"]
```

Running:

```haskell
>>> :load "rwh/ch03/BookStore3.hs"
[1 of 1] Compiling Main             ( rwh/ch03/BookStore3.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> bookID (Book 3 "Probability Theory" ["E.T.H. Jaynes"])
3
>>> bookTitle (Book 3 "Probability Theory" ["E.T.H. Jaynes"])
"Probability Theory"
>>> bookAuthors (Book 3 "Probability Theory" ["E.T.H. Jaynes"])
["E.T.H. Jaynes"]

>>> :type bookID
bookID :: BookInfo -> Int
>>> :type bookTitle
bookTitle :: BookInfo -> String
>>> :type bookAuthors
bookAuthors :: BookInfo -> [String]
>>>
```

### BookStore4.hs<a id="sec-1-2-7" name="sec-1-2-7"></a>

-   <rwh/ch03/BookStore4.hs>

```haskell
data BookInfo = Book Int String [String]
                deriving (Show)



data MagazineInfo = Magazine Int String [String]
                    deriving (Show)


type CustomerID = Int        --- Type synonym 
type ReviewBody = String 
type Address = [String] 

data BookReview = BookReview BookInfo CustomerID String 

data BetterReviw = BetterReviw BookInfo CustomerID ReviewBody 

type BookRecord = (BookInfo, BookReview) 

bookID      (Book id title authors) = id
bookTitle   (Book id title authors) = title
bookAuthors (Book id title authors) = authors


data Customer = Customer {
      customerID      :: CustomerID
    , customerName    :: String
    , customerAddress :: Address
    } deriving (Show)

myInfo = Book 9780135072455 "Algebra of Programming"
              ["Richard Bird", "Oege de Moor"]

customer1 = Customer 271828 "J.R. Hacker"
            ["255 Syntax Ct",
             "Milpitas, CA 95134",
             "USA"]

customer2 = Customer {
              customerID = 271828
            , customerAddress = ["1048576 Disk Drive",
                                 "Milpitas, CA 95134",
                                 "USA"]
            , customerName = "Jane Q. Citizen"
            }
```

Running:

```haskell
>>> :load "rwh/ch03/BookStore4.hs"
[1 of 1] Compiling Main             ( rwh/ch03/BookStore4.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> customer1
Customer {customerID = 271828, customerName = "J.R. Hacker", customerAddress = ["255 Syntax Ct","Milpitas, CA 95134","USA"]}
>>> 

>>> customer2
Customer {customerID = 271828, customerName = "Jane Q. Citizen", customerAddress = ["1048576 Disk Drive","Milpitas, CA 95134","USA"]}
>>> 

>>> customerID customer1
271828
>>> customerID customer2
271828
>>> customerName customer1
"J.R. Hacker"
>>> customerAddress customer1
["255 Syntax Ct","Milpitas, CA 95134","USA"]
>>> 

>>> :t customerName
customerName :: Customer -> String
>>>  :t customerAddress
customerAddress :: Customer -> Address
>>> :t customerID
customerID :: Customer -> CustomerID
>>>
```

## Chapter 4  - Functional Programming<a id="sec-1-3" name="sec-1-3"></a>

### InteractWith.hs<a id="sec-1-3-1" name="sec-1-3-1"></a>

Page 71: <rwh/ch04/InteractWith.hs>

```haskell
import System.Environment (getArgs)

interactWith function inputFile outputFile = do
  input <- readFile inputFile
  writeFile outputFile (function input)

main = mainWith myFunction
  where mainWith function = do
          args <- getArgs
          case args of
            [input,output] -> interactWith function input output
            _ -> putStrLn "error: exactly two arguments needed"

        -- replace "id" with the name of our function below
        myFunction = id
```

Running:

```
$ ghc --make InteractWith 
[1 of 1] Compiling Main             ( InteractWith.hs, InteractWith.o )
Linking InteractWith ...

$ ./InteractWith /etc/issue 
error: exactly two arguments needed

$ ./InteractWith /etc/issue /tmp/issue.out

$ cat /tmp/issue.out 
Arch Linux \r (\l)
```

### FixLines.hs<a id="sec-1-3-2" name="sec-1-3-2"></a>

Page 73: <rwh/ch04/FixLines.hs> 

```haskell
import System.Environment (getArgs)

interactWith function inputFile outputFile = do
  input <- readFile inputFile
  writeFile outputFile (function input)

splitLines [] = []
splitLines cs =
    let (pre, suf) = break isLineTerminator cs
    in  pre : case suf of 
                ('\r':'\n':rest) -> splitLines rest
                ('\r':rest)      -> splitLines rest
                ('\n':rest)      -> splitLines rest
                _                -> []

isLineTerminator c = c == '\r' || c == '\n'

fixLines :: String -> String
fixLines input = unlines (splitLines input)


main = mainWith myFunction
  where mainWith function = do
          args <- getArgs
          case args of
            [input,output] -> interactWith function input output
            _ -> putStrLn "error: exactly two arguments needed"

        -- replace "id" with the name of our function below
        myFunction = fixLines
```

Running Interactive:

```haskell
>>> :load "rwh/ch04/FixLines.hs"
[1 of 1] Compiling Main             ( rwh/ch04/FixLines.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> break odd [2, 4, 5, 6, 8]
([2,4],[5,6,8])

>>> splitLines "line1\nline2\r\nline3\r\n"
["line1","line2","line3"]
>>>
```

Running in batch mode:

```sh
$ curl -O http://www.gnu.org/licenses/gpl-3.0.txt

$ file gpl-3.0.txt 
gpl-3.0.txt: ASCII text

#  dos2unix replacement 
#
$ awk 'sub("$", "\r")' gpl-3.0.txt  >  gpl-3.0.dos.txt

$ file gpl-3.0.dos.txt 
gpl-3.0.dos.txt: ASCII text, with CRLF line terminators

$ ghc --make FixLines
[1 of 1] Compiling Main             ( FixLines.hs, FixLines.o )
Linking FixLines ...

$ file FixLines
FixLines: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically 
linked, interpreter 
/nix/store/n2wxp513rr00f6hr2dy0waqahns49dch-glibc-2.21/lib/ld-linux-x86-64.so.2, 
for GNU/Linux 2.6.32, not stripped


$ ./FixLines 
error: exactly two arguments needed

$ ./FixLines gpl-3.0.dos.txt gpl-3.0.unix.txt

$ file gpl-3.0.unix.txt 
gpl-3.0.unix.txt: ASCII text
```

### ch4.exercises.hs<a id="sec-1-3-3" name="sec-1-3-3"></a>

1.  Write your own “safe” definitions of the standard partial list

functions, but make sure they never fail. As a hint, you might want to
consider using the following types:

```haskell
safeHead :: [a] -> Maybe a
safeTail :: [a] -> Maybe [a]
safeLast :: [a] -> Maybe a
safeInit :: [a] -> Maybe [a]
```

1.  Write a function splitWith  that acts similarly to words  but takes a predicate and a

list of any type, and then splits its input list on every element for which the predicate
returns False:

```haskell
-- file: ch04/ch04.exercises.hs
splitWith :: (a -> Bool) -> [a] -> [[a]]
```

Page 84: <rwh/ch04/ch04.exercises.hs> 

```haskell
safeHead :: [a] -> Maybe a
safeHead (head:rest) = Just head
safeHead []   = Nothing

safeTail :: [a] -> Maybe [a]
safeTail (head:rest) = Just rest 
safeTail []          = Nothing 


safeLast :: [a] -> Maybe a
safeLast [x] = Just x
safeLast (hd:tl) = safeLast tl 
safeLast []  = Nothing

safeInit :: [a] -> Maybe [a]
safeInit (x:xs)      = Just $ init (x:xs)
safeInit []          = Nothing 

{-
splitWith_aux :: (a -> Bool) -> [a] -> [[a]] -> [[a]]
splitWith_aux fnp []     acc  = [] 
splitWith_aux fnp (x:xs) acc  = if fnp x
                                then  splitWith_aux fnp xs  

-}
```

Running:

```haskell
>>> :load "rwh/ch04/ch04.exercises.hs"
[1 of 1] Compiling Main             ( rwh/ch04/ch04.exercises.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> safeHead [1, 2, 3, 4]
Just 1
>>> safeHead [1 ..]
Just 1
>>> safeHead []
Nothing
>>> 

>>> safeTail [1, 2, 3, 4]
Just [2,3,4]
>>> safeTail []
Nothing
>>> 

>>> safeLast [1, 2, 3, 4]
Just 4
>>> safeLast [4]
Just 4
>>> safeLast []
Nothing
>>> 

>>> safeInit []
Nothing
>>> safeInit [1, 2, 3, 4, 5]
Just [1,2,3,4]
>>> safeInit [1]
Just []
>>>
```

### IntParser.hs<a id="sec-1-3-4" name="sec-1-3-4"></a>

Page 86: <rwh/ch04/IntParse.hs> 

```haskell
import Data.Char (digitToInt)

asInt :: String -> Int 
asInt xs = loop 0 xs

loop :: Int -> String -> Int 
loop acc [] = acc
loop acc (x:xs) = let acc' = acc * 10 + digitToInt x
                  in loop acc' xs
```

Running:

```haskell
>>>:load rwh/ch04/IntParse.hs 
[1 of 1] Compiling Main             ( rwh/ch04/IntParse.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> digitToInt '9'
9
>>> digitToInt 'a'
10
>>> digitToInt 'x'
 *** Exception: Char.digitToInt: not a digit 'x'
>>> 

>>> asInt "33"
33
>>> asInt "100"
100
>>> 

>>> :t asInt
asInt :: String -> Int
>>> 

>>> asInt "not a number"
 *** Exception: Char.digitToInt: not a digit 'n'
>>>
```

## Chapter 5  - Writing a Library: Working with JSON Data<a id="sec-1-4" name="sec-1-4"></a>

### SimpleJson1.hs<a id="sec-1-4-1" name="sec-1-4-1"></a>

Page: 112 <rwh/ch05/SimpleJSON1.hs>

```haskell
data JValue = JString String
            | JNumber Double 
            | JBool Bool
            | JNull 
            | JObject [(String, JValue)]
            | JArray  [JValue]
            deriving (Eq, Ord, Show)

getString :: JValue -> Maybe String 
getString (JString s) = Just s
getString _           = Nothing

getBool :: JValue -> Maybe Bool 
getBool (JBool b) = Just b 
getBool _         = Nothing 

getNumber :: JValue -> Maybe Double
getNumber (JNumber n) = Just n
getNumber _           = Nothing

getObject :: JValue -> Maybe [(String, JValue)]
getObject js = case js of
               JObject xs -> Just xs
               _          -> Nothing 


getArray :: JValue -> Maybe [JValue]
getArray js = case js of
              JArray xs -> Just xs
              _         -> Nothing 

isNull :: JValue -> Bool 
isNull JNull = True
isNull _     = False
```

Running:

```haskell
>>> :load "rwh/ch05/SimpleJSON1.hs"
[1 of 1] Compiling Main             ( rwh/ch05/SimpleJSON1.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> JString "New Jersey"
JString "New Jersey"
>>> JNumber 3.1415
JNumber 3.1415
>>> JBool False
JBool False
>>> JBool True
JBool True
>>> 

>>> getString (JNumber 3)
Nothing
>>> getString (JString "Utah" )
Just "Utah"
>>> 

>>> getNumber (JNumber 10.2323)
Just 10.2323
>>> getNumber (JString "Texas")
Nothing
>>> 

>>> getBool (JString "Alabama")
Nothing
>>> getBool (JBool False)
Just False
>>> 

>>> getArray (JArray [JString "Alabama", JNumber 102.23, JBool True])
Just [JString "Alabama",JNumber 102.23,JBool True]
>>> 

>>> getObject (JObject [("Alabama", JNumber 0), ("Texas", JNumber 2), ("Nevada", JNumber 20), ("New York", JNumber 40)])
Just [("Alabama",JNumber 0.0),("Texas",JNumber 2.0),("Nevada",JNumber 20.0),("New York",JNumber 40.0)]
>>> 

>>> isNull JNull
True
>>> isNull (JString "California")
False
>>>
```

### SimpleJSon2.hs<a id="sec-1-4-2" name="sec-1-4-2"></a>

Page: 112 - File: <rwh/ch05/SimpleJSON2.hs>

```haskell
module SimpleJSON2
       (
         JValue (..)
         , getString
         , getInt
         , getDouble
         , getObject
         , getArray
         , isNull
       ) where 

data JValue = JString String
            | JNumber Double 
            | JBool Bool
            | JNull 
            | JObject [(String, JValue)]
            | JArray  [JValue]
            deriving (Eq, Ord, Show)

getString :: JValue -> Maybe String 
getString (JString s) = Just s
getString _           = Nothing

getBool :: JValue -> Maybe Bool 
getBool (JBool b) = Just b 
getBool _         = Nothing 

getNumber :: JValue -> Maybe Double
getNumber (JNumber n) = Just n
getNumber _           = Nothing

getDouble = getNumber 

getObject :: JValue -> Maybe [(String, JValue)]
getObject js = case js of
               JObject xs -> Just xs
               _          -> Nothing 


getArray :: JValue -> Maybe [JValue]
getArray js = case js of
              JArray xs -> Just xs
              _         -> Nothing 

getInt (JNumber n) = Just (truncate n)
getInt _           = Nothing 

isNull :: JValue -> Bool 
isNull JNull = True
```

File: <rwh/ch05/Main.hs>

```haskell
module Main (main) where

import SimpleJSON2

main = print (JObject [("foo", JNumber 1), ("bar", JBool False)])
```

Compile the module:

```sh
$ alias ghc=/nix/store/fcwp5nswfq4wm4hc3c9ij8rap9dr9p3q-ghc-7.10.2/bin/ghc

# Generate only object code
#
$ ghc -c SimpleJSON2.hs


$ file SimpleJSON2.o 
SimpleJSON2.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped

$ file SimpleJSON2.hi
SimpleJSON2.hi: data


# Error:
#
$ ghc -o simple Main.hs SimpleJSON2.o
Linking simple ...
SimpleJSON2.o:(.data+0x0): multiple definition of `__stginit_SimpleJSON2'
./SimpleJSON2.o:(.data+0x0): first defined here
SimpleJSON2.o:(.data+0x0): multiple definition of `SimpleJSON2_isNull_closure'
./SimpleJSON2.o:(.data+0x0): first defined here
...
SimpleJSON2.o: In function `SimpleJSON2_JArray_info':
(.text+0x24f0): multiple definition of `SimpleJSON2_JArray_static_info'
./SimpleJSON2.o:(.text+0x24f0): first defined here
collect2: error: ld returned 1 exit status


# Now it works 
#
$ ghc --make -o simple Main.hs
Linking simple ...

$ file simple 
simple: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, 
interpreter /nix/store/n2wxp513rr00f6hr2dy0waqahns49dch-glibc-2.21/lib/ld-linux-x86-64.so.2, 
for GNU/Linux 2.6.32, not stripped

$ ./simple 
JObject [("foo",JNumber 1.0),("bar",JBool False)]
```

### PutJSON.hs<a id="sec-1-4-3" name="sec-1-4-3"></a>

Page: 116 <rwh/ch05/PutJson.hs>

```haskell
module PutJSON where

import Data.List (intercalate)
import SimpleJSON2

renderJValue :: JValue -> String
renderJValue (JString s)   = show s
renderJValue (JNumber n)   = show n
renderJValue (JBool True)  = "true"
renderJValue (JBool False) = "false"
renderJValue JNull         = "null"

renderJValue (JObject o) = "{" ++ pairs o ++ "}"
  where pairs [] = ""
        pairs ps = intercalate ", " (map renderPair ps)
        renderPair (k,v)   = show k ++ ": " ++ renderJValue v

renderJValue (JArray a) = "[" ++ values a ++ "]"
  where values [] = ""
        values vs = intercalate ", " (map renderJValue vs)

-- Good Haskell style involves separating pure code from code that
-- performs I/O. (Real World Haskell) 
--
putJValue :: JValue -> IO ()
putJValue v = putStrLn (renderJValue v)
```

Running:

```haskell
```

### PrettyJSON.hs<a id="sec-1-4-4" name="sec-1-4-4"></a>

Page: 119 <rwh/ch05/PrettyJSON.hs> 

```haskell
renderJValue :: JValue -> Doc
renderJValue (JBool True)  = text "true"
renderJValue (JBool False) = text "false"
renderJValue JNull         = text "null"
renderJValue (JNumber num) = double num
renderJValue (JString str) = string str
```

## Chapter 8  - Efficient File Processing, Regular Expressions, and Filename Matching<a id="sec-1-5" name="sec-1-5"></a>

### ElfMagic.hs<a id="sec-1-5-1" name="sec-1-5-1"></a>

Page 194. <rwh/ch08/ElfMagic.hs>  

This function tests if the file is a Unix ELF executable that are
recognized by its magic number. 

```haskell
import qualified Data.ByteString.Lazy as L

hasElfMagic :: L.ByteString -> Bool
hasElfMagic content = L.take 4 content == elfMagic
    where elfMagic = L.pack [0x7f, 0x45, 0x4c, 0x46]

isElfFile :: FilePath -> IO Bool
isElfFile path = do
  content <- L.readFile path
  return (hasElfMagic content)
```

Running 

```haskell
GHCi, version 7.10.2: http://www.haskell.org/ghc/  :? for help
>>> 

>>> :load "ch08/ElfMagic.hs"
[1 of 1] Compiling Main             ( ch08/ElfMagic.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> :t hasElfMagic 
hasElfMagic :: L.ByteString -> Bool
>>> 
>>> 


>>> file <- L.readFile "/bin/sh"
>>> :t file
file :: L.ByteString
>>>
>>> hasElfMagic file
True

>>> L.take 10 file
"\DELELF\STX\SOH\SOH\NUL\NUL\NUL"
>>> 


>>> file <- L.readFile "/etc/issue"
>>> file
"Arch Linux \\r (\\l)\n\n"
>>> hasElfMagic file
False
>>> 

>>> import qualified System.Directory as SD


>>> :t isElfFile 
isElfFile :: FilePath -> IO Bool
>>> 
>>> isElfFile "/bin/sh"
True
>>> isElfFile "/etc/fstab"
False
>>> 

>>> SD.setCurrentDirectory "/bin"
>>> SD.getCurrentDirectory 
"/usr/bin"
>>> 

>>> files <- SD.getDirectoryContents "/bin"
>>> :t files
files :: [FilePath]
>>> 

>>> take 4 files
[".","..","install-info","update-desktop-database"]
>>> 

>>> let flist = drop 2 files
>>> take 4 flist
["install-info","update-desktop-database","libinput-list-devices","visudo"]
>>> 

>>> :t isElfFile 
isElfFile :: FilePath -> IO Bool
>>> 
>>> :t filter
filter :: (a -> Bool) -> [a] -> [a]
>>> 

>>> filter isElfFile flist

<interactive>:83:8:
    Couldn't match type ‘IO Bool’ with ‘Bool’
    Expected type: FilePath -> Bool
      Actual type: FilePath -> IO Bool
    In the first argument of ‘filter’, namely ‘isElfFile’
    In the expression: filter isElfFile flist
>>> 

>>> import Control.Monad (filterM)
>>> :t filterM
filterM :: Monad m => (a -> m Bool) -> [a] -> m [a]
>>> 

>>> filesOnly <- filterM SD.doesFileExist flist
>>> 

>>> take 4 filesOnly 
["install-info","update-desktop-database","libinput-list-devices","visudo"]
>>> 


>>> filterM isElfFile  (take 30 filesOnly ) >>= mapM_ putStrLn 
install-info
update-desktop-database
libinput-list-devices
visudo
suexec
jack_wait
dirname
j2k_dump
json-glib-format
runlevel
chacl
eu-addr2line
c++
git
gcov
ionice
lircd
...
```

### HighestClose.hs<a id="sec-1-5-2" name="sec-1-5-2"></a>

Page 196. <rwh/ch08/HighestClose.hs> 

```haskell
import qualified Data.ByteString.Lazy.Char8 as L

closing = readPrice . (!!4) . L.split ','

readPrice :: L.ByteString -> Maybe Int
readPrice str =
    case L.readInt str of
      Nothing             -> Nothing
      Just (dollars,rest) ->
        case L.readInt (L.tail rest) of
          Nothing           -> Nothing
          Just (cents,more) ->
            Just (dollars * 100 + cents)


highestClose = maximum . (Nothing:) . map closing . L.lines

highestCloseFrom path = do
    contents <- L.readFile path
    print (highestClose contents)
```

File: <rwh/ch08/prices.csv> 

```csv
Date,Open,High,Low,Close,Volume,Adj Close
2008-08-01,20.09,20.12,19.53,19.80,19777000,19.80
2008-06-30,21.12,21.20,20.60,20.66,17173500,20.66
2008-05-30,27.07,27.10,26.63,26.76,17754100,26.76
2008-04-30,27.17,27.78,26.76,27.41,30597400,27.41
```

Running:

```haskell
>>> :load "rwh/ch08/HighestClose.hs" 
[1 of 1] Compiling Main             ( rwh/ch08/HighestClose.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> highestCloseFrom "rwh/ch08/prices.csv"
Just 2741
>>> 

>>> contents <- L.readFile "rwh/ch08/prices.csv" 

>>> :t contents
contents :: L.ByteString
>>> 

-- The output was formatted manually to fit in the 
-- screen. 
--
>>> tail $ L.lines contents 
["2008-08-01,20.09,20.12,19.53,19.80,19777000,19.80",
"2008-06-30,21.12,21.20,20.60,20.66,17173500,20.66",
"2008-05-30,27.07,27.10,26.63,26.76,17754100,26.76",
"2008-04-30,27.17,27.78,26.76,27.41,30597400,27.41"]
>>> 

>>> mapM_ (\ x -> putStrLn (show x)) $ tail $ L.lines contents 
"2008-08-01,20.09,20.12,19.53,19.80,19777000,19.80"
"2008-06-30,21.12,21.20,20.60,20.66,17173500,20.66"
"2008-05-30,27.07,27.10,26.63,26.76,17754100,26.76"
"2008-04-30,27.17,27.78,26.76,27.41,30597400,27.41"
>>> 

>>> mapM_ (\ x -> putStrLn (show x)) $ map (L.split ',')  $ tail $ L.lines contents 
["2008-08-01","20.09","20.12","19.53","19.80","19777000","19.80"]
["2008-06-30","21.12","21.20","20.60","20.66","17173500","20.66"]
["2008-05-30","27.07","27.10","26.63","26.76","17754100","26.76"]
["2008-04-30","27.17","27.78","26.76","27.41","30597400","27.41"]
>>> 

-- Adj close 
--
>>> map (!!4) $ map (L.split ',')  $ tail $ L.lines contents 
["19.80","20.66","26.76","27.41"]
>>> 

-- Optmization with function composition 
-- 
>>> map ( (!!4) . L.split ',')  $ tail $ L.lines contents 
["19.80","20.66","26.76","27.41"]
>>> 

>>> map ( (!!4) . L.split ',')  . tail . L.lines $ contents 
["19.80","20.66","26.76","27.41"]
>>> 

>>> let readLByteStringMaybe  = readMaybe . L.unpack 
>>> :t readLByteStringMaybe
readLByteStringMaybe :: Read a => L.ByteString -> Maybe a
>>> 

>>> map (\x -> readLByteStringMaybe x :: Maybe Double) $ map ( (!!4) . L.split ',')  . tail . L.lines $ contents 
[Just 19.8,Just 20.66,Just 26.76,Just 27.41]
>>> 

>>> map ((\x -> readLByteStringMaybe x :: Maybe Double) . (!!4) . L.split ',')  . tail . L.lines $ contents 
[Just 19.8,Just 20.66,Just 26.76,Just 27.41]
>>> 

-- If any number fail to be parsed the whole column will fail, 
-- it will return Nothing 
--
>>> sequence $ map ((\x -> readLByteStringMaybe x :: Maybe Double) . (!!4) . L.split ',')  . tail . L.lines $ contents 
Just [19.8,20.66,26.76,27.41]
>>> 


>>> fmap sum $ sequence $ map ((\x -> readLByteStringMaybe x :: Maybe Double) . (!!4) . L.split ',')  . tail . L.lines $ contents 
Just 94.63
>>> 

>>> let parseColumnDouble n =  sequence . map ((\x -> readLByteStringMaybe x :: Maybe Double) . (!!n) . L.split ',')  . tail . L.lines 
>>> 

>>> :t parseColumnDouble
parseColumnDouble :: Int -> L.ByteString -> Maybe [Double]
>>> 

--- Multiline function composition 
--- 
--- 
---
:set +m  -- Allow multi line paste in the repl.

:{
let parseColumnDouble n =
      sequence
      . map ((\x -> readLByteStringMaybe x :: Maybe Double)
             . (!!n)
             . L.split ','
            )
             . tail
             . L.lines            
:}


>>> parseColumnDouble 0 contents 
Nothing
>>> parseColumnDouble 1 contents 
Just [20.09,21.12,27.07,27.17]
>>> parseColumnDouble 2 contents 
Just [20.12,21.2,27.1,27.78]
>>> parseColumnDouble 3 contents 
Just [19.53,20.6,26.63,26.76]
>>> parseColumnDouble 4 contents 
Just [19.8,20.66,26.76,27.41]

-- Year 
--
>>> map (!!0) $ map (L.split ',')  $ tail $ L.lines contents 
["2008-08-01","2008-06-30","2008-05-30","2008-04-30"]
>>> 


>>>
```

## Chapter 10 - Code Case Study: Parsing a Binary Data Format<a id="sec-1-6" name="sec-1-6"></a>

### PNM.hs<a id="sec-1-6-1" name="sec-1-6-1"></a>

Page 246 - File: <rwh/ch10/PNM.hs> 

```haskell
import qualified Data.ByteString.Lazy.Char8 as L8
import qualified Data.ByteString.Lazy as L 
import Data.Char (isSpace)

data Greymap = Greymap {
      greyWidth  :: Int
    , greyHeight :: Int
    , greyMax    :: Int
    , greyData   :: L.ByteString
    } deriving (Eq)

instance Show Greymap where
    show (Greymap w h m _) = "Greymap " ++ show w ++ "x" ++ show h ++
                             " " ++ show m

parseP5 :: L.ByteString -> Maybe (Greymap, L.ByteString)
parseP5 s =
  case matchHeader (L8.pack "P5") s of
    Nothing -> Nothing
    Just s1 ->
      case getNat s1 of
        Nothing -> Nothing
        Just (width, s2) ->
          case getNat (L8.dropWhile isSpace s2) of
            Nothing -> Nothing
            Just (height, s3) ->
              case getNat (L8.dropWhile isSpace s3) of
                Nothing -> Nothing
                Just (maxGrey, s4)
                  | maxGrey > 255 -> Nothing
                  | otherwise ->
                      case getBytes 1 s4 of
                        Nothing -> Nothing
                        Just (_, s5) ->
                          case getBytes (width * height) s5 of
                            Nothing -> Nothing
                            Just (bitmap, s6) ->
                              Just (Greymap width height maxGrey bitmap, s6)


matchHeader :: L.ByteString -> L.ByteString -> Maybe L.ByteString
matchHeader prefix str
    | prefix `L8.isPrefixOf` str
        = Just (L8.dropWhile isSpace (L.drop (L.length prefix) str))
    | otherwise
        = Nothing


getNat :: L.ByteString -> Maybe (Int, L.ByteString)
getNat s = case L8.readInt s of
             Nothing -> Nothing
             Just (num,rest)
                 | num <= 0    -> Nothing
                 | otherwise -> Just (fromIntegral num, rest)

getBytes :: Int -> L.ByteString -> Maybe (L.ByteString, L.ByteString)
getBytes n str = let count           = fromIntegral n
                     both@(prefix,_) = L.splitAt count str
                 in if L.length prefix < count
                    then Nothing
                    else Just both
```

Running:

```haskell
>>> :load "rwh/ch10/PNM.hs"
[1 of 1] Compiling Main             ( rwh/ch10/PNM.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> content <- L.readFile "rwh/ch10/bird.pgm"
>>> 
>>> L.take 30 content
"P5\n321 481\n255\n4`oxyrxvuuuuuuu"
>>> 
>>> parseP5 content
Just (Greymap 321x481 255,"")
>>>
```

Test files:

-   <rwh/ch10/bird.pgm>

-   rwh/ch10/bird.png

![img](rwh/ch10/bird.png) 

### Parse.hs<a id="sec-1-6-2" name="sec-1-6-2"></a>

Page 240; 250 - File: <rwh/ch10/Parse.hs>

```haskell
import qualified Data.ByteString.Lazy.Char8 as L8
import qualified Data.ByteString.Lazy as L 
import Data.Word (Word8)
import Data.Int (Int64)
import Data.Char (chr, isDigit, isSpace)
import Control.Applicative ((<$>))

data ParseState = ParseState {
      string :: L.ByteString
    , offset :: Int64           -- imported from Data.Int
    } deriving (Show)

simpleParse :: ParseState -> (a, ParseState)
simpleParse = undefined

betterParse :: ParseState -> Either String (a, ParseState)
betterParse = undefined

newtype Parse a = Parse {
      runParse :: ParseState -> Either String (a, ParseState)
    }

identity :: a -> Parse a
identity a = Parse (\s -> Right (a, s))


parse :: Parse a -> L.ByteString -> Either String a
parse parser initState
    = case runParse parser (ParseState initState 0) of
        Left err          -> Left err
        Right (result, _) -> Right result

modifyOffset :: ParseState -> Int64 -> ParseState
modifyOffset initState newOffset =
    initState { offset = newOffset }

getState :: Parse ParseState
getState = Parse (\s -> Right (s, s))

putState :: ParseState -> Parse ()
putState s = Parse (\_ -> Right ((), s))

bail :: String -> Parse a
bail err = Parse $ \s -> Left $
           "byte offset " ++ show (offset s) ++ ": " ++ err

(==>) :: Parse a -> (a -> Parse b) -> Parse b
firstParser ==> secondParser  =  Parse chainedParser
  where chainedParser initState   =
          case runParse firstParser initState of
            Left errMessage ->
                Left errMessage
            Right (firstResult, newState) ->
                runParse (secondParser firstResult) newState


parseByte :: Parse Word8
parseByte =
    getState ==> \initState ->
    case L.uncons (string initState) of
      Nothing ->
          bail "no more input"
      Just (byte,remainder) ->
          putState newState ==> \_ ->
          identity byte
        where newState = initState { string = remainder,
                                     offset = newOffset }
              newOffset = offset initState + 1

instance Functor Parse where
    fmap f parser = parser ==> \result ->
                    identity (f result)

w2c :: Word8 -> Char
w2c = chr . fromIntegral

parseChar :: Parse Char
parseChar = w2c <$> parseByte

peekByte :: Parse (Maybe Word8)
peekByte = (fmap fst . L.uncons . string) <$> getState

peekChar :: Parse (Maybe Char)
peekChar = fmap w2c <$> peekByte


parseWhile :: (Word8 -> Bool) -> Parse [Word8]
parseWhile p = (fmap p <$> peekByte) ==> \mp ->
               if mp == Just True
               then parseByte ==> \b ->
                    (b:) <$> parseWhile p
               else identity []

parseWhileVerbose p =
    peekByte ==> \mc ->
    case mc of
      Nothing -> identity []
      Just c | p c ->
                 parseByte ==> \b ->
                 parseWhileVerbose p ==> \bs ->
                 identity (b:bs)
             | otherwise ->
                 identity []

parseWhileWith :: (Word8 -> a) -> (a -> Bool) -> Parse [a]
parseWhileWith f p = fmap f <$> parseWhile (p . f)

parseNat :: Parse Int
parseNat = parseWhileWith w2c isDigit ==> \digits ->
           if null digits
           then bail "no more input"
           else let n = read digits
                in if n < 0
                   then bail "integer overflow"
                   else identity n

(==>&) :: Parse a -> Parse b -> Parse b
p ==>& f = p ==> \_ -> f

skipSpaces :: Parse ()
skipSpaces = parseWhileWith w2c isSpace ==>& identity ()

assert :: Bool -> String -> Parse ()
assert True  _   = identity ()
assert False err = bail err

parseBytes :: Int -> Parse L.ByteString
parseBytes n =
    getState ==> \st ->
    let n' = fromIntegral n
        (h, t) = L.splitAt n' (string st)
        st' = st { offset = offset st + L.length h, string = t }
    in putState st' ==>&
       assert (L.length h == n') "end of input" ==>&
       identity h
```

Running:

```haskell
>>> :load "rwh/ch10/Parse.hs"
[1 of 1] Compiling Main             ( rwh/ch10/Parse.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> :t identity
identity :: a -> Parse a
>>> 
>>> :info Parse
newtype Parse a
  = Parse {runParse :: ParseState -> Either String (a, ParseState)}
  	-- Defined at rwh/ch10/Parse.hs:18:1
>>> 
>>> 

>>> :type parse (identity 1) undefined
parse (identity 1) undefined :: Num a => Either String a
>>> parse (identity 1) undefined
Right 1
>>> 
>>> 
>>> parse (identity "foo") undefined
Right "foo"
>>> 

>>> let before = ParseState (L8.pack "foo") 0
>>> before
ParseState {string = "foo", offset = 0}
>>> 

>>> let after = modifyOffset before 3
>>> after
ParseState {string = "foo", offset = 3}
>>>

>>> :t L8.uncons
L8.uncons :: L.ByteString -> Maybe (Char, L.ByteString)
>>> 
>>> :t L8.pack
L8.pack :: [Char] -> L.ByteString
>>> 
>>> L8.pack "foo"
"foo"
>>> :t L8.pack "foo"
L8.pack "foo" :: L.ByteString
>>> 
>>> L8.uncons $ L8.pack "foo"
Just ('f',"oo")
>>> L8.uncons $ L8.empty
Nothing
>>> 

>>> :t runParse
runParse :: Parse a -> ParseState -> Either String (a, ParseState)
>>> 

>>> :t parse
parse :: Parse a -> L.ByteString -> Either String a

-- 0xff = 255
--
>>> parse parseByte (L8.pack "\xff")
Right 255
>>> 

>>> parse parseByte (L8.pack "\xa")
Right 10
>>> parse parseByte (L8.pack "\xb")
Right 11
>>> parse parseByte (L8.pack "")
Left "byte offset 0: no more input"
>>> 

-- 0xfa = 16 * 15 + 10  = 250 decimal 
--
--
>>> runParse parseByte  $ ParseState (L8.pack "\xfa") 0
Right (250,ParseState {string = "", offset = 1})
>>> 

>>> runParse parseByte  $ ParseState (L8.pack "x9023") 1
Right (120,ParseState {string = "9023", offset = 2})
>>> 

>>> runParse parseByte  $ ParseState (L8.pack "") 1
Left "byte offset 1: no more input"
>>> 

>>> let input = L8.pack "foo"

>>> :t input
input :: L.ByteString
>>> 
>>> L.head input
102
>>> parse parseByte input
Right 102
>>> 
>>> parse (id <$> parseByte) input
Right 102
>>> parse ((*2) <$> parseByte) input
Right 204 
>>> 

>>> parse parseNat (L8.pack "10023 asdb")
Right 10023
>>> parse parseNat (L8.pack "sad10023 asdb")
Left "byte offset 0: no more input"
>>> 

>>> parse parseChar (L8.pack "23")
Right '2'
>>> parse parseChar (L8.pack "")
Left "byte offset 0: no more input"
>>>
```

### TreeMap.sh<a id="sec-1-6-3" name="sec-1-6-3"></a>

Page: 244 - File: <rwh/ch10/TreeMap.hs>

```haskell
data Tree a =  Node (Tree a) (Tree a)
              | Leaf a 
              deriving (Show)

treeLengths (Leaf s) = Leaf (length s)
treeLengths (Node l r) = Node (treeLengths l) (treeLengths r)

treeMap :: (a -> b) -> Tree a -> Tree b
treeMap f (Leaf a)   = Leaf (f a)
treeMap f (Node l r) = Node (treeMap f l) (treeMap f r)

{-
class Functor f where
    fmap :: (a -> b) -> f a -> f b
-}
instance Functor Tree where
    fmap = treeMap

>>> fmap length (Node (Leaf "North Carolina") (Leaf "Puerto Rico"))
Node (Leaf 14) (Leaf 11)
>>> 

>>> fmap id (Node (Leaf "a") (Leaf "b"))
Node (Leaf "a") (Leaf "b")
>>>
```

Running: 

```haskell
>>> :load "rwh/ch10/TreeMap.hs"
[1 of 1] Compiling Main             ( rwh/ch10/TreeMap.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> :info Tree
data Tree a = Node (Tree a) (Tree a) | Leaf a
  	-- Defined at rwh/ch10/TreeMap.hs:2:1
instance Show a => Show (Tree a)
  -- Defined at rwh/ch10/TreeMap.hs:4:25
>>> 

>>> let tree = Node (Leaf "foo") (Node (Leaf "x") (Leaf "quux"))
>>> :t tree
tree :: Tree [Char]
>>> 
>>> treeLengths tree
Node (Leaf 3) (Node (Leaf 1) (Leaf 4))
>>> 
>>> treeMap (odd . length) tree
Node (Leaf True) (Node (Leaf True) (Leaf False))
>>>
```

## Chapter 13 - Data Structures<a id="sec-1-7" name="sec-1-7"></a>

### passwd-al.hs<a id="sec-1-7-1" name="sec-1-7-1"></a>

Page 300 - File: <rwh/ch13/passwd-al.hs>

```haskell
-- file: ch13/passwd-al.hs
import Data.List
import System.IO
import Control.Monad(when)
import System.Exit
import System.Environment(getArgs)

main = do

    -- Load the command-line arguments
    args <- getArgs

    -- If we don't have the right amount of args, give an error and abort
    when (length args /= 2) $ do
        putStrLn "Syntax: passwd-al filename uid"
        exitFailure

    -- Read the file lazily
    content <- readFile (args !! 0)

    -- Compute the username in pure code
    let username = findByUID content (read (args !! 1))

    -- Display the result
    case username of 
         Just x -> putStrLn x
         Nothing -> putStrLn "Could not find that UID"

-- Given the entire input and a UID, see if we can find a username.
findByUID :: String -> Integer -> Maybe String
findByUID content uid =
    let al = map parseline . lines $ content
        in lookup uid al

-- Convert a colon-separated line into fields
parseline :: String -> (Integer, String)
parseline input =
    let fields = split ':' input
        in (read (fields !! 2), fields !! 0)

{- | Takes a delimiter and a list. Break up the list based on the
-  delimiter. -}
split :: Eq a => a -> [a] -> [[a]]

-- If the input is empty, the result is a list of empty lists.
split _ [] = [[]]
split delim str =
    let -- Find the part of the list before delim and put it in "before".
        -- The rest of the list, including the leading delim, goes
        -- in "remainder".
        (before, remainder) = span (/= delim) str
        in
        before : case remainder of
                      [] -> []
                      x -> -- If there is more data to process,
                           -- call split recursively to process it
                           split delim (tail x)
```

File: <file:///rwh/ch13/passwd.txt> 

```txt
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
jgoerzen:x:1000:1000:John Goerzen,,,:/home/jgoerzen:/bin/bash
```

Running:

```
$ runhaskell ch13/passwd-al.hs
Syntax: passwd-al filename uid

$ runhaskell ch13/passwd-al.hs  /etc/passwd 0
root

$ runhaskell ch13/passwd-al.hs  /etc/passwd 1000
archmaster

$ runhaskell ch13/passwd-al.hs  /etc/passwd 81
dbus

$ runhaskell ch13/passwd-al.hs  ch13/passwd.txt 0
passwd-al.hs: Prelude.!!: index too large

$ runhaskell ch13/passwd-al.hs  ch13/passwd.txt 12
passwd-al.hs: Prelude.!!: index too large
```

### buildmap.hs<a id="sec-1-7-2" name="sec-1-7-2"></a>

Page 302 - File: <rwh/ch13/buildmap.hs> 

```haskell
-- file: ch13/buildmap.hs
import qualified Data.Map as Map

-- Functions to generate a Map that represents an association list
-- as a map

al = [(1, "one"), (2, "two"), (3, "three"), (4, "four")]

{- | Create a map representation of 'al' by converting the association
-  list using Map.fromList -}
mapFromAL =
    Map.fromList al

{- | Create a map representation of 'al' by doing a fold -}
mapFold =
    foldl (\map (k, v) -> Map.insert k v map) Map.empty al

{- | Manually create a map with the elements of 'al' in it -}
mapManual =
    Map.insert 2 "two" . 
    Map.insert 4 "four" .
    Map.insert 1 "one" .
    Map.insert 3 "three" $ Map.empty
```

Running:

```haskell
$ ghci
GHCi, version 7.10.2: http://www.haskell.org/ghc/  :? for help
>>> 
>>> :load ch13/buildmap.hs 
[1 of 1] Compiling Main             ( ch13/buildmap.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> al
[(1,"one"),(2,"two"),(3,"three"),(4,"four")]
>>> 
>>> mapFromAL
fromList [(1,"one"),(2,"two"),(3,"three"),(4,"four")]
>>> 
>>> mapFold
fromList [(1,"one"),(2,"two"),(3,"three"),(4,"four")]
>>> 
>>> mapManual
fromList [(1,"one"),(2,"two"),(3,"three"),(4,"four")]
>>>
```

### funcrecs.hs<a id="sec-1-7-3" name="sec-1-7-3"></a>

Function as data. 

Page 303 - File: <rwh/ch13/funcrecs.hs> 

```haskell
{- | Our usual CustomColor type to play with -}
data CustomColor =
  CustomColor {red :: Int,
               green :: Int,
               blue :: Int}
  deriving (Eq, Show, Read)

{- | A new type that stores a name and a function.
The function takes an Int, applies some computation to it, and returns
an Int along with a CustomColor -}
data FuncRec =
    FuncRec {name :: String,
             colorCalc :: Int -> (CustomColor, Int)}


plus5func color x = (color, x + 5)
purple = CustomColor 255 0 255
plus5 = FuncRec {name = "plus5", colorCalc = plus5func purple}
always0 = FuncRec {name = "always0", colorCalc = \_ -> (purple, 0)}
```

Running:

```haskell
>>> :load ch13/funcrecs.hs 
[1 of 1] Compiling Main             ( ch13/funcrecs.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> :info CustomColor
data CustomColor
  = CustomColor {red :: Int, green :: Int, blue :: Int}
  	-- Defined at ch13/funcrecs.hs:3:1
instance Eq CustomColor -- Defined at ch13/funcrecs.hs:7:13
instance Read CustomColor -- Defined at ch13/funcrecs.hs:7:23
instance Show CustomColor -- Defined at ch13/funcrecs.hs:7:17
>>> 
>>> :info FuncRec
data FuncRec
  = FuncRec {name :: String, colorCalc :: Int -> (CustomColor, Int)}
  	-- Defined at ch13/funcrecs.hs:12:1
>>> 
>>> :t plus5
plus5 :: FuncRec
>>> 
>>> name plus5
"plus5"
>>> :t colorCalc plus5
colorCalc plus5 :: Int -> (CustomColor, Int)
>>> 
>>> (colorCalc plus5) 7
(CustomColor {red = 255, green = 0, blue = 255},12)
>>> 
>>> :t colorCalc always0
colorCalc always0 :: Int -> (CustomColor, Int)
>>> 
>>> (colorCalc always0) 7
(CustomColor {red = 255, green = 0, blue = 255},0)
>>>
```

### funcrecs2.hs<a id="sec-1-7-4" name="sec-1-7-4"></a>

Page 304 - File: <rwh/ch13/funcrecs2.hs> 

```haskell
-- file: ch13/funcrecs2.hs
data FuncRec =
    FuncRec {name :: String,
             calc :: Int -> Int,
             namedCalc :: Int -> (String, Int)}

mkFuncRec :: String -> (Int -> Int) -> FuncRec
mkFuncRec name calcfunc =
    FuncRec {name = name,
             calc = calcfunc,
             namedCalc = \x -> (name, calcfunc x)}

plus5 = mkFuncRec "plus5" (+ 5)
always0 = mkFuncRec "always0" (\_ -> 0)
```

Running:

```haskell
>>> :load ch13/funcrecs2.hs 
[1 of 1] Compiling Main             ( ch13/funcrecs2.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> :t plus5
plus5 :: FuncRec
>>> 
>>> name plus5
"plus5"
>>> 
>>> :t name
name :: FuncRec -> String
>>>
>>> 
>>> :t namedCalc
namedCalc :: FuncRec -> Int -> (String, Int)
>>> 
>>> :t calc
calc :: FuncRec -> Int -> Int
>>> 
>>> (calc plus5) 5
10
>>> let plus5a = plus5 {name = "PLUS5A"}
 *Main System.Directory System.Process| 
>>> 
>>> :t plus5a
plus5a :: FuncRec
>>> 
>>> (namedCalc plus5a) 5
("plus5",10)
>>>
```

### passwdmap.hs<a id="sec-1-7-5" name="sec-1-7-5"></a>

Page: 305 - File: <rwh/ch13/passwdmap.hs>

```haskell
-- file: ch13/passwdmap.hs
import Data.List
import qualified Data.Map as Map
import System.IO
import Text.Printf(printf)
import System.Environment(getArgs)
import System.Exit
import Control.Monad(when)


{- | The primary piece of data this program will store.
   It represents the fields in a POSIX /etc/passwd file -}
data PasswdEntry = PasswdEntry {
    userName :: String,
    password :: String,
    uid :: Integer,
    gid :: Integer,
    gecos :: String,
    homeDir :: String,
    shell :: String}
    deriving (Eq, Ord)

{- | Define how we get data to a 'PasswdEntry'. -}
instance Show PasswdEntry where
    show pe = printf "%s:%s:%d:%d:%s:%s:%s" 
                (userName pe) (password pe) (uid pe) (gid pe)
                (gecos pe) (homeDir pe) (shell pe)

{- | Converting data back out of a 'PasswdEntry'. -}
instance Read PasswdEntry where
    readsPrec _ value =
        case split ':' value of
             [f1, f2, f3, f4, f5, f6, f7] ->
                 -- Generate a 'PasswdEntry' the shorthand way:
                 -- using the positional fields.  We use 'read' to convert
                 -- the numeric fields to Integers.
                 [(PasswdEntry f1 f2 (read f3) (read f4) f5 f6 f7, [])]
             x -> error $ "Invalid number of fields in input: " ++ show x
        where 
        {- | Takes a delimiter and a list.  Break up the list based on the
        -  delimiter. -}
        split :: Eq a => a -> [a] -> [[a]]
        -- If the input is empty, the result is a list of empty lists.
        split _ [] = [[]]
        split delim str =
            let -- Find the part of the list before delim and put it in
                -- "before".  The rest of the list, including the leading 
                -- delim, goes in "remainder".
                (before, remainder) = span (/= delim) str
                in
                before : case remainder of
                              [] -> []
                              x -> -- If there is more data to process,
                                   -- call split recursively to process it
                                   split delim (tail x)


-- Convenience aliases; we'll have two maps: one from UID to entries
-- and the other from username to entries
type UIDMap = Map.Map Integer PasswdEntry
type UserMap = Map.Map String PasswdEntry

{- | Converts input data to maps.  Returns UID and User maps. -}
inputToMaps :: String -> (UIDMap, UserMap)
inputToMaps inp =
    (uidmap, usermap)
    where
    -- fromList converts a [(key, value)] list into a Map
    uidmap = Map.fromList . map (\pe -> (uid pe, pe)) $ entries
    usermap = Map.fromList . 
              map (\pe -> (userName pe, pe)) $ entries
    -- Convert the input String to [PasswdEntry]
    entries = map read (lines inp)


main = do
    -- Load the command-line arguments
    args <- getArgs
    -- If we don't have the right number of args,
    -- give an error and abort
    when (length args /= 1) $ do
        putStrLn "Syntax: passwdmap filename"
        exitFailure
    -- Read the file lazily
    content <- readFile (head args)
    let maps = inputToMaps content
    mainMenu maps

mainMenu maps@(uidmap, usermap) = do
    putStr optionText
    hFlush stdout
    sel <- getLine
    -- See what they want to do.  For every option except 4,
    -- return them to the main menu afterwards by calling
    -- mainMenu recursively
    case sel of
         "1" -> lookupUserName >> mainMenu maps
         "2" -> lookupUID >> mainMenu maps
         "3" -> displayFile >> mainMenu maps
         "4" -> return ()
         _ -> putStrLn "Invalid selection" >> mainMenu maps
    where 
    lookupUserName = do
        putStrLn "Username: "
        username <- getLine
        case Map.lookup username usermap of
             Nothing -> putStrLn "Not found."
             Just x -> print x
   
    lookupUID = do
        putStrLn "UID: "
        uidstring <- getLine
        case Map.lookup (read uidstring) uidmap of
             Nothing -> putStrLn "Not found."
             Just x -> print x

    displayFile = 
        putStr . unlines . map (show . snd) . Map.toList $ uidmap

    optionText = 
          "\npasswdmap options:\n\
           \\n\
           \1   Look up a user name\n\
           \2   Look up a UID\n\
           \3   Display entire file\n\
           \4   Quit\n\n\
           \Your selection: "
```

Running:

```
$ runhaskell ch13/passwdmap.hs 
Syntax: passwdmap filename

$ runhaskell ch13/passwdmap.hs /etc/passwd

passwdmap options:

1   Look up a user name
2   Look up a UID
3   Display entire file
4   Quit

Your selection: 1
Username: 
archmaster
archmaster:x:1000:1000::/home/archmaster:/bin/bash

passwdmap options:

1   Look up a user name
2   Look up a UID
3   Display entire file
4   Quit

Your selection: 2
UID: 
1000
archmaster:x:1000:1000::/home/archmaster:/bin/bash

passwdmap options:

1   Look up a user name
2   Look up a UID
3   Display entire file
4   Quit

Your selection: 3
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/usr/bin/nologin
daemon:x:2:2:daemon:/:/usr/bin/nologin
mail:x:8:12:mail:/var/spool/mail:/usr/bin/nologin
ftp:x:14:11:ftp:/srv/ftp:/usr/bin/nologin
http:x:33:33:http:/srv/http:/usr/bin/nologin
uuidd:x:68:68:uuidd:/:/usr/bin/nologin
dbus:x:81:81:dbus:/:/usr/bin/nologin
avahi:x:84:84:avahi:/:/bin/nologin
ntp:x:87:87:Network Time Protocol:/var/lib/ntp:/bin/false
nobody:x:99:99:nobody:/:/usr/bin/nologin
polkitd:x:102:102:Policy Kit Daemon:/:/usr/bin/nologin
colord:x:124:124::/var/lib/colord:/bin/false
systemd-journal-gateway:x:191:191:systemd-journal-gateway:/:/usr/bin/nologin
systemd-timesync:x:192:192:systemd-timesync:/:/usr/bin/nologin
systemd-network:x:193:193:systemd-network:/:/usr/bin/nologin
systemd-bus-proxy:x:194:194:systemd-bus-proxy:/:/usr/bin/nologin
systemd-resolve:x:195:195:systemd-resolve:/:/usr/bin/nologin
git:x:996:996:git daemon user:/:/bin/bash
systemd-journal-upload:x:997:997:systemd Journal Upload:/:/sbin/nologin
systemd-coredump:x:998:998:systemd Core Dumper:/:/sbin/nologin
systemd-journal-remote:x:999:999:systemd Journal Remote:/:/sbin/nologin
archmaster:x:1000:1000::/home/archmaster:/bin/bash

passwdmap options:

1   Look up a user name
2   Look up a UID
3   Display entire file
4   Quit

Your selection: 4
```

## Chapter 14 - Monads<a id="sec-1-8" name="sec-1-8"></a>

### Maybe.hs<a id="sec-1-8-1" name="sec-1-8-1"></a>

Page 367 - File: <rwh/ch14/Maybe.hs>

```haskell
-- file: ch14/Maybe.hs 

data Maybe a = Nothing | Just a 

instance Monad Maybe where
  
 -- chain 
 (>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
 Just x    >>= fn = fn x
 Nothing   >>= fn = Nothing

 -- inject 
 return :: a ->  Maybe a
 return a = Just a 

 ---
 (>>) :: Maybe a -> Maybe b -> Maybe b
 Just _   >> mb = mb
 Nothing  >> mb = Nothing
 
 fail _ = Nothing 



{- Function that executes the Maybe monad. If the computation 
   fails the third parameter is Nothing it returns the value n, 
   on  the other hand if the computation succeeds the third
   parameter is (Just x) it applies the function (a -> b) to the
   value x wrapped in the monad. 

-}
maybe :: b -> (a -> b ) -> Maybe a -> b
maybe n _ Nothing  = n 
maybe _ f (Just x) = f x
```

### MultiplyTo.hs<a id="sec-1-8-2" name="sec-1-8-2"></a>

Page: 343. File: <rwh/ch14/MultiplyTo.hs>

```haskell
guarded :: Bool -> [a] -> [a]
guarded True  xs = xs
guarded False _  = []

multiplyTo :: Int -> [(Int, Int)]
multiplyTo n = do 
  x <- [1..n]
  y <- [x..n]
  guarded (x * y == n) $
    return (x, y)
```

Running:

```haskell
>>> :load ch14/MultiplyTo.hs 
[1 of 1] Compiling Main             ( ch14/MultiplyTo.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> :t multiplyTo 
multiplyTo :: Int -> [(Int, Int)]
>>> 
>>> multiplyTo 8
[(1,8),(2,4)]
>>> 
>>> multiplyTo 100
[(1,100),(2,50),(4,25),(5,20),(10,10)]
>>> 
>>> multiplyTo 891
[(1,891),(3,297),(9,99),(11,81),(27,33)]
>>> 
>>> multiplyTo 1000
[(1,1000),(2,500),(4,250),(5,200),(8,125),(10,100),(20,50),(25,40)]
>>>
```

### SimpleState.hs<a id="sec-1-8-3" name="sec-1-8-3"></a>

Page: 347 - File: <rwh/ch14/SimpleState.hs> 

```haskell
-- file: ch14/SimpleState.hs

{-
   This function transforms one state into another yielding
   a result (output). The state monad is also called
   State Transformer Monad. 

   s            : Type of state
   a            : Type of state output

   s -> (a, s)  : State transformer function 


            :: SimpleState s a         :: SimpleState s a

             |-------------|            |-------------|
State 0      |             | State 1    |             | State 2
        ---> | a -> (a, s) | ------>    | a -> (a, s) | ------->            
             |-------------|            |-------------|  
                  |                                |
                  |                                |
                 \ / Output 1: a                  \ /  Output 2: a

 -}
type SimpleState s a = s -> (a, s)

-- A type can be partially applied. The type constructor is:
-- SimpleState s
-- 
type StringState a = SimpleState String a

returnSt :: a -> SimpleState s a
returnSt a = \s -> (a, s)

returnAlt :: a -> SimpleState s a
returnAlt a s = (a, s)

bindSt :: (SimpleState s a) -> (a -> SimpleState s b) -> SimpleState s b
bindSt m fn = \s -> let (a, s') = m s
                    in (fn a) s'

{-
  A more readable version of bindSt 

   -- m == step
   -- k == makeStep
   -- s == oldState 
-}
bindAlt :: (SimpleState s a) -> (a -> SimpleState s b) -> SimpleState s b
bindAlt step makeStep oldState =
  let (result, newState) = step oldState
  in (makeStep result) newState


{- Get current state and returens it as result -}
getSt :: SimpleState s s 
getSt = \s -> (s, s)

{- Set current state and ignore the current one. -}
putSt :: s -> SimpleState s ()
putSt s = \_ -> ((), s)
```

Running:

```haskell
>>> :load ch14/SimpleState.hs 
[1 of 1] Compiling Main             ( ch14/SimpleState.hs, interpreted )
Ok, modules loaded: Main.
>>> 

>>> :set +m
>>> 

-- It can be pasted in Ghci 
--
-- The current state is n, the next state is 
-- n + 1, the state output is 2 * n 
--

{-
       |---------|        |----------|       |----------|     |---------|
   --->| stateFn |---->   | stateFn  |-----> | stateFn  |---->| stateFn |--> 
  1    |---------|    2   |----------|   3   |----------| 4   |---------|   5 ...
            |                   |                 |                | 
            |                   |                 |                |
           \ /                 \ /               \ /              \ /
           output = 3          output = 4       output = 6        output = 8
-}


:{

let stateFn :: SimpleState Int Int 
    stateFn n = (2 * n, n + 1)
:}

>>> :t stateFn 
stateFn :: SimpleState Int Int
>>> 

>>> stateFn 1
(2,2)
>>> stateFn 2
(4,3)
>>> stateFn 3
(6,4)
>>> stateFn 4
(8,5)
>>> stateFn 5
(10,6)
>>> stateFn 6
(12,7)
>>> 


>>> :t returnSt 10
returnSt 10 :: Num a => SimpleState s a
>>> 
>>> (returnSt 10) 3
(10,3)
>>> (returnSt 20) 4
(20,4)
>>> (returnSt 12) 5
(12,5)
>>> 

>>> getSt 3
(3,3)
>>> 
>>> getSt 10
(10,10)
>>> 

>>> (putSt 3) 4
((),3)
>>> (putSt 3) 4
((),3)
>>> (putSt 3) 5
((),3)
>>> (putSt 3) 10
((),3)
>>>
```

### State.hs<a id="sec-1-8-4" name="sec-1-8-4"></a>

Page: 349 - File: <rwh/ch14/State.hs> 

```haskell
{-
  Applies a state transformer to a state and returns a new state
  yielding a result. 
  
  runState :: State s a -> s -> (a, s)

 -}
newtype State s a = State { runState :: s -> (a, s)  }


returnState :: a -> State s a
returnState a = State ( \s -> (a, s) )

bindState :: State s a -> (a -> State s b) -> State s b
bindState m fn = State $ \s -> let (a, s') = runState m s
                               in runState (fn a) s'

-- evalState : Returns only the result, throwing away the final state
--
evalState :: State s a -> s -> a
evalState fn s = fst (runState fn s)

-- execState : Throws the result away, returning only the final state
execState :: State s a -> s -> s
execState fn s = snd (runState fn s)


get :: State s s
get = State (\s -> (s, s))

put :: s -> State s ()
put s = State (\ _ -> ((), s))

{- State Monad Evaluation Functions -}

-- runState : Returns both the result and the final state



{- State Monad Evaluation Functions -}

-- runState : Returns both the result and the final state



{-
   Applies a function to the result of the
   state transformer (state monad) application
   keeping the current state. 

-}
instance Functor (State s) where
  
  {- fmap :: (a -> b) -> F a -> F b -}
  --
  -- fmap :: (a -> b) -> State s a -> State s b
  fmap f fns =
    State $ \oldState -> let (output, newState) = runState fns oldState
                         in (f output, newState)
                 


instance Applicative (State s) where
  
  pure = returnState

  --
  -- (<*>) :: State s (a -> b) -> State s a -> State s b
  --
  -- fsa :: State s a
  --
  -- fn :: State s (a -> b)
  --
  -- output :: a 
  -- newState :: s
  --
  -- f_a_to_b :: a -> b
  -- newState' :: s
  --
  fn <*> fsa = State $ \ oldState ->
    let (output, newState) = runState fsa oldState in
    let (f_a_to_b, newState') = runState fn newState in
    (f_a_to_b output, newState')


               

instance Monad (State s) where

  -- return :: a -> State s a 
  --
  return a = State $ \s ->  (a, s)   
  
  -- (>>=) :: State s a -> (a -> State s b) -> State s b
  --
  -- StateFn    :: State s a
  --
  -- stateMaker :: a -> State s b 
  --
  -- result     :: a
  --
  -- newState   :: s 
  --
  --  
  --
  stateFn >>= stateMaker  =

    State $ \oldState -> let (result, newState) = runState stateFn oldState
                         in  runState (stateMaker result) newState
```

Running: 

```haskell
>>> :load ch14/State.hs 
[1 of 1] Compiling Main             ( ch14/State.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> :set +m
>>> 
 
>>> let stateFn1 = State (\x -> (2 * x, x + 1)) :: State Int Int
>>> :t stateFn1 
stateFn1 :: State Int Int
>>> 


>>> runState stateFn1 0
(0,1)
>>> runState stateFn1 1
(2,2)
>>> runState stateFn1 2
(4,3)
>>> runState stateFn1 3
(6,4)
>>> runState stateFn1 4
(8,5)
>>> runState stateFn1 5
(10,6)
>>> 

>>> runState (fmap (*2) stateFn1) 0
(0,1)
>>> runState (fmap (*2) stateFn1) 1
(4,2)
>>> runState (fmap (*2) stateFn1) 2
(8,3)
>>> runState (fmap (*2) stateFn1) 3
(12,4)
>>> runState (fmap (*2) stateFn1) 4
(16,5)
>>> runState (fmap (*2) stateFn1) 5
(20,6)
>>> 


{- stateFn in monadic notation
   This block can be copied in the repl.
-}

:{
let stateFn2 :: State Int Int
    stateFn2 = do
      sc <- get
      put (sc + 1)
      return $ 2 * sc

:}

>>> :t stateFn2
stateFn2 :: State Int Int
>>> 
>>> runState stateFn2 0
(0,1)
>>> runState stateFn2 1
(2,2)
>>> runState stateFn2 2
(4,3)
>>> runState stateFn2 3
(6,4)
>>> runState stateFn2 4
(8,5)
>>> runState stateFn2 5
(10,6)
>>> 

{- stateFn3   -}

:{
let stateFn3 :: State Int Int
    stateFn3 =
      get          >>= \ sc ->
      put (sc + 1) >>= \_   ->
      return (2 * sc)
      
:}

>>> runState stateFn3 0
(0,1)
>>> runState stateFn3 1
(2,2)
>>> runState stateFn3 2
(4,3)
>>> runState stateFn3 3
(6,4)
>>> runState stateFn3 4
(8,5)
>>> runState stateFn3 5
(10,6)
>>> 

>>> runState (replicateM 10 stateFn3) 0
([18,16,14,12,10,8,6,4,2,0],10)
>>> 
>>> runState (replicateM 0 stateFn3) 0
([],0)
>>> runState (replicateM 1 stateFn3) 0
([0],1)
>>> runState (replicateM 2 stateFn3) 0
([2,0],2)
>>> runState (replicateM 3 stateFn3) 0
([4,2,0],3)
>>> runState (replicateM 4 stateFn3) 0
([6,4,2,0],4)
>>> runState (replicateM 10 stateFn3) 0
([18,16,14,12,10,8,6,4,2,0],10)
>>> 


>>> runState ((\x -> x + 3) <$>  stateFn3) 0
(3,1)
>>> runState ((\x -> x + 3) <$>  stateFn3) 1
(5,2)
>>> runState ((\x -> x + 3) <$>  stateFn3) 2
(7,3)
>>> runState ((\x -> x + 3) <$>  stateFn3) 3
(9,4)
>>> runState ((\x -> x + 3) <$>  stateFn3) 4
(11,5)
>>>
```

## Chapter 27 - Sockets and Syslog<a id="sec-1-9" name="sec-1-9"></a>

### Requirements<a id="sec-1-9-1" name="sec-1-9-1"></a>

-   [Network Package](https://hackage.haskell.org/package/network-2.6.2.1)

### UDP Syslog Server and Client<a id="sec-1-9-2" name="sec-1-9-2"></a>

Page 611 - File: <rwh/ch27/syslogclient.hs> 

```haskell
-- file: ch27/syslogclient.hs
import Data.Bits
import Network.Socket
import Network.BSD
import Data.List
import SyslogTypes


data SyslogHandle = 
    SyslogHandle {slSocket :: Socket,
                  slProgram :: String,
                  slAddress :: SockAddr}

openlog :: HostName             -- ^ Remote hostname, or localhost
        -> String               -- ^ Port number or name; 514 is default
        -> String               -- ^ Name to log under
        -> IO SyslogHandle      -- ^ Handle to use for logging

openlog hostname port progname =
    do -- Look up the hostname and port.  Either raises an exception
       -- or returns a nonempty list.  First element in that list
       -- is supposed to be the best option.
       addrinfos <- getAddrInfo Nothing (Just hostname) (Just port)
       let serveraddr = head addrinfos
      
       -- Establish a socket for communication
       sock <- socket (addrFamily serveraddr) Datagram defaultProtocol

       -- Save off the socket, program name, and server address in a handle
       return $ SyslogHandle sock progname (addrAddress serveraddr)


syslog :: SyslogHandle -> Facility -> Priority -> String -> IO ()
syslog syslogh fac pri msg =
    sendstr sendmsg
    where code = makeCode fac pri
          sendmsg = "<" ++ show code ++ ">" ++ (slProgram syslogh) ++
                    ": " ++ msg
          -- Send until everything is done
          sendstr :: String -> IO ()
          sendstr [] = return ()
          sendstr omsg = do sent <- sendTo (slSocket syslogh) omsg
                                    (slAddress syslogh)
                            sendstr (genericDrop sent omsg)


closelog :: SyslogHandle -> IO ()
closelog syslogh = sClose (slSocket syslogh)


{- | Convert a facility and a priority into a syslog code -}
makeCode :: Facility -> Priority -> Int
makeCode fac pri =
    let faccode = codeOfFac fac
        pricode = fromEnum pri 
        in
          (faccode `shiftL` 3) .|. pricode
```

File: <rwh/ch27/SyslogTypes.hs> 

```haskell
module SyslogTypes where
{- | Priorities define how important a log message is. -}

data Priority = 
            DEBUG                   -- ^ Debug messages
          | INFO                    -- ^ Information
          | NOTICE                  -- ^ Normal runtime conditions
          | WARNING                 -- ^ General Warnings
          | ERROR                   -- ^ General Errors
          | CRITICAL                -- ^ Severe situations
          | ALERT                   -- ^ Take immediate action
          | EMERGENCY               -- ^ System is unusable
                    deriving (Eq, Ord, Show, Read, Enum)

{- | Facilities are used by the system to determine where messages
are sent. -}

data Facility = 
              KERN                      -- ^ Kernel messages
              | USER                    -- ^ General userland messages
              | MAIL                    -- ^ E-Mail system
              | DAEMON                  -- ^ Daemon (server process) messages
              | AUTH                    -- ^ Authentication or security messages
              | SYSLOG                  -- ^ Internal syslog messages
              | LPR                     -- ^ Printer messages
              | NEWS                    -- ^ Usenet news
              | UUCP                    -- ^ UUCP messages
              | CRON                    -- ^ Cron messages
              | AUTHPRIV                -- ^ Private authentication messages
              | FTP                     -- ^ FTP messages
              | LOCAL0                  
              | LOCAL1
              | LOCAL2
              | LOCAL3
              | LOCAL4
              | LOCAL5
              | LOCAL6
              | LOCAL7
                deriving (Eq, Show, Read)

facToCode = [ 
                       (KERN, 0),
                       (USER, 1),
                       (MAIL, 2),
                       (DAEMON, 3),
                       (AUTH, 4),
                       (SYSLOG, 5),
                       (LPR, 6),
                       (NEWS, 7),
                       (UUCP, 8),
                       (CRON, 9),
                       (AUTHPRIV, 10),
                       (FTP, 11),
                       (LOCAL0, 16),
                       (LOCAL1, 17),
                       (LOCAL2, 18),
                       (LOCAL3, 19),
                       (LOCAL4, 20),
                       (LOCAL5, 21),
                       (LOCAL6, 22),
                       (LOCAL7, 23)

           ]


codeToFac = map (\(x, y) -> (y, x)) facToCode


{- | We can't use enum here because the numbering is discontiguous -}
codeOfFac :: Facility -> Int
codeOfFac f = case lookup f facToCode of
                Just x -> x
                _ -> error $ "Internal error in codeOfFac"

facOfCode :: Int -> Facility
facOfCode f = case lookup f codeToFac of
                Just x -> x
                _ -> error $ "Invalid code in facOfCode"
```

File: <rwh/ch27/syslogserver.hs> 

```haskell
import Data.Bits
import Network.Socket
import Network.BSD
import Data.List

type HandlerFunc = SockAddr -> String -> IO ()

serveLog :: String              -- ^ Port number or name; 514 is default
         -> HandlerFunc         -- ^ Function to handle incoming messages
         -> IO ()

serveLog port handlerfunc = withSocketsDo $
    do -- Look up the port.  Either raises an exception or returns
       -- a nonempty list.  
       addrinfos <- getAddrInfo 
                    (Just (defaultHints {addrFlags = [AI_PASSIVE]}))
                    Nothing (Just port)
       let serveraddr = head addrinfos
       -- Create a socket
       sock <- socket (addrFamily serveraddr) Datagram defaultProtocol
       -- Bind it to the address we're listening to
       bindSocket sock (addrAddress serveraddr)
       -- Loop forever processing incoming data.  Ctrl-C to abort.
       procMessages sock
    where procMessages sock =
              do -- Receive one UDP packet, maximum length 1024 bytes,
                 -- and save its content into msg and its source
                 -- IP and port into addr
                 (msg, _, addr) <- recvFrom sock 1024
                 -- Handle it
                 handlerfunc addr msg
                 -- And process more messages
                 procMessages sock

-- A simple handler that prints incoming packets
plainHandler :: HandlerFunc
plainHandler addr msg = 
    putStrLn $ "From " ++ show addr ++ ": " ++ msg
```

Running:

This app loaded in GHCI without any errors, however it didn't work the
server printed nothing. It was tested in GHC/GHCI Version 7.10.2, Arch
Linux 64 bits, Linux version 4.4.3-1-ARCH.

### TCP Syslog Server and Client<a id="sec-1-9-3" name="sec-1-9-3"></a>

Page 617 - File: <rwh/ch27/syslogtcpserver.hs> 

```haskell
import Data.Bits
import Network.Socket
import Network.BSD
import Data.List
import Control.Concurrent
import Control.Concurrent.MVar
import System.IO


type HandlerFunc = SockAddr -> String -> IO ()

serveLog :: String              -- ^ Port number or name; 514 is default
         -> HandlerFunc         -- ^ Function to handle incoming messages
         -> IO ()
serveLog port handlerfunc = withSocketsDo $
    do -- Look up the port.  Either raises an exception or returns
       -- a nonempty list.  
       addrinfos <- getAddrInfo 
                    (Just (defaultHints {addrFlags = [AI_PASSIVE]}))
                    Nothing (Just port)
       let serveraddr = head addrinfos
       -- Create a socket
       sock <- socket (addrFamily serveraddr) Stream defaultProtocol
       -- Bind it to the address we're listening to
       bindSocket sock (addrAddress serveraddr)
       -- Start listening for connection requests.  Maximum queue size
       -- of 5 connection requests waiting to be accepted.
       listen sock 5
       -- Create a lock to use for synchronizing access to the handler
       lock <- newMVar ()
       -- Loop forever waiting for connections.  Ctrl-C to abort.
       procRequests lock sock
    where
          -- | Process incoming connection requests
          procRequests :: MVar () -> Socket -> IO ()
          procRequests lock mastersock = 
              do (connsock, clientaddr) <- accept mastersock
                 handle lock clientaddr
                    "syslogtcpserver.hs: client connnected"
                 forkIO $ procMessages lock connsock clientaddr
                 procRequests lock mastersock

          -- | Process incoming messages
          procMessages :: MVar () -> Socket -> SockAddr -> IO ()
          procMessages lock connsock clientaddr =
              do connhdl <- socketToHandle connsock ReadMode
                 hSetBuffering connhdl LineBuffering
                 messages <- hGetContents connhdl
                 mapM_ (handle lock clientaddr) (lines messages)
                 hClose connhdl
                 handle lock clientaddr 
                    "syslogtcpserver.hs: client disconnected"
          -- Lock the handler before passing data to it.
          handle :: MVar () -> HandlerFunc
          -- This type is the same as
          -- handle :: MVar () -> SockAddr -> String -> IO ()
          handle lock clientaddr msg =
              withMVar lock 
                 (\a -> handlerfunc clientaddr msg >> return a)

-- A simple handler that prints incoming packets
plainHandler :: HandlerFunc
plainHandler addr msg = 
    putStrLn $ "From " ++ show addr ++ ": " ++ msg
```

File: <rwh/ch27/syslogtcpserver.hs> 

```haskell
-- file: ch27/syslogtcpclient.hs
import Data.Bits
import Network.Socket
import Network.BSD
import Data.List
import SyslogTypes
import System.IO

data SyslogHandle = 
    SyslogHandle {slHandle :: Handle,
                  slProgram :: String}

openlog :: HostName             -- ^ Remote hostname, or localhost
        -> String               -- ^ Port number or name; 514 is default
        -> String               -- ^ Name to log under
        -> IO SyslogHandle      -- ^ Handle to use for logging

openlog hostname port progname =
    do -- Look up the hostname and port.  Either raises an exception
       -- or returns a nonempty list.  First element in that list
       -- is supposed to be the best option.
       addrinfos <- getAddrInfo Nothing (Just hostname) (Just port)
       let serveraddr = head addrinfos

       -- Establish a socket for communication
       sock <- socket (addrFamily serveraddr) Stream defaultProtocol

       -- Mark the socket for keep-alive handling since it may be idle
       -- for long periods of time
       setSocketOption sock KeepAlive 1

       -- Connect to server
       connect sock (addrAddress serveraddr)

       -- Make a Handle out of it for convenience
       h <- socketToHandle sock WriteMode

       -- We're going to set buffering to BlockBuffering and then
       -- explicitly call hFlush after each message, below, so that
       -- messages get logged immediately
       hSetBuffering h (BlockBuffering Nothing)

       -- Save off the socket, program name, and server address in a handle
       return $ SyslogHandle h progname



syslog :: SyslogHandle -> Facility -> Priority -> String -> IO ()
syslog syslogh fac pri msg =
    do hPutStrLn (slHandle syslogh) sendmsg
       -- Make sure that we send data immediately
       hFlush (slHandle syslogh)
    where code = makeCode fac pri
          sendmsg = "<" ++ show code ++ ">" ++ (slProgram syslogh) ++
                    ": " ++ msg

closelog :: SyslogHandle -> IO ()
closelog syslogh = hClose (slHandle syslogh)

{- | Convert a facility and a priority into a syslog code -}
makeCode :: Facility -> Priority -> Int
makeCode fac pri =
    let faccode = codeOfFac fac
        pricode = fromEnum pri 
        in
          (faccode `shiftL` 3) .|. pricode
```

Running: syslogtcpserver with telnet as client:

```haskell
>>> :load "syslogtcpserver.hs"
[1 of 1] Compiling Main             ( syslogtcpserver.hs, interpreted )
Ok, modules loaded: Main.
>>> 

--  Open another terminal window 
--  and enter: 
--
--  $ telnet localhost 10514 
--
--  and type the log messages. 
--
--
>>> serveLog "10514" plainHandler
From 127.0.0.1:49570: syslogtcpserver.hs: client connnected
From 127.0.0.1:49570: Test message - Fatal kernel error
From 127.0.0.1:49570: Application server running OK.
```

Running: syslogtcpserver with syslogclient:

syslogserver:

```haskell
>>> :load "syslogtcpserver.hs"
[1 of 1] Compiling Main             ( syslogtcpserver.hs, interpreted )
Ok, modules loaded: Main.
>>> 
>>> serveLog "10514" plainHandler
```

syslogclient:

```haskell
>>> 
>>> :load sylogtcpclient.hs 
[1 of 2] Compiling SyslogTypes      ( SyslogTypes.hs, interpreted )
[2 of 2] Compiling Main             ( sylogtcpclient.hs, interpreted )
Ok, modules loaded: SyslogTypes, Main.
>>> 

>>> openlog "localhost" "10514" "tcptest"
 *** Exception: connect: does not exist (Connection refused)
>>> 

>>> sl <- openlog "localhost" "1514" "tcptest"
 *** Exception: connect: does not exist (Connection refused)
>>>
```