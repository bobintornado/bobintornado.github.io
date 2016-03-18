---
layout: post
comments: true
title:  "Golang Interface & Reflection"
date:   2016-02-14 23:24:36
categories: Golang
---

I think it's worth trying to write just another post on Golang interface, in attempt to serve as a quick introduction to interface and reflection of Golang in simple English.

According to Effective Go:

`Interfaces in Go provide a way to specify the behavior of an object: if something can do this, then it can be used here.`. 

This is pretty much half the story of the interface, and normal every day use of it.

Let's say you have a `generic` interface, which has one method, called `general()`.

{% highlight go %}
type generic interface {
    general()
}
{% endhighlight %}

Then you write a struct which implements the generic interface.

{% highlight go %}
type specification struct {
}

func (s specification) general() {
    log.Println("doing general in a specific way")
}
{% endhighlight %}

Then you could just use instances of specification anywhere as if you are using generic.

This is pretty much the ducking type part of Golang, and personally I think this is similar to `abstract` in Java.

Notice that the "implementation" is implicit, not explicit. But the compiler still checks for the "type safety", and will warn you if something goes wrong.

And another part of the story is how an interface value is stored. 

In Go, the interface has two parts, the interface value and the "actual“ value. With this two parts understanding, a lot of common pitfalls could be avoided.

And the Golang does packing and unpacking for you automatically when necessary.

For example, the following codes demonstrate it. 

{% highlight go %}

func PrintAll(vals []interface{}) {
    for _, val := range vals {
        fmt.Println(val)
    }
}

func main() {
    names := []string{"stanley", "david", "oscar"}
    PrintAll(names)
}

{% endhighlight %}

With this understanding, we could explore reflection in Golang now. 

According to the official Golang blog post [Laws of Reflection](http://blog.golang.org/laws-of-reflection),

>At the basic level, reflection is just a mechanism to examine the type and value pair stored inside an interface variable.

And that is it. 

I believe this is pretty much everything one needs to know at an beginning level.

After this, you may also want to read the following posts. 

1. [Laws of Reflection](http://blog.golang.org/laws-of-reflection)
2. [How to use interfaces in Go](http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go)
3. [Structs and Interfaces](https://www.golang-book.com/books/intro/9)

Happy coding.

Till next time.
