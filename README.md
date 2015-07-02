# More Loop Types

## Objectives:

1. Fast enumeration with the `for-in` loop.
2. Distinguishing the use cases of `for` and `for-in` loops.
3. "Simple" loop types `while` and `do-while`.

## Introduction

Interacting with the data held in collections often involves implementing loops. Before we go on to discuss collections in more detail, it's important that we spend some more time practicing with loops so that we're more proficient at accessing and manipulating their contents. In Objective-C, there are four types of loops—one of which you should already have been introduced to:

  * The `for` loop,
  * The `for-in` loop (a.k.a. "fast enumeration"),
  * The `while` loop, and
  * The `do-while` loop.

### Review: The `for` Loop

The `for` loop is the most commonly-used kind of loop in Objective-C. 

```objc
for (initialization; condition; increment) {
    statements
}
```
It's a favorite due to its integration of a counter (often `NSUInteger i`) right in its declaration syntax. The counter makes it easy to keep track of the current index when working with an array, or in any situation when the specific iteration of the loop matters to its implementation, such as the example loop that we built in order to produce a repetitive sentence about buffalo:

```objc
NSMutableString *grammarQuirk = [[NSMutableString alloc] init];

for (NSUInteger i = 0; i < 8; i++) {
    NSString *buffalo = @"buffalo";
    if (i == 0 || i == 2 || i == 6) {
        buffalo = [buffalo capitalizedString];
    }

    if (i == 7) {
        [grammarQuirk appendFormat:@"%@.", buffalo];
    } else {
        [grammarQuirk appendFormat:@"%@ ", buffalo];
    }
}

NSLog(@"%@", grammarQuirk);
```
This will print: `Buffalo buffalo Buffalo buffalo buffalo buffalo Buffalo buffalo.`.

But what if we have a collection whose contents we want to use in the same way each time. Do we really need the counter? Or, what if we have a collection that isn't an array: can we still iterate over it? Yes, we can. Enter, the `for-in` loop.

## Fast Enumeration: The `for-in` Loop

The `for-in` loop works in a very similar way to the `for` loop but is optimized for certain use cases. A `for-in` loop declaration is structured like this:

```objc
for (Class *object in collection) {
    statements
}
```
This is equivalent to declaring a `for` loop in the following manner, but without needing to use the counter in the implementation beyond the initial accessing of the index:

```objc
for (NSUInteger i; i < collection.count; i++) {
    Class *object = collection[i];
    statements
}
```
**Top-tip:** *Fast enumeration works best with collections of a single type. Accessing a mixed type collection with fast enumeration can cause the* `for-in` *loop to crash at run time.*

Because a `for-in` loop doesn't depend on a counter to access the collection by index, it can be used to iterate over *unordered* collections such as dictionaries (`NSDictionary` and `NSMutableDictionary`), or ordered collections when the order isn't significant to the operation.

```objc
NSMutableArray *usernames = [ @[ @"jmburges", @"tim_the_enchanter", @"tomalley", @"jimmysee" ] mutableCopy];
NSString *requestedUsername = @"markedwardmurray";
BOOL uniqueUsername = YES;
    
for (NSString *anyUsername in usernames) {
    if ([requestedUsername isEqualToString:anyUsername]) {
        uniqueUsername = NO;
    }
}
    
if (uniqueUsername) {
    [usernames addObject:requestedUsername];
    NSLog(@"Added username: %@", requestedUsername);
}
```
This will print: `Added username: markedwardmurray`.

The efficiency differences between a `for` loop and `for-in` loop are rather miniscule. If you're unsure if you can use a `for-in` for a specific case, write your first implementation with a `for` loop. If you don't use the counter integer outside of accessing the collection, you can probably refactor to using a `for-in` loop.

## Simple Loops: `while` and `do-while`

The two other loop types available in Objective-C are the `while` and the `do-while` loops. Neither contains a counter by default and will run so long as the given condition evaluates as true.

```objc
while (condition) {
    statements
}
```

```objc
do {
    statements
} while (condition)
```

**Top-tip:** *It's easy to get stuck in an infinite loop using these types so be cautious! Include at least one* `NSLog()` *within the loop implementation to let you see if your loop ends up running continuously.*

The main difference between these two types is that a `do-while` loop evaluates its condition at the *end* of the implementation to judge whether it should run *again*. It's guaranteed that a `do-while` loop will always run at least once.

Let's use a `while` loop to generate a longer form of the exclamation "eek!":

```objc
NSString *eek = @"";

while (eek.length < 10) {
    eek = [eek stringByAppendingString:@"e"];
}
eek = [eek stringByAppendingString:@"k!"];

NSLog(@"%@", eek);
```
This will print: `eeeeeeeeeek!` with ten `e`s. We can change the length of our "eek" but altering the integer in the loop's conditional statement.

If say, we have a simple dice game that allows a player to total their die rolls up until they roll a one, we can use a `do-while` loop that contains the first die roll.

```objc
NSUInteger dieRoll = 0;
NSUInteger rollTotal = 0;

do {
    dieRoll = 1 + arc4random() % 6;
    rollTotal += dieRoll;
        
    NSLog(@"Rolled a %lu", dieRoll);
    
} while (dieRoll > 1);

NSLog(@"Total score: %lu", rollTotal);
```
This will print something a little different each time, but it would resemble:

```
Rolled a 2
Rolled a 6
Rolled a 1
Total Score: 9
```










