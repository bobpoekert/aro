Everything is 2d for now.

Each object has the following:

    * A position in space (which is indexed in a global quadtree)
    * Some non-indexed opengl stuff (transformation matrix, texture, etc)
    * A struct containing all of the fields

A qual is a set of conditions which cause the handler to execute when they're all met.

A condition is:

    * An operator (=, <, >, <=, >=, !=, etc)
    * A reference to the value on the left-hand side
    * A reference to the value on teh right-hand side
    * A reference to the qual that this condition is bound to
    * A boolean flag indicating whether it's currently true

A qual is:

    * A vector of conditions
    * A payload to execute when conditions become true
    * A set of objects that are currently matching

Sets of pointers are represented as a single prefix trie.

    * space efficient (pointers often have shared prefixes)
    * O(log n) membership test
    * O(n) intersection

global index `<class, field>` -> set of conditions

To update a field:

    * change the value in the object struct
    * look up condition set
    * for each condition:
        * if condition is true and its met bit is false:
            * set its met bit to true
            * if object isn't already in qual's object set:
                * increment the counter for the object in the qual's object set
                * if the counter >= the number of conditions, execute the payload
        * if condition is false and its met bit is true:
            * set its met bit to false
            * if object is in qual's object set:
                * decrement the counter for the object in the qual's object set
                * if the counter <= 0, remove the object from the set
