---
layout: post
title: "Serendipity and Ruby Objects"
date: 2015-05-14 10:38:12 -0700
comments: true
categories: 
---
I often do Ruby training classes for groups and individuals, both for those who are new to Ruby as well as those who want to dive deeper into specific topics. One of the things that I explicitly try to model when I'm teaching people things is There Ain't No Such Thing As An Expert, or TANSTAAX. There are precious few people who know everything inside and out, and rather than try to puff myself up as some sort of magical repository of wisdom, its better to model to student how to react to moments of discovering your knowledge isn't perfect, how to turn a surprise into a learning opportunity.

During one of these training sessions, I loaded up this chunk of Ruby into IRB.

```
class Parent
  def speak
    puts "Pick up your room!"
  end
end

class Child < Parent
end
```

I'm defining a `Parent` class with one instance method `speak` and a `Child` class that inherits from `Parent`, then I showed the students how method inheritance works in practice.

```
2.2.2 :009 > Parent.new.speak
Pick up your room!
 => nil
2.2.2 :010 > Child.new.speak
Pick up your room!
 => nil
```

Next, I wanted to show how we could redefine the `speak` method for `Child`, and as this was a group who primarily developed in Java, also show off a tiny bit of the Ruby magic, but redefining it by reopening the class. Now, normally if I were to do that, I'd type it in like this:

```
2.2.2 :011 > class Child < Parent
2.2.2 :012?>   def speak
2.2.2 :013?>     puts "YOU'RE NOT THE BOSS OF ME!"
2.2.2 :014?>     end
2.2.2 :015?>   end
 => :speak
2.2.2 :016 > Child.new.speak
YOU'RE NOT THE BOSS OF ME!
 => nil
```

However, in my excitement to show how cool Ruby is, what I actually did was make a critical typo:

```
2.2.2 :017 > Child < Parent
 => true
```

Wait, what?! Child is /less/ than Parent? That's interesting. Why would it be less than Parent? I was in the middle of a class, and didn't want to derail, so I quickly said "Oh, that's interesting! I want to explore this later, hang on.." and copied the code into a tiny git repo I keep called `sandbox-journal`. As someone who's attention is easily diverted, I've found having the code equivalent of a little notebook to jot down my thoughts, ideas, and questions to help me focus on the problem at hand rather than rabbit holes.

So, questionable code captured, I later returned to this problem that evening. Now, I know that all objects in Ruby have their own unique ID. Could that be what we're comparing?

```
2.2.2 :021 > Child.object_id
 => 70157984875120
2.2.2 :022 > Parent.object_id
 => 70157984897760
2.2.2 :023 > Child.object_id < Parent.object_id
 => true
```

Possibly, except that I know that `<`, just like `+`, is actually a method.

```
2.2.2 :024 > 2.+(3)
 => 5
2.2.2 :025 > 2 + 3
 => 5
2.2.2 :026 > 2.+(3)
 => 5
2.2.2 :027 > 2 < 3
 => true
2.2.2 :028 > 2.<(3)
 => true
2.2.2 :029 > Child.object_id.<(Parent.object_id)
 => true
```

It is only a syntactic convenience that we're able to use these operators in a more human friendly manner. So, when I'm testing `Child.object_id < Parent.object_id` my use of `<` is a method on the object returned by `object_id`, which in this case if a FixNum.

```
2.2.2 :030 > Child.object_id.class
 => Fixnum
```

When I'm comparing one class to another, I'm using not some magical concept of "less than" that Ruby is aware of, but I'm calling a method upon the return value of the previous method. Thanksfully, Ruby offers us the ability to inquire where a given method is defined.

```
2.2.2 :031 > Child.new.method(:speak).owner
 => Parent
2.2.2 :019 > Child.method(:<).owner
 => Module
```

We see here that `speak` is defined as an instance method in `Parent` and thus handed down via inheritance to a `Child` instance, and that `<` as a class method of `Child` is defined in Module. So what exactly is it comparing? Off to [http://ruby-doc.org](http://ruby-doc.org/) we go! It is a great resource, one that every Ruby developer should have bookmarked. You're not going to remember every single method, or how exactly each method behaves, so being comfortable with this documentation will pay off, and odds are you'll learn something new (or be reminded of something you forgot) every time you browse through its pages.

Navigating to [the page documenting Module](http://ruby-doc.org/core-2.2.2/Module.html) I found the following:


>**mod < other → true, false, or nil**
Returns true if mod is a subclass of other. Returns nil if there’s no relationship between the two. (Think of the relationship in terms of the class definition: “class A<B” implies “A<B”.)

*-- [Documentation for Module#<](http://ruby-doc.org/core-2.2.2/Module.html#method-i-3C)*

For our problem, we see that if the Class or Module on the left side of `<` is a subclass of the Class or Module on the right side, we will get back `true`. If it is NOT a subclass, we will get back `false`, and if they're in no way related to each other, we'll get back `nil`. In the description above, we can see an abstract example to the left of the *→*, and to the right we see the possible outputs. On the actual Ruby-Docs.org website, you can also mouseover the definition to see a link "click to toggle source" which will reveal the actual source code that is run when you call the method. If you're curious -- and I encourage you to be so -- take a minute to open it up and see what's going on. You may not be able to write C, but I wager that you can read it an puzzle out how it works.

So this particular bit of Ruby isn't likely to be a big win for me; I can't imagine a terribly large number of situations in which I'd


















