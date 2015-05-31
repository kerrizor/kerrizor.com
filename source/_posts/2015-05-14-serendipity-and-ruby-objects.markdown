---
layout: post
title: "Serendipity and Ruby Objects"
date: 2015-05-14 10:38:12 -0700
comments: true
categories: 
---
One of the things that I explicitly try to model when I'm teaching people is the idea that There Ain't No Such Thing As An Expert. <!-- more -->There are precious few people who know everything inside and out, and rather than try to puff myself up as some sort of magical repository of wisdom, it's better to model for students how to react to moments of discovering your knowledge isn't perfect, how to turn a surprise into a learning opportunity.

I often do Ruby training classes for groups and individuals, both for those who are new to Ruby as well as those who want to dive deeper into specific topics. During one of these training sessions, I loaded this chunk of Ruby into IRB:

```ruby
class Parent
  def speak
    puts "Pick up your room!"
  end
end

class Child < Parent
end
```

I'm defining a `Parent ` class with one instance method `speak` and a `Child` class that inherits from `Parent`, then I showed the students how method inheritance works in practice.

```ruby
2.2.2 :009 > Parent.new.speak
Pick up your room!
 => nil
2.2.2 :010 > Child.new.speak
Pick up your room!
 => nil
```

Next, I wanted to show how we could redefine the `speak` method for `Child`. As this was a group who, though junior, had previously developed in Java, I also wanted to show off a tiny bit of the Ruby magic, by accomplishing this not by restarting and loading up the objects again, but by reopening the class. Now, normally if I were to do that, I'd type it in like this:

```ruby
2.2.2 :011 > class Child < Parent
2.2.2 :012?>   def speak
2.2.2 :013?>     puts "YOU'RE NOT THE BOSS OF ME!"
2.2.2 :014?>   end
2.2.2 :015?> end
 => :speak
2.2.2 :016 > Child.new.speak
YOU'RE NOT THE BOSS OF ME!
 => nil
```

However, in my excitement to show how cool Ruby is, what I actually did was make a critical typo:

```ruby
2.2.2 :017 > Child < Parent
 => true
```

Wait, what?! Child is /less/ than Parent? That's interesting. Why would it be less than Parent? I was in the middle of a class, and didn't want to derail, so I quickly said "Oh, that's interesting! I want to explore this later, hang on.." As someone who's attention is easily diverted, I've found having the code equivalent of a little notebook to jot down my thoughts, ideas, and questions to help me focus on the problem at hand rather than rabbit holes, so I keep a little folder of notes (backed by git) of interesting things.

So, questionable code captured, I as able to return to it later that evening. What was going on? Now, I know that all objects in Ruby have their own unique ID. Could that be what we're comparing?

```ruby
2.2.2 :021 > Child.object_id
 => 70157984875120
2.2.2 :022 > Parent.object_id
 => 70157984897760
2.2.2 :023 > Child.object_id < Parent.object_id
 => true
```

Possibly, except that I know that `<`, just like `+`, is actually a method.

```ruby
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

```ruby
2.2.2 :030 > Child.object_id.class
 => Fixnum
```

When I'm comparing one class to another, I'm using not some magical concept of "less than" that Ruby is aware of, but I'm calling a method upon the return value of the previous method. Thankfully, Ruby offers us the ability to inquire where a given method is defined.

```ruby
2.2.2 :031 > Child.new.method(:speak).owner
 => Parent
2.2.2 :019 > Child.method(:<).owner
 => Module
```

We see here that `speak` is defined as a method in `Parent` and thus handed down via inheritance to a `Child` instance, and that `<`, as a class method of `Child`, is defined in `Module`.

So what exactly is it comparing? Off to [http://ruby-doc.org](http://ruby-doc.org/) we go! It is a great resource, one that every Ruby developer should have bookmarked. You're not going to remember every single method, or how exactly each method behaves, so being comfortable with this documentation will pay off, and odds are you'll learn something new (or be reminded of something you forgot) every time you browse through it's pages.

Navigating to [the page documenting Module](http://ruby-doc.org/core-2.2.2/Module.html) I found the following:


>**mod < other → true, false, or nil**
Returns true if mod is a subclass of other. Returns nil if there's no relationship between the two. (Think of the relationship in terms of the class definition: “class A<B” implies “A<B”.)

*-- [Documentation for Module#<](http://ruby-doc.org/core-2.2.2/Module.html#method-i-3C)*

For our problem, we see that if the Class or Module on the left side of `<` is a subclass of the Class or Module on the right side, we will get back `true`. If it is NOT a subclass, we will get back `false`, and if they're in no way related to each other, we'll get back `nil`. In the description above, we can see an abstract example to the left of the *→*, and to the right we see the possible outputs. On the actual Ruby-Docs.org website, you can also mouseover the definition to see a link "click to toggle source" which will reveal the actual source code that is run when you call the method. If you're curious -- and I encourage you to be so -- take a minute to open it up and see what's going on. You may not be able to write C, but I wager that you can read it and puzzle out how it works.

So this particular bit of Ruby isn't likely to be a big win for me; I can't imagine a terribly large number of situations in which I'd need this functionality, but for that handful of cases, it is spectacularly useful. 

## "We learned a valuable lesson today"

More than just learning a bit of Ruby trivia, I hope this story shows a little about my process for learning.

### Spot the oddity, and move on
Don't get distracted from the task you were attempting to accomplish. Time box yourself to 5 or 10 minutes, and if you haven't solved why it's behaving the way it is, but have an alternate solution, simply tuck the bizarre code away somewhere for future research.

### Create a hypothesis and experiment
Use the tried and true Scientific Method. Think about the language you're working in normally behaves; what causes similar effects? What might be behavior, given the outcome? The consider how you could test that idea; if the language works the way you think it does, what would happen in the REPL if you changed an input value, or how you worked with the result?

### Read the docs
It's rare to have a language, framework, or library without some kind of documentation. Identify what the best resources are for your particular case - online, desktop app like Dash, command-line man pages, or even documentation that ships inside a REPL. Learn how to read the documentation source you choose, how to search, navigate, and expand definitions, and read it when you're surprised by something, *even when you think you won't understand it.*

### Imagine a reason or use case
Once you've learned how it behaves and played with it (either in a REPL or small mock-up script or application) try to imagine the use case for why this feature was built. What problems were other developers having that the language or framework's core team were attempting to address? Seeing the context of why you'd use a particular feature helps to understand why it behaves the way it does.

### Share the knowledge
Write up a blog post, tweet out a "Did you know..." gist, or even just talk about it with your peers on Slack or IRC. Sure, someone might say "Oh, I can't believe you didn't know that" but people who feign surprise are jerks and you should feel free to ignore this boorish behavior. Besides, no matter how new you are, it's likely that at least one other person DIDN'T know about it or could use the reminder. Furthermore, you're the one who's trying to give back to the community by share the knowledge and teaching, so in the end it's you who is the awesome gold star developer, not them. Own it!



















