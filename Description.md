## Introduction ##

We were to write a program that could simulate the Game of Life (`class Life`) using cells that exhibited Conway behavior (`class ConwayCell`), Fredkin behavior (`class FredkinCell`), or a hybrid of the two (`class Cell`).

## Implementation ##

Our Life board contains a vector of vectors of T, where T is either `ConwayCell`, `FredkinCell` or `Cell`, along with read/write methods, a couple of iterators to iterate through each cell's corner and edge neighbors, and a method simulating the Life board through one generation.

Because we used two iterators to navigate each cell's neighbors, we took the "extra-space" approach, where there would be a couple of rows and a couple of columns that were permanently dead and unseen by the user (furthermore, we actually have four permanent dead rows and three permanent dead columns). Without this extra space, we could easily seg fault or have undefined behavior.

In every simulation of a generation, we always check the liveliness of each cell's edge neighbors, and, depending on whether we're dealing with a `ConwayCell`, we may also check each/some (`ConwayCell`/`Cell`) cell's corner neighbors. After we iterate through the whole board, we go through another iteration to update all cells to the next generation (to do this in one iteration, and not two, would cause major problems; you can't update a cell right away).

There are four other classes in this program, three of which have already been mentioned. The fourth is `AbstractCell`, an abstract class that holds the common properties of all forms of cells that are children of `AbstractCell`. Such properties include a liveliness bool, and an is\_alive() getter method. There are also a few methods that the class forces its children to implement, including read/write methods.

`ConwayCell` and `FredkinCell` are concrete children of `AbstractCell` and (obviously) implement the abstract methods. To `ConwayCell`, there's not much more than that. `FredkinCell`, however, also needs to keep track of its age, as well as whether it is alive and has stayed alive. Such variables, and getter methods were added here.

`Cell` is not a child of `AbstractCell` (nor should it be), and is a sort of hybrid between the Conway and Fredkin implementations of the game of life. Each instance of `Cell` starts out behaving as a (new) `FredkinCell` (it contains a pointer to an `AbstractCell`), and changes its behavior to a (new) `ConwayCell` once the age of the cell has reached two.

While `Cell` is not a child of `AbstractCell`, it still implements much of the same methods, otherwise we run into problems running the simulation method on the Life board. Plus, it just makes common sense.


## Optimizations ##

We were able to avoid using malloc() and free(). The new and delete keywords were only used inside the handle class.