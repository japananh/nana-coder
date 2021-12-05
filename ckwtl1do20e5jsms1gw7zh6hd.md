## Refactoring - Part 2

in Part 1, we discussed the [**Extract function**](https://nanacoder.hashnode.dev/refactoring-part-1) and how to apply it. Today, I'll introduce you to the **Extract variable**.

This post was inspired by the book Refactoring, which was written by Martin Fowler and Kent Beck.

# Extract variable

## Example

For example, you have these pieces of code like below.

```go
...
return order.quantity * order.itemPrice - 
    math.max(0, order.quantity - 500) * order.itemPrice * 0.05 + 
    math.min(order.quantity * order.itemPrice * 0.1, 100)
```

That code can be broken into local variables as below.
 
```go
... 
basePrice := order.quantity * order.item.Price
quantityDiscount = math.max(0, order.quantity - 500) * order.itemPrice * 0.05
shipping := math.min(order.quantity * order.itemPrice * 0.1, 100)

return basePrice - quantityDiscount - shipping
```

## When to apply it?

Expressions can become very complex and hard to read. In such situations, local variables may help break the expression down into something more manageable. This allows us to better understand the purpose of what's happening.

By extracting a variable, you add a name to the expression in your code. You have to ensure the expression you want to extract does not have side effects and your variable is immutable.