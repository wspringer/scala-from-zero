
=============
 Before Zero
=============

You just installed Scala. Let's see where we are. There's a bunch of
tools in ./bin. The obvious choice to start playing around with scala
would be to start the scala command, but if Scala is anything like
Java, then you would need to pass a 1) classpath, and 2) the fully
qualified name of a class with a static main method. 

That sounds a little frightening. We want to start from scratch, and -
assuming that Scala works the same as Java - I would now have to first
understand how to define a class, how to make sure that class is
'executable' and last, and then compile it. That seems so complicated! 

But hold on. This is Scala, not Java. Perhaps Scala is different. The
easiest way to learn about its expectations is just fire up scala from
the commandline and see what it expects.

::

  Welcome to Scala version 2.8.0.final (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_15).
  Type in expressions to have them evaluated.
  Type :help for more information.

  scala>

Hold on now. This is different. Instead of telling me what pass as
commandline options, it now shows me some sort of prompt. That means
there is something I can type. Let's try that. Let's start from 

Zero
====

Let me just type zero and see what happens:

::

  scala> 0
  res0: Int = 0

It's a bit puzzling, but it somehow told me some things on the thing I
typed. I don't know about the res0, so I will ignore that for now. But
the Int is unmistakenly referring to its type, and the zero clearly is
its value. Typing enter a couple of extra times is not giving me
anything else. 

So what do I learn from that. I typed something. Scala told me about
the type. Apparently Int is a type. Let me see if that holds. 

One, Two, Three, Ints
=====================

::

  scala> 1
  res1: Int = 1

  scala> 2
  res2: Int = 2

  scala> 3
  res3: Int = 3

Typing one two three gives basically the same. The 'res<whatever>'
things are also getting incremented? Let me try that. 

::

  scala> 396
  res4: Int = 396

Hmmm, so apparently, there is no direct relationship between that
'res'-thing over there and the number. Still puzzled, but it doesn't
seem to get in the way, so I will leave it there for now. But if I
have integer numbers, then I might have operators as well. Let's find
out. 

Operators
=========

::

  scala> 2 + 3  
  res5: Int = 5

Hey, that's nice! Scala is a calculator. I can just throw bc away, and
start using Scala as of now. That is... if it supports other operators
as well. Let's try some others.

::

  scala> 2 * 3
  res6: Int = 6

  scala> 4 / 2
  res7: Int = 2

  scala> 9 % 2
  res8: Int = 1

  …

  scala> 4 ^ 2
  res28: Int = 6

Hold on, that's not quite what I expected. I mean, surely Java doesn't
have the power operator defined for ints, but some language have, and
define it like this. On second thought, Scala *does* look a little
like Groovy, so perhaps Groovy notation works here:

::

  scala> 4 ** 2
  <console>:6: error: value ** is not a member of Int
         4 ** 2
         ^

O dear, this is even worse. Let's try to decode it. ** is not a member
of Int. A member? Since when do primitives have members. Java doesn't
define members on integers, unless it would be on the boxed type. But
this Int wasn't boxed, because otherwise it would not have supported
the operators we used, or would it?

This is what will happen if you are trying to frame Scala in your Java
mental modal. You will have false assumptions. Let's stay calm and
start all over again, and forget about all we know about Java.

First of all, Scala tells us ** is not a member, so it follows Ints
have members. It also means that there *are* other members defined for
Int. Now if I only could find out about those other 

Methods on Int
==============

I am going to forget about the operators for now and focus on the
*members* mentioned by the Scala REPL. How I do find about members
defined on Int? If this were an IDE, I would type the number, then
type a dot and hit CTRL-Space or TAB. 

::

  scala> 234.

  !=             ##             %              &              *
  +              -              /              <              <<
  <=             ==             >              >=             >>
  >>>            ^              asInstanceOf   equals         hashCode
  isInstanceOf   toByte         toChar         toDouble       toFloat
  toInt          toLong         toShort        toString       unary_+
  unary_-        unary_~        |

Hey! That actually works. It *does* support autocompletion. This is
great. So what do we have here? That's funny. It lists all of the
operators again. That seems to suggest… It seems to suggest that
operators are *members*? This one needs some time to sink in. Could it
be that everything we did so far was calling methods on objects of
type Int?

Let's try that. I'm typing a + and press TAB again.

::

  scala> 234.+    

  def +(Byte): Int        def +(Char): Int
  def +(Double): Double   def +(Float): Float     def +(Int): Int
  def +(Long): Long       def +(Short): Int       def +(String): String

Now that's interesting. We haven't seen anything like this yet, but
the def keyword does remind me of a couple of other programming
languages. The suggestion here seems to be that the + 'member' or
operator accepts varies types of arguments. This I got to see.

::

  scala> 234.+(12)
  res31: Double = 246.0

Hey it works! Well. Kind of. It's also a little strange. I thought I
was just adding 12 to the 234 of type Int I had before. But what I get
is a Double.

Double
======

Getting a Double is interesting in a way, because all I had so far
where Ints, and at least Scala supports floating point numbers as
well. Nevertheless, what I got was not quite what I expected. Let me
try to read that again. Could it be that just typing 234. is giving me
a Double?

:: 

  scala> 234.
  res33: Double = 234.0

And it does. In fact, trying this gives Doubles as well.

::

  scala> 0.234
  res34: Double = 0.234

But this doesn't:

  scala> .23
  <console>:1: error: ';' expected but double literal found.
         res34.23

Now I am even more confused. It seems it's interpreting .23 as a
member of the result of the previous call. (Did I say result? Could it
be that this is the reason I'm seeing res... all over the place?)

Perhaps I want to much. Perhaps I should not worry too much about a
shortened notation for now. I am able to type Double literals. Let's
worry about syntactic sugar later. In fact, let's make that a new
principle. 

.. pull-quote::

   We are happy with the full notation first, and worry about
   shortened notation later.

That makes this problem go away for a while. But we will back to it
later on. I am sure of it. 

In fact, I don't want to get to distracted. If you remember, I was
actually trying to figure out what these *members* on Int were. It
seems that when I thought I saw a listing of all members of Int, all I
got was a list of members on Double. I don't want to get stuck on this
for now. I am way more interested in exploring these *members* defined
for numbers a little more. One that drew my attention in particular is
that comparisson operators also seem to be defined as methods, as
suggested by listing <, >, <= and >= in the autocompletion
suggestions.

Comparisson Operators
=====================

Here's my first experiment:

::

  scala> 4.>(3)
  res41: Boolean = true

Nice, a couple of things learned. More to add to my stack of things I
want to explore. First of all, it returns something of type
Boolean. We hadn't seen that before. But it seems to suggest that
comparisson operators are just methods. 

In fact, when you come to think about it, *all* of the operators we
have seen so far are just methods on the objects, and it seems having
the ability to drop the dot and parentheses is just syntactic
sugar. Yet another thing to put on my stack.

:: 

  scala> 3.<=(3)
  res43: Boolean = true

  scala> 3.>=2  
  res44: Boolean = true

  scala> 3.==(2)
  res45: Boolean = false

Cool. In hindsight, I do see something funny on line two. I
accidentally omitted the parantheses and it still worked. Yet another
thing to put on my list of things I need to explore a little further. 

String
======

One of the operation defined on Double was toString::

  scala> 45.toString()
  res46: java.lang.String = 45

So String is a type as well. In fact, in this case it's good old
java.lang.String. That's a familiar face. It makes you wonder if you
can also just type it as a literal. 

::

  scala> "literal"
  res47: java.lang.String = literal

Same thing. It's interesting to note that while printing it, it ommits
the double quotes, but it all makes sense. For Double's, we found out
that you can list all of its *members* by adding a . and then hit
TAB. Let's try the same thing for String::

     scala> "literal".

     +                     asInstanceOf          charAt
     codePointAt           codePointBefore       codePointCount
     compareTo             compareToIgnoreCase   concat
     contains              contentEquals         endsWith
     equalsIgnoreCase      getBytes              getChars
     indexOf               intern                isEmpty
     isInstanceOf          lastIndexOf           length
     matches               offsetByCodePoints    regionMatches
     replace               replaceAll            replaceFirst
     split                 startsWith            subSequence
     substring             toCharArray           toLowerCase
     toString              toUpperCase           trim

It's basically everything defined by java.lang.String. Nothing new
here yet.

 

