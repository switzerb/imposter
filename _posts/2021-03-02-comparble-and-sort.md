---
layout: post
title:  "Kotlin Comparable"
date:   2021-03-02
categories: kotlin, toolkit
---

By default, the order of elements will be in `natural order` - 5 is greater than 2, -4 is greater than -10, b is greater than a, etc.

## Comparable
To define natural order for a user-defined type, you can implement the interface [Comparable](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-comparable/). Kotlin also provides an additional advantage that the instance implementing Comparable interface can be compared using relational operators:

```kotlin
    data class Fruit(
        val taste_ranking: Int,
        val name: String,
        val color: String
    ) : Comparable<Fruit> {
    
    // defines natural order for the type Fruit
    override fun compareTo(other: Fruit): Int {
        return if (this.taste_ranking != other.taste_ranking) {
            this.taste_ranking - other.taste_ranking
        } else 0
    }

    override fun toString(): String = this.name
}

val grape = Fruit(1, "Grape", "purple")
val mango = Fruit(5, "Mango", "orange")
val strawberry = Fruit(6, "Strawberry", "red")
val breadfruit = Fruit(-10, "Breadfruit", "beige")

println(grape > mango) //false
println(strawberry > breadfruit) //true
```

## Comparators
To create more nuanced sorting or custom ordering, create a [Comparator](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-comparator/) for the user-defined type.

Using the above `Fruit` type:

```kotlin
// A comparator to compare Fruit by color 
class FruitByColor : Comparator<Fruit> {
    override fun compare(o1: Fruit?, o2: Fruit?): Int {
        if (o1 == null || o2 == null) {
            return 0;
        }
        return o1.color.compareTo(o2.color)
    }
}

val fruits = listOf(grape, mango, strawberry, breadfruit)
rintln(fruits.sortedWith(FruitByColor())) //[Breadfruit, Mango, Grape, Strawberry]
```
