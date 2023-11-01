# Reified Interpreters

In the previous chapter we learned about algebraic data types. In this chapter we'll learn one of their major use cases, implementing interpreters. The interpreter strategy is perhaps the most important in all of functional programming.

Why might we want to implement an interpreter? It sounds like a fairly esoteric thing to do. The central idea is to **separate description from action**. When we use the interpreter strategy our program consists of two parts: the instructions or program that describes what we want to do, and the interpreter that carries the actions in the description. This is the core of many systems that maintain compositionality and reasoning, particularly while allowing effects in our code. For example, imagine we're implementing a graphics library using the interpreter strategy. A description simply says what we want to draw on the screen, but critically it does not actually draw anything. The interpreter takes this description and carries out the actions described by it. As the description doesn't do anything, we can freely compose descriptions. For example, if we have a description that describes a circle, and one for a square, we can compose them by saying we should draw the circle next to the square. This creates a new description that is the composition of the two base descriptions. Drawing on the screen is an effect, and by keeping a clear separation between description and action we can more easily reason about our code.

It may be hard to see how this works from an abstract description. We'll make it concrete in just a moment, by building an interpreter for regular expressions. This example is chosen because regular expressions are familiar to many, so we can concentrate on the interpreter strategy and not on the details of regular expressions. We'll them extract the general strategy from this specific example, and finally give a few pointers to learn more.