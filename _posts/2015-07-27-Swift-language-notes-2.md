---
layout: post
title:  "Swift language notes 2"
date:   2015-07-27 10:30:00+1000
categories: ios swift
tags: ios swift
---

#### CustomStringConvertible interface
This interface is newly added in Swift 2. It just has one function (description). According to Apple's document, "This textual representation is used when values are written to an output stream, for example, by print."

#### mutating
Structures and enumerations are value types. The properties of a value type cannot be modified from within its instance methods. However, you can change them from within its instance methods by using "mutating" keyword.
{% highlight swift %}
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveByX(2.0, y: 3.0)
{% endhighlight %}

#### precondition
"precondition" can check a necessary condition to detect whether the program can proceed or not even in production code. While "assert" just works in debug mode by default.

#### [:]
If the compiler can infer the type, you can use this syntax to empty the dictionary.
{% highlight swift %}
var placesAndTravelTimes = [Place: [Place: Double]]()
 
 // Mutating function to allow adding of Place
 public mutating func addPlace(place: Place) {
     precondition(placesAndTravelTimes[place] == nil)
     placesAndTravelTimes[place] = [:]
 }
{% endhighlight %}

#### ??
`a ?? b` is equivalent to `a != nil ? a! : b`

#### SequenceType, GeneratorType and IndexingGenerator
SequenceType can be iterated with a for...in loop. GeneratorType encapsulates iteration state and interface for iteration over a sequence. IndexingGenerator is a generator for an arbitrary collection. The example below is from Apple "Exploring San Francisco" demo code.
{% highlight swift %}
public struct PathsView: SequenceType, CustomStringConvertible {
    private let paths: [[Place]]
    
    public typealias Generator = IndexingGenerator<[[Place]]>
    
    private init(paths: [[Place]]) {
        self.paths = paths
    }
    
    public func generate() -> Generator {
        return IndexingGenerator(paths)
    }
    
    public var description: String {
        return "\(paths.count) paths"
    }
}
{% endhighlight %}

#### Hashable
`x == y` implies `x.hashValue == y.hashValue` and Hashable extends Equatable. Instances of this type can be keys of the dictionary.
{% highlight swift %}
extension CLLocationCoordinate2D: Hashable {
    public var hashValue: Int {
        return latitude.hashValue ^ longitude.hashValue
    }
}

public func ==(lhs: CLLocationCoordinate2D, rhs: CLLocationCoordinate2D) -> Bool {
    return lhs.latitude == rhs.latitude &&
        lhs.longitude == rhs.longitude
}
{% endhighlight %}