## How to solve a coding interview question?

In this post, I'll analyze an interview question. The purpose of these questions is to test candidates' technical knowledge, coding ability, problem-solving skills, and creativity. If you can't find a complete solution, **it might be ok**.

You can find all code samples [here](https://replit.com/@japananh/dictionary).

# Problem

I was given an array of strings and a word. I need to create a function to test whether that word is in the array or not.

```text
// input: ["car", "cat", "bar"]
isInDict("car") // true
isInDict("cat") // true
isInDict("at") // false
```

# Version 1

`Array.includes` in  `isInDict()` has a time complexity is O(n).

```javascript
class Dictionary {
  constructor(words) {
    this.dict = words
  }

  isInDict(word) {
    return this.dict.includes(word) // O(n)
  }
}
```
```javascript
const dict = new Dictionary(['car', 'bar', 'cat', 'bat'])
console.log(dict.isInDict('cat')) // true
console.log(dict.isInDict('car')) // true
console.log(dict.isInDict('ba')) // false
```

# Version 2

I can improve the time complexity from `O(n)` to **a constant time** `O(1)` by using `Set.has()`.

```javascript
class Dictionary2 {
  constructor(wordArr) {
    this.dict = new Set(wordArr)
  }

  isInDict(word) {
    return this.dict.has(word) // O(1)
  }
}
```
```javascript
const dict = new Dictionary(['car', 'bar', 'cat', 'bat'])
console.log(dict.isInDict('cat')) // true
console.log(dict.isInDict('car')) // true
console.log(dict.isInDict('ba')) // false
```

# Version 3

My code will fail if I pass a **regex** into the `isInDict` function.

```javascript
const dict = new Dictionary(['car', 'bar', 'cat', 'bat'])
console.log(dict.isInDict('cat')) // true
console.log(dict.isInDict('ca*')) // false -> incorrect
console.log(dict.isInDict('*at')) // false -> incorrect
```

To solve the problem, I create a new data structure. It might look like this.

```javascript
Dictionary {
  dict: {
    car: true,
    '*ar': true,
    'c*r': true,
    'ca*': true,
    bar: true,
    'b*r': true,
    'ba*': true,
    cat: true,
    '*at': true,
    'c*t': true,
    bat: true,
    'b*t': true
  }
}
```

After refactoring the `constructor()`, the final code will look like this.

```javascript
class Dictionary {
  constructor(words) {
    this.dict = words.reduce((acc, word) => {
      acc[word] = true

      word.split('').forEach((letter, i) => {
        const start = word.slice(0, i)
        const end = word.slice(i + 1)
        const partialWord = `${start}*${end}`
        acc[partialWord] = true
      })

      return acc
    }, {})
  }

  isInDict(word) {
    return !!this.dict[word]
  }
}
```

```javascript
const dict = new Dictionary(['car', 'bar', 'cat', 'bat'])
console.log(dict.isInDict('cat')) // true
console.log(dict.isInDict('ca*')) // true
console.log(dict.isInDict('*at')) // true
```
