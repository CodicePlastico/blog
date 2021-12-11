---
layout: post
current: post
cover: 'assets/images/post-cover/2021-f-sudoku.jpg'
socialcover: 'assets/images/post-cover/2021-f-sudoku-s.jpg'
slug: advent-calendar-sudoku-f#
title: 'Sudoku in F#: a functional story'
date: 2021-12-11 09:00:00
category : [tech]
tags: [functional, programming, howto]
author: [gianni, jacek]
---

# This post is part of the [F# Advent Calendar 2021](https://sergeytihon.com/2021/10/18/f-advent-calendar-2021/).
**Many thanks to Sergey Tihon for organizing all of this.**
**Go checkout the other many and excellent posts.**

In **CodicePlastico** we love .net. We developed some of our applications with .net 5 RC but we already have .net 6 applications ready for production the day after the launch! We are focused mainly on C# and we love it. We refactor old services and applications porting them to a newer version  of .net when it's possible or when our customers need our contribution with an old codebase.
So we have the great opportunity to try new ways to write better code using some interesting features that the community released with the last two .net versions: that means lots of fun for us developers. We started to explore different ways to write loops and branches because... you know... they are so imperative: our life is better with pattern matching, extensions methods, intensively use of Linq, because we have the opportunity to write functional code in C#, do you agree? 
But unfortunately all that glitters is not gold.

<figure style="text-align:center"><img src="/assets/images/post-content/advent-calendar-sudoku/advent-calendar_s_001.png" alt="sudoku in f#" /></figure>

## Once upon a time…

Ten months ago we discovered that we could improve the flow to digitally sign a document using the [Railway Oriented Programming](https://swlaschin.gitbooks.io/fsharpforfunandprofit/content/posts/recipe-part2.html). So we started to think what we need to apply this pattern to our c# application. We discovered some great c# functional libraries, such as [language-ext](https://github.com/louthy/language-ext), but we found it intriguing to develop this library by ourselves, and you can read [here](https://blog.codiceplastico.com/rop-csharp9) (in italian) the article we wrote to celebrate this experiment.
Alberto and I enjoyed ourselves to wrote that article and reaching the desired result, but we agreed that our solution worked and was elegant, but a bit tricky.
<br>
<br>

## ...some time later

Now Our c# code is more and more functional, but sometimes it’s a bit tricky. On the other hand, c# is enhancing its propension to be functional, but it is still an Object Oriented Programming language.
So, last month we began to learn F# and we started with a kata: we wanted to solve a Sudoku, to explore how F# works with arrays, matrices and control flows. Our dream was

<cite>“Write elegant, more readable code, 
focusing on conciseness”</cite>

To learn how to solve this puzzle, we explored existing solutions. We found some old examples (2011) using MSF (Microsoft Solver Foundation) and we found these resources:
*   Fwaris’s [repo](https://gist.github.com/fwaris/949657) and the Robert Pickering’s [repo](https://gist.github.com/robertpi/737992) that use the module SfsWrapper available [here](https://raw.githubusercontent.com/dungpa/opt-query/master/OptQuery/SfsWrapper.fs).
*   James McCaffrey’s article (2015) available on MSDN Magazine, which you can read [here](https://docs.microsoft.com/it-it/archive/msdn-magazine/2014/august/test-run-solving-sudoku-puzzles-using-the-msf-library).
Unfortunately, both these solutions are using the nuget package available [here](https://www.nuget.org/packages/Microsoft.Solver.Foundation/) and using .net 5 the script throws this error:

```c#
System.TypeLoadException: Could not load type 'System.Runtime.Remoting.Messaging.CallContext' from assembly 'mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.
   at Microsoft.SolverFoundation.Services.SolverContext.GetContext()
   at <StartupCode$FSI_0003>.$FSI_0003.main@()
```
<br>

We found [this archived post](https://social.msdn.microsoft.com/Forums/en-US/d4e73692-ae95-4d3f-938c-b2cdb5a2b9c9/solver-foundation-with-net5?forum=solverfoundation) (August 2020) on social.msdn.microsoft.com from piercarlo.s that had got that same error, without responses.

<figure style="text-align:center"><img src="/assets/images/post-content/advent-calendar-sudoku/advent-calendar_s_002.png" alt="sudoku in f#" /></figure>

## Exploring new ways

Ok, we understood that the Microsoft official library could not be the right tool to solve a Sudoku in F#. While searching for something different, we found the unofficial version, available [here](https://www.nuget.org/packages/Unofficial.Microsoft.Solver.Foundation.Express/), but… it’s unofficial so we discarded this solution.
We decided to learn a new library even if it lacks of F# examples, and we choose the [Google OR-Tools optimization library](https://developers.google.com/optimization) that allows us to solve our task that is a well known [constraints optimization problem](https://developers.google.com/optimization/cp).

As we said before, we decided to learn F# and we set our goal: write as little code as possible to solve a Sudoku with Google OR-Tools.
The result was amazing: the code, after a few refactoring tasks, appeared readable, concise thanks to the great support offered by F# to work with arrays and matrices. 

Starting from a multi-dimensional array

```c#
let data = [|
    [| 0; 0; 0;  2; 0; 6;  0; 0; 4; |];
    [| 0; 0; 2;  9; 0; 0;  0; 6; 0; |];
    [| 0; 0; 4;  0; 0; 1;  2; 0; 0; |];
    [| 4; 5; 0;  0; 0; 9;  3; 8; 0; |];
    [| 0; 0; 3;  0; 0; 0;  6; 0; 0; |];
    [| 0; 9; 6;  4; 0; 0;  0; 1; 5; |];
    [| 0; 0; 9;  6; 0; 0;  4; 0; 0; |];
    [| 0; 1; 0;  0; 0; 4;  8; 0; 0; |];
    [| 3; 0; 0;  5; 0; 2;  0; 0; 0; |];
|]
```
<br>

You can view the first implementation of the program in the box below:

```c#
#r "nuget: Google.OrTools.FSharp, 9.1.9490"
 
open Google.OrTools.Sat
 
let flatten (matrix:'a[,]) = matrix |> Seq.cast<'a> |> Seq.toArray
 
let allDifferentArray finish func (model: CpModel) (grid: IntVar [,]) =
    [0 .. finish]
    |> Seq.map func
    |> Seq.iter(fun m -> model.AddAllDifferent(m) |> ignore)
    (model, grid)
 
let allDifferentRows (model: CpModel, grid: IntVar [,]) =
    allDifferentArray ((Array2D.length1 grid) - 1) (fun r -> grid.[r,*]) model grid
 
let allDifferentColumns (model: CpModel, grid: IntVar [,]) =
    allDifferentArray ((Array2D.length2 grid) - 1) (fun c -> grid.[*, c]) model grid
 
let allDifferentSubCubes (model: CpModel, grid: IntVar [,]) =
    for r in 0 .. 3 .. (Array2D.length1 grid) - 1 do
        for c in 0 .. 3 .. (Array2D.length2 grid) - 1 do
            model.AddAllDifferent(flatten grid.[r..r + 2,c..c + 2]) |> ignore
    (model, grid)
 
let setConstants (data:int [] []) =
    let model = CpModel()
    let grid = Array2D.init 9 9 (fun r c ->
        if data.[r].[c] <> 0 then
            model.NewConstant(int64(data.[r].[c]), $"grid{r}{c}")
        else
            model.NewIntVar(int64(1), int64(9), $"grid{r}{c}"))
    (model, grid)
 
let allDifferent =
    allDifferentRows >> allDifferentColumns >> allDifferentSubCubes
 
let solve (model: CpModel, grid: IntVar [,]) =
    let solver = CpSolver()
    let status = solver.Solve(model)
 
    match status with
    | CpSolverStatus.Optimal -> Some (Array2D.init 9 9 (fun r c -> solver.Value(grid.[r, c])))
    | _ -> None
 
setConstants data
|> allDifferent
|> solve
|> printResult
```
<br>

To print the solution you can add this function

```c#
let printResult (result: int64[,] option) =
    match result with
    | Some(result) ->
        for r = 0 to (Array2D.length1 result) - 1 do
            for c = 0 to (Array2D.length2 result) - 1 do
                printf "%d" <| result.[r, c]
            printfn ""
    | None -> printfn "Infeasible"
```
<br>

## What did we learn?

From this first implementation we learnt:
*   How to use Google Or-Tools and how this library can solve other constraints programming issues
*   How lists and matrices work in F# and powerful function to manage them
*   Function composition, using `>>` operator. The operator can reduce the lines of code of our applications
*   Low verbosity of F# code
*   Enhanced readability of applications

<figure style="text-align:center"><img src="/assets/images/post-content/advent-calendar-sudoku/advent-calendar_s_003.png" alt="sudoku in f#" /></figure>

## That’s all?

If you remember last February we tried to use the Railway Oriented Programming in c# and we found our (improvable, for sure) solution a bit tricky. Could F# help us to go ahead and achieve more benefits from a true functional programming language?
After having reread for the second time some chapters from the Scott Wlaschin F# online book, and in particular the [chapter](https://swlaschin.gitbooks.io/fsharpforfunandprofit/content/posts/recipe-part2.html) related to Railway Oriented Programming, using the `>=>` (“fish”) composition operator

```c#
type Result<'TSuccess, 'TFailure> =
    | Success of 'TSuccess
    | Failure of 'TFailure

let (>=>) switch1 switch2 x = 
    match switch1 x with
    | Success s -> switch2 s
    | Failure f -> Failure f
```
<br>

we improved our script to manage the flow creating Success and Error binaries. Thanks to this pattern we improved the script, managing error conditions in a readable, easy, concise way, and we created something that is really **composable**

```c#
#r "nuget: Google.OrTools, 9.1.9490"
 
open Google.OrTools.Sat
 
let flatten (matrix:'a[,]) = matrix |> Seq.cast<'a> |> Seq.toArray
 
let allDifferentArray finish func (model: CpModel) (grid: IntVar [,]) =
    [0 .. finish]
    |> Seq.map func
    |> Seq.iter(fun m -> model.AddAllDifferent(m) |> ignore)
    Success ((model, grid))
 
let allDifferentRows (model: CpModel, grid: IntVar [,]) =
    allDifferentArray ((Array2D.length1 grid) - 1) (fun r -> grid.[r,*]) model grid
   
let allDifferentColumns (model: CpModel, grid: IntVar [,]) =
    allDifferentArray ((Array2D.length2 grid) - 1) (fun c -> grid.[*, c]) model grid
 
let allDifferentSubCubes (model: CpModel, grid: IntVar [,]) =
    for r in 0 .. 3 .. (Array2D.length1 grid) - 1 do
        for c in 0 .. 3 .. (Array2D.length2 grid) - 1 do
            model.AddAllDifferent(flatten grid.[r..r + 2,c..c + 2];) |> ignore
    Success((model, grid))
 
let verifyColumns (data:int [] []) =
    match Array.exists (fun (elem: int []) -> elem.Length <> 9) data with
        | true -> Failure "Columns number not valid"
        | false -> Success data
 
let verifyRows (data:int [] []) =
    match Array.length data = 9 with
        | true -> Success data
        | false -> Failure "Rows number not valid"
 
let setConstants (data:int [] []) =
    let model = CpModel()
    let grid = Array2D.init 9 9 (fun r c ->
        if data.[r].[c] <> 0 then
            model.NewConstant(int64(data.[r].[c]), $"grid{r}{c}")
        else
            model.NewIntVar(int64(1), int64(9), $"grid{r}{c}"))
   
    Success(model, grid)
   
let allDifferent =
    allDifferentRows
    >=> allDifferentColumns
    >=> allDifferentSubCubes
 
let solve (model: CpModel, grid: IntVar [,]) =
    let solver = CpSolver()
    let status = solver.Solve(model)
 
    match status with
    | CpSolverStatus.Optimal -> Success (Array2D.init 9 9 (fun r c -> solver.Value(grid.[r, c])))
    | _ -> Failure "Can't solve sudoku"
 
let solveSudoku =
    verifyColumns
    >=> verifyRows
    >=> setConstants
    >=> allDifferent
    >=> solve
 
solveSudoku data
|> printResult
```
<br>

where now the `printResult` function is the end of the binaries:

```c#
let printResult (result: Result<int64 [,], string>) =
    match result with
    | Success res ->
        for r = 0 to (Array2D.length1 res) - 1 do
            for c = 0 to (Array2D.length2 res) - 1 do
                printf "%d" <| res.[r, c]
            printfn ""
    | Failure f -> printfn "%A" f
```
<br>

## What did we learn?

From this second implementation we learnt:
*   The function composition, using `>=>` fish operator. The operator can reduce the lines of code of our applications because it establishes a relationship between two functions and simplify the implementation of the switch between Success and Failure binaries
*   The low verbosity of F# code managing the error conditions throughout the chain
*   The enhanced error management using the ROP
*   Using the ROP and the fish operator you are focused on the logic of each step of the chain and not on the flow management for the program
*   The real power of functions composition: with the fish operator we can add a function to the chain with the only constraint of respect the contracts between input and output of the two chained near functions


<figure style="text-align:center"><img src="/assets/images/post-content/advent-calendar-sudoku/advent-calendar_s_004.png" alt="sudoku in f#" /></figure>

## What we learned

Learning F#, and functional programming, is learning a new way of modeling solutions, solving problems and writing less code to achieve readability.
Google OR-Tools is a very interesting library to solve not only constraints programming problems, but also other ones like linear programming, scheduling and routing.
We understand how implementing ROP in F# can be simpler than in C#: we discovered that many improvements of latest c# releases are not so exciting compared to what this old-but-gold functional programming language could give us!
<br>
<br>

### Links

*   Railway oriented programming, [https://swlaschin.gitbooks.io/fsharpforfunandprofit/content/posts/recipe-part2.html](https://swlaschin.gitbooks.io/fsharpforfunandprofit/content/posts/recipe-part2.html)
*   Scott Wlaschin - Railway Oriented Programming - Error handling in functional languages, [https://vimeo.com/97344498](https://vimeo.com/97344498)
*   Google OR-Tools, [https://developers.google.com/optimization](https://developers.google.com/optimization)
*   Constraints optimization, [https://developers.google.com/optimization/cp](https://developers.google.com/optimization/cp)