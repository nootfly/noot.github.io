---
layout: post
title:  "Swift language notes"
date:   2014-07-16 09:47:14+1000
categories: ios swift
tags: ios swift
---
This blog would record my study notes of Swift language. All codes are tested on Xcode 6 - beta3 playgorund. Some are excerpt From: Apple Inc. “The Swift Programming Language.” iBooks.

#### Define int array
     var list:[Int] = []
     var list2 = [Int]()

#### For loop change
    for 1..3

change to

    for 1..<3
        
##### Funcitons
Functions can also take a variable number of arguments, collecting them into an array.
         
    func sumOf(numbers: Int...) -> Int {
       var sum = 0
       for number in numbers {
        sum += number
       }
       if numbers.count > 0 {
        return sum / numbers.count
       } else {
       return sum
      }
    }
    sumOf()
    sumOf(10, 20, 30)
    
Functions are a first-class type. This means that a function can return another function as its value.

    func makeIncrementer() -> (Int -> Int) {
      func addOne(number: Int) -> Int {
        return 1 + number
       }
      return addOne
    }
    var increment = makeIncrementer()
    increment(7)
    
A function can take another function as one of its arguments.

    func hasAnyMatches(list: Int[], condition: Int -> Bool) -> Bool {
       for item in list {
         if condition(item) {
            return true
         }
       }
      return false
    }
    func lessThanTen(number: Int) -> Bool {
    return number < 10
    }
    var numbers = [20, 19, 7, 12]
    hasAnyMatches(numbers, lessThanTen)

#### Array sort
    var array = [1, 5, 3, 12, 2]
    array.sort {$0 > $1}
    
#### Struct VS Class
One of the most important differences between structures and classes is that structures are always copied when they are passed around in your code, but classes are passed by reference.


#### Protocol
Classes, enumerations, and structs can all adopt protocols.

     protocol ExampleProtocol {
          var simpleDescription: String { get }
          mutating func adjust()
     }

    enum Suit: ExampleProtocol {
      var simpleDescription:String {
        get {
          return "test"
        }
      }
      case Spades, Hearts, Diamonds, Clubs

      func color() -> String {
        switch self {
        case .Spades, .Clubs:
            return "black"
        case .Diamonds, .Hearts:
            return "red"            
        }
     } 
     func adjust()  {
         print("adjust")
     }
    }
    
Use extension to add functionality to an existing type, such as new methods and computed properties. 
  
    extension Int: ExampleProtocol {
    var simpleDescription: String {
    return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
    }
    7.simpleDescription”
    
    extension Double {    
    var absoluteValue:Double {
       if self < 0  {
         return -self
       } else  {
         return  self
        }
      }
     }

     (-3.12).absoluteValue
     3.12.absoluteValue




 

 