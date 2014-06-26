---
layout: post
title: "/Regular Expressions in Ruby/"
date: 2014-06-25 20:18:27 -0400
comments: true
categories: Flatiron Regex Ruby Regular Expressions Beginner Matchdata Substitution
---



#Quote of the day:#
> "Some people, when confronted with a problem, think 'I know, I'll use regular expressions.'
Now they have two problems. " - [**Jamie Zawinski**](http://en.wikipedia.org/wiki/Jamie_Zawinski "Jamie Zawinski's Wikipedia Page")  

Hey everyone, my name is **Simon Seroussi** but a lot of my American friends call me **French Fry** (hence the blog's title). As a lack of any fitting alternative (french toast and french needle don't necessarily display a better portrait), I've proudly rolled with it.  

I'm currently enrolled at the [**Flatiron School**](http://flatironschool.com "Flatiron's School Website"), an amazing 12-weeks intensive **coding bootcamp** in New York City's financial district.  

This blog will be the extension of a broader personnal website .. but you'll have to wait a little more for that.
Meanwhile, feel free to check out my technical, less technical and non-technical posts on this blog powered by    [**octopress**](www.octopress.org "Octopress, A blogging framework for hackers.")!  

This first short blog post will focus on **Regular Expressions** and my tips to **write** and **use** those beauties. 

-----

#What are they?#
_A regular expression provides a concise and flexible means to "match" (specify and recognize) strings of text, such as particular characters, words, or patterns of characters. Common abbreviations for "regular expression" include regex and regexp._  
- From [**Codecademy**](http://www.codecademy.com/fr/courses/javascript-intermediate-en-NJ7Lr/0/1 "Codecademy Website")  

Interestingly, they're present in almost every programming language (some even, like Perl, rely entirely on them) but are too often over-used, wrongly used or not used at all when they could in fact make pattern searching, and even pattern replacement, **waaaaaayyyyyyyyyy** easier! 
I'll only go in the basics.


Regular expressions in their basic usage are pattern finders. An easy one looks like that..  
    
    /^Ruby\s{1}+[i]{1}[s]{1}\s{1}amazing$/

Can you guess what string would that expression match? 

This expression would only match the string **Ruby is amazing**, nothing else. 
Remember, Regex need to be written with case and space sensitivity in mind!   

They can be daunting at first... but if you give them enough time and attention, they reveal themselves to be **fun to write and read**.  

**I promise.**  

Not convinced? Follow me through this short exercice. 

-----
#E-mails Checker: Regex in 3 steps#

In a recent lab, [**Regex_rally-ruby-005**](https://github.com/flatiron-school-students/regex_rally-ruby-005 "Flatiron's Github Repo"), students from the [**Flatiron School **](http://flatironschool.com/ "Flatiron School Website") were asked to write a regular expression that only matches given valid email addresses. 

We hold implicitly the instructions to construct our Regular Expression through a list of e-mails listed in our *specs* file.  

```
match = %w{steven@flatironschool.com john.doe@example.com phil@example.co.uk }

do_not_match = ["steven@flatironschool", "user at example.com", "user@example.com@example.com"]
```

Let's make those 'match' requirements a little more complex by taking integers into account: 

```
    match = %w{steven@flatironschool.com john.doe@example.com phil@example.co.uk simon123@gmail.com}
```

###Step 1.
The first important step is to write the opening and closing forward slashes for our regualar expressions (le **DUH**) as follows : 

    / /

To avoid ensuing confusions and errors, **Divide and Conquer**. Don't just stare at the e-mail adresses and try coming up with the right options by glancing at Regex documentation. In other words I'd comment out all the requirement your regular expression needs to take into account.  


    #always:  
    starts with a string,  
    only one '@',   
    includes a '.',  
    ends with a string.**  

    #sometimes:  
    1. includes another '.'  
    2. include integers  
    3. can include another string after "."  
    4. can include another string ('.co.uk') before the ending string.**  

    #never:  
    1. includes spaces. ** 


###Step 2.
Personally, I like writing down what the regular expression needs to do in more _'normal'_ terms.  

Let's face it, regular expressions can be cryptic, to say the least. Just try to narrate a regex at loud and look at the face of people around you.   

They probably think you just insulted them... **Stop reading this and run**! 

![alt text](http://38.media.tumblr.com/09a61fffc9c8c7dbb8e5bb0e9e84e10d/tumblr_mt3xaycoFe1ser43yo1_1280.png "Capitaine Haddocks Insultes")  

In this particular example, I would write something like...  

    string(integers)(.)(_)(integers)(string)@{1}(string).(string)(.)string 

In my 'syntax', I like to put in parenthesis the matches that are optional ('sometimes' in our notes) and the mandatory ones ('always') without parenthsis.  

In curly brackets '{}', I like to include the digit for elements that require to appear only a certain number of times. In this case, we only want '@' to come up once.  


###Step 3.
Now, we can finally start writing our regular expression!  

We want it to always begin with a string. We'll therefore use the option ```\A``` (**start of string**) followed by the expression pattern ```[a-z]``` (**any single character in the range a-z**). With regex, we then need to precise the number of characters we want to match. Here, we need AT LEAST one, expressed by the option ```a+```(**one or more of a**), **a** being the expression being numbered, here ```\A[a-z]```.

    /\A[a-z]+/

We want our regex to match e-mail addresses that possible have numbers or dots (**or both!**) after the initial string. However, we do not want any white space. We will use a backward slash to avoid matching white spaces (_a regex option (x) can also make your regex ignore whitespace, but we'll see this in another post_).  

For digits, we use the pattern ```\d``` (**any digit**).
For the dot, we'll simply write a ```.``` preceded by a backward slash. The dot alone is in fact an option in regex, in means "any single character".. without a backward slash indicating that we're matching a dot, you could fall in a rampant matching spree you probably want to avoid. 

Our digits and dot need to remain optional, so we'll use this option ```a*``` (**zero or more of a**). 

    /\A[a-z]+\d*\.*


We then want another optional string, and then an ```@``` that can only be matched once. For that we use ```a{3}``` (**exactly 3 of a**). Here, ```@{1}```.

    /\A[a-z]+\d*\.*[a-z]*@{1}/

Now, we want another optional string (for **email provider** i.e: gmail), a mandatory dot (**.**com), a mandatory string (**com**), an optional dot and optional string (.com**.uk**).


    /\A[a-z]+\d*\.*[a-z]*@{1}[a-z]+\.[a-z]+\.*[a-z]*/

Ultimately, we want to indicate the regex that whatever string we match, it is ending now. Nothing else should be matched after that. We use this expression pattern ```\z```, meanin end of string.

    /\A[a-z]+\d*\.*[a-z]*@{1}[a-z]+\.[a-z]+\.*[a-z]*\z/


This takes some time, but I found it to be a pretty safe method to write regular expressions in one try...
without having to constantly return to your code and trying to debug it, often only making it worse. 

![alt text](http://gocomics.typepad.com/.a/6a00d8341c5f3053ef0192ab478d7e970d-pi "Repetitive mistakes") 

-----

#How to use them#

###MatchData Objects:###
Remember, regular expressions are merely instances of the Regex Class. In this sense, there are merely objects, and can be treated as sort!  

We can gather sort out data from a string into different objects by wrapping them in different parenthesis.  

In **IRB**: 
``` irb
my_string = "SimonOnRegex: Grouping Like a Boss".match(/(^[a-z]*)(:)(.*)/i)
```

would give us: 
``` irb
     => #<MatchData "SimonOnRegex: Grouping Like a Boss" 1:"SimonOnRegex" 2:":" 3:" Grouping Like a Boss"> 
```

We can then use the ```[]``` square bracket method and pass it parameters to access each grouping. 

``` ruby
my_string[1] # => "SimonOnRegex"  
my_string[2] # => ":"
my_string[3] #  => " Grouping Like a Boss" 
``` 

We can assign them variables, using the ```.captures``` method. 

my_string = "SimonOnRegex: Grouping Like a Boss"
one, two, three = my_string.match(/(^[a-z]*)(:)(.*)/i).captures
 
``` ruby
p one   #=> "SimonOnRegex"
p two   #=> ":"
p three   #=> " Grouping Like a Boss" 
```

-----

###Substitutions:###
Designed to manipulate strings, regex are extermely popular to substitution. 

``` ruby
"flatiron district".sub /district/, "school" # => "flatiron school"
```

You might be thinking: _"this is useless, I can use a regular gsub and get the same result without bothering about all the annoying regex syntax!!"_
And you are right.

``` ruby
"flatiron district".sub "district", "school" # => "flatiron school"
```

Imagine however, if you could combine the power of grouping with substitutions?  

**You can!**

You have that ugly string of digits: "1234567890" ... and you want a beautiful, formatted phone number: "(XXX) XXXX-XXX"

Ring a bell?

``` ruby
my_phone = 1234567890
my_phone.to_s.sub /(\d{3})(\d{3})(\d{4})/, '(\1) \2-\3'
# => "(123) 456-7890"  
```
Here, we're simply grouping the three first digit (under default variable \1) and putting them in parenthesis, the three next digits (\2) preceding them by a space and adding a dash after them, and then the last four digit. 

This is simply beautiful. And it actually makes a lot of sense!

-----

###Now go save the day with Regex!

![alt text](http://imgs.xkcd.com/comics/regular_expressions.png "Regex Savior")  

[**xkcd, A Webcomic of Romance, Sarcasm, Math, and Language**](ttps://xkcd.com/ "xkcd Website")   

___


##Useful Ressources:##
[**Rubular**](http://rubular.com/ "Rubular Website"), a Ruby regular expression editor.  
[**Rubyxp**](http://rubyxp.com/ "Rubyxp Website"), a nice alternative to Rubular.   
[**Rexegg**](http://www.rexegg.com/ "Rexegg Website"), a great website dedicated to regex tutorials  
[**Regular-Expressions.info**](http://www.regular-expressions.info/ "Regular-Expressions.info Website"), another one, because why the fuck not.   
