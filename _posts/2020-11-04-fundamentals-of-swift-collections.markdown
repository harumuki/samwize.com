---
layout: post
title: "Fundamentals of Swift Collections"
date: 2020-11-04T18:10:05+08:00
categories: [Foundation]
published: false
---

While preparing for a technical coding interview, and practising LeetCode questions, I created this _cheatsheet_.

## Loop array

```swift
for value in array
for index in array.indices
for (index, value) in array.enumerated()
```

## Loop array with range

```swift
for i in 0..<array.count // Traditional loop with indices

for v in array[2...] // Skip the first 2 elements
for v in array[..<(array.count-2)] // Skip the last 2 elements
for v in array.prefix(3) // The first 3
for v in array.suffix(3) // The last 3
```

## Loop dictionary

```swift
for (key, value) in dictionary
```

## Check if dictionary has key

```swift
dictionary.keys.contains("k")
```

```swift
// Alternative, if the value type is _never_ `Optional`
if dictionary["k"] == nil // "k" is not in keys
```

## Special sequences

```swift
let tenOnes = repeatElement(1, count: 10) // Array(tenOnes)

stride(from: 0, to: 60, by: 5) // Every 5 min
```
## Array CRUD

```swift
// Find
firstIndex(of: x) // O(n)
```

```swift
// Insert
append(x) // O(1)
append(contentsOf: anArray)
insert(x, at: i) // O(n)
```

It is important to note that `append` is much faster than `insert`.

```swift
// Remove
remove(at: i) // O(n)
removeAll(where: {..})
dropFirst(x)
let last = popLast() //
```

```swift
// Move
move(fromOffsets:toOffset:)
```

## Mutate

```swift
sorted()
reversed()
shuffled()
swapAt()
```

There are more ops using your own predicate: `max(by:)`, `sorted(by:)`, `filter()`, `map()`

## Partition

`partition(by:)` is a powerful API, and quite advanced. It reorders the sequence by a predicate, such that the right half **satisfy the predicate** (while left half does not).

It returns the index of the first index in the right half.

```swift
let p = array.partition { .. }
let h1 = list[..<p] // Left half
let h2 = list[p...] // Right half
```

Note that it is an **unstable partition**, which means the order in both half are not preserved. There is an [internal](https://github.com/apple/swift/blob/main/test/Prototypes/Algorithms.swift) `halfStablePartition` which will have the 1st half retain the original order, and also a `stablePartition` which will have both half retain the original order.

## Equatable

To compare and sort, the type should implement `Equatable` protocol.

```swift
extension MyClass: Equatable {
    public static func == (lhs: MyClass, rhs: MyClass) -> Bool {
        return lhs.foo == rhs.foo
    }
}
```

## Identity Comparison ===

There is an op `===` for [identity comparison](https://developer.apple.com/documentation/swift/1538988) eg. _are they the same instances?_

```swift
// For example, in the above Equatable implementation, we could also compare their identities
return lhs === rhs
```

## Dictionary O(1) access

An optimizing trick is to make use of the quick O(1) operation when accessing a dictionary, since they are hashed.

The trade off is that the setup takes O(n), and a space of O(n). And you need the type to be hashable.

## Hashable

```swift
extension MyClass: Hashable {
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self).hashValue)
    }
}
```
