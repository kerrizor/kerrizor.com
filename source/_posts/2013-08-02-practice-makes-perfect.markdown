---
layout: post
title: "Practice Makes Perfect"
date: 2013-08-02 14:58
comments: true
categories: [Career, Education, Training]
---
*This post originally appeared on the [Blue Box blog](http://www.bluebox.net/about/blog/2012/06/practice-make-perfect/)*

Contrary to popular belief, there is no such thing as “on the job training.” The skills that differentiate senior and lead developers from the herd don’t come from sheer number hours spent simply doing the job. You will pick up a lot of exposure to concepts, techniques, and processes that way, but without focused, deliberate practice, you’re just spinning your wheels.

<!-- more -->

When you’re working, the goals are to accomplish the task, avoid failure, and get things done. Space to truly engage in learning is incidental. You are in the closed mode; “don’t distract me, I’m on a mission!” Practice, on the other hand, is about working on a problem with the idea of exploring the problem area, of learning something, and your goal, oddly enough, is to Deliberately Fail.

At Blue Box Group, we have the usual geek clubhouse toys — a MAME cabinet with 1200+ video games, ping-pong, a kegerator (with two varieties of beer!), and a rather nice billiards table, which is our primary obsession these days. Not content with the classic pool hall games of 8-ball, 9-ball, or cut-throat, we play a peculiar game invented by our Principal Technologist we call “Calvin Pool.” The rules are simple – after the break, the person whose turn it is has to attempt to simply hit whichever ball on the table their opponent chooses with the cue ball. There are a few wrinkles, naturally, but more or less the game boils down to “try to make the most difficult shot possible.”

I can’t even begin to tell you how much my game has improved in the last few weeks. I’m making shots that long stymied me, shots that intimidate and impress people who are occasional players of the game. The practice of setting myself up to fail and practicing the difficult, exploring the various possible approaches to seemingly impossible problems, has made me confident when dealing with the easy obstacles.

Similarly, when practicing programming, you need to challenge yourself to learn through exploration and trials, and accept that, as often as not, failure is going to be your ultimate outcome. If you’re not failing, you’re not practicing – you’re just warming up. So, how does someone practice the art and skill of programming? Here are a couple ideas.

### Limit yourself

Decide for an afternoon, a day, or even a week, you’re going to use a technique you’re unfamiliar with. Make life hard for yourself on purpose.

  + MONKEY PATCH ALL THE THINGS!
  + Make everything a gem
  + TDD the living snot out of the next bug you fix
  + Use an IDE you don’t normally use for a month
  + Don’t use the letter ‘G’
  + Make all actions happen through observers

By forcing yourself to find new patterns to solve your problems, you’ll quickly run into the current limits of your knowledge and experience, and push out past them.

### Have some fun while doing something dumb!

Extend your practice by choosing a simple problem and keep solving it in new ways. Each time, add complications and obstructions. Solve a problem over and over, with new obstacles. For instance, let’s take this User Story as a place to start:

“The user will be able to convert a temperature in Celsius to Fahrenheit, and vice versa.”

Simple enough. Five minutes (or less) of Googling will turn up a myriad of solutions, and give you the basic math. Start with making it a small command-line app – easy-peasy lemon-squeezy! Once that’s done (you did tests, too, right?), let’s mutate it and iterate:

  + Make it a gem
  + Release it on Github
  + Make it a website
  + Make it a mix-in on Numeric
  + Make it a web service
  + Find other temperature scales you could convert to and from
  + Have it attempt to resolve the user’s location via browser location or IP address, and give automated temperature conversions
  + Have it query wikipedia (or other data source) to offer information about the resulting temperature – trivia, facts, elements that melt or ignite or freeze at that temp, etc
  + Have it record the average, median, and min/max the user has calculated, and say witty things as milestones are passed
  + …

Practice doesn’t have to be about flash cards and rote memorization. Try a little learning-by-doing, by applying concepts and techniques you want to try to your daily workflow but haven’t found the time for. Get your hands dirty and stretch outside your comfort zone, and approach previously solved and well-understood problems from a fresh perspective, to truly discover the benefits (and drawbacks) of approaches you’re unfamiliar with.
