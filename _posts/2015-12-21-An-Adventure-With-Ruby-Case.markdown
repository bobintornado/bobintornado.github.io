---
layout: post
comments: true
title:  "An Adventure With Ruby Case"
date:   2015-12-21 11:24:36
categories: Ruby
---

Today I was trying to accomplish a simple task in Ruby with my colleague, which is to \"run different statements based on which group the element belongs to\".

This seems so trivial that we immediately wrote down the following codes without a second thought. 

{% highlight ruby %}
  
group_a = [:symbol_1, :symbol_2, :symbol_3]
group_b = [:symbol_sample, :symbol_example, :symbol_case]
group_c = [:a_random_value, :two_random_values]

case the_element
when group_a.include? the_element
  puts "attack!"
when group_b.include? the_element
  puts "defense!"
when group_c.include? the_element
  puts "smash!"
else 
  puts "retreat!"
end

{% endhighlight %}

But later when we were writing RSpec for our codes, we realized that this snippet of codes always puts out value `retreat`.

We were so confused as we when manually try out `group_a.include? an_element_value`, it does give us `true`.

And we were like 

{% include image.html img="assets/ruby_case/Jackie-Chan-WTF.jpg" title="wtf" %}

So we went online trying to find some examples in order to double confirm we were writing some correct Ruby codes.

So we read the following sample codes from this blog [post](http://www.skorks.com/2009/08/how-a-ruby-case-statement-works-and-what-you-can-do-with-it/).

{% highlight ruby %}
  
print "Enter your grade: "
grade = gets.chomp
case grade
when "A"
  puts 'Well done!'
when "B"
  puts 'Try harder!'
when "C"
  puts 'You need help!!!'
else
  puts "You just making it up!"
end
{% endhighlight %}

Mmmmmm. This seems correct. And our codes also seem correct.

Errr. BUT.

{% include image.html img="assets/ruby_case/wait-a-minute.jpg" title="wait a minute" %}

There seems to be something WRONG here with our expectations of our Ruby codes.

Then I suddenly realized that while I am expecting the `case` to execute the `when` clause where it first return `true`, it is actually ***COMPARING THE VALUE AGAINST THE VALUE WHEN RETURNS***.


It's not `conditions` based here, it's value based! So what happened here for the sample code is actually 

{% highlight ruby %}

print "Enter your grade: "
grade = gets.chomp
if "A" === grade
  puts 'Well done!'
elsif "B" === grade
  puts 'Try harder!'
elsif "C" === grade
  puts 'You need help!!!'
else
  puts "You just making it up!"
end

{% endhighlight %}

So of course our codes will not work! As we were ***comparing symbol with `true` and `false` boolean values, so of course it always run the `else` clause lah!***

And I almost felt that Ruby is doing this to me.

{% include image.html img="assets/ruby_case/Troll.jpg" title="troll" %}

So I went on reading the actual documentation for `case`, where I quoted below from the [documentation](http://ruby-doc.org/docs/keywords/1.9/Object.html#method-i-case).

> Case statements consist of an optional condition, which is in the position of an argument to case, and zero or more when clauses. The first when clause to match the condition (or to evaluate to Boolean truth, if the condition is null) “wins”, and its code stanza is executed. The value of the case statement is the value of the successful when clause, or nil if there is no such clause.

> A case statement can end with an else clause. Each when statement can have multiple candidate values, separated by commas.

I read this twice and finally understand what's going on here. 

***SO THE CONDITION IS OPTIONAL.*** And if I omitted it (condition is null), it will follow my initial expectation, where the first `when` clause evaluated to Boolean truth \"wins\".

{% include image.html img="assets/ruby_case/not_bad.jpg" title="not bad" %}

So I should write the following codes for my case

{% highlight ruby %}
  
group_a = [:symbol_1, :symbol_2, :symbol_3]
group_b = [:symbol_sample, :symbol_example, :symbol_case]
group_c = [:a_random_value, :two_random_values]

# omit condition, leave it null
case 
when group_a.include? the_element
  puts "attack!"
when group_b.include? the_element
  puts "defense!"
when group_c.include? the_element
  puts "smash!"
else 
  puts "retreat!"
end

{% endhighlight %}

And this works!

Oh Yeah.

{% include image.html img="assets/ruby_case/yeah.jpg" title="yeah" %}

What a day.

Happy coding. 

Till next time.
