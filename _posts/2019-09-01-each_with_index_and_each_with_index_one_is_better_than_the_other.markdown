---
layout: post
title:      "each_with_index and each.with_index: one is better than the other. "
date:       2019-09-02 02:11:26 +0000
permalink:  each_with_index_and_each_with_index_one_is_better_than_the_other
---


Ruby is currently (and probably will continue to be) my favorite programming language. Mats did a beautiful thing for everyone in the community I now belong to. There are so many "higher" level methods and you really cannot know them all by memorization. But in ruby most of time if you just guess It will probably work. I wanted to reiterate over a few cool little things I encountered here at Flatiron. The first is the subtle but useful difference between each_with_index and each.with_index. First the one that I think is less cool (as you have less control) is some_array.each_with_index. Here you pass in a block and you extract each element of the array and it has a corresponding index. So ```some_array = ["Randy", "Kermit", "Ms Piggy", "Burt", "Ernie"]  some_array.each_with_index{ |element,index| puts "Yo, Im #{element} at the #{index} position"} #=> "Yo, Im Randy at the 0 position" ``` 
However each.with_index is totally cooler because you get to determine where the index starts. This is actually useful as arrays start at 0 but as humans we start at 1. So as I'm obviously about to show you (I'm sure you already guess it) each.with_index will be like such ```some_array.each.with_index(1){ |element, index| puts "Yo, Im #{element} at the #{index} position"}``` #=> "Yo Im Randy at the 1 position" Better? Better because Its more human readable. 
