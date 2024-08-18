+++
title = 'The Curious Case of Python Class Attributes'
date = 2024-08-17T15:07:58-05:00
+++

# The Curious Case of Python Class Attributes

For most of my career I've worked with statically typed languages, most often C#. Recently, I've been exploring Python, a dynamically typed language with it's own set of features and behavior that differ from what I'm familiar with. I recently ran into one of these differences and spent quite a long time diving into the Python language to figure out what was going on.

# C\#

If like me, you come from a statically typed background you're probably used to your classes looking a little something like this:

```C#
public class Person {
	public string name;
	public string quidditchPosition = "None";
	public List<string> favoriteColors = new List<string>();

	public Person(string name) {
		this.name = name;
	}

	public void AddFavoriteColor(string color){
		favoriteColors.Add(color);
	}

	public void SetPosition(string p){
		this.quidditchPosition = p;
	}

	public void Print(){
		Console.WriteLine($"{this.name}'s favorite colors are {String.Join(" and ", this.favoriteColors)} and he plays {this.quidditchPosition}");
	}
}
```

Essentially, declaring your instance variables with their types at the top of the class. Then in your constructor and methods utilize those variables. If we write a quick script to use this class we get the following output, as expected.

```C#
public class Program
{
	public static void Main()
	{
		var harry = new Person("Harry");
		var draco = new Person("Draco");
		harry.Print();
		draco.Print();

		harry.SetPosition("Seeker");
		harry.AddFavoriteColor("Red");
		harry.AddFavoriteColor("Gold");

		draco.AddFavoriteColor("Green");
		draco.AddFavoriteColor("Silver");

		harry.Print();
		draco.Print();
	}
}

Harry's favorite colors are  and he plays 
Draco's favorite colors are  and he plays 
Harry's favorite colors are Red and Gold and he plays Seeker
Draco's favorite colors are Green and Silver and he plays None
```

# Python

Now if we translate our class into Python, we come up with this:

```Python
class Person:
    name = ""
	quidditch_position = "None"
	favorite_colors = list()

    def __init__(self, name):
        self.name = name

    def add_favorite_color(self, color):
        self.favorite_colors.append(color)

    def set_position(self, position):
        self.quidditch_position = position

    def __str__(self):
        return f"{self.name}'s favorite colors: {self.favorite_colors} and he plays {self.quidditch_position}"
```

Translating our script gives us the following output:

```Python
if __name__ == '__main__':
    harry = Person("Harry")
    draco = Person("Draco")

    print(harry)
    print(draco)

    harry.add_favorite_color("red")
    harry.add_favorite_color("gold")
    draco.add_favorite_color("green")
    draco.add_favorite_color("silver")

    harry.set_position("Seeker")

    print(harry)
    print(draco)

Harry's favorite colors: [] and he plays
Draco's favorite colors: [] and he plays
Harry's favorite colors: ['red', 'gold', 'green', 'silver'] and he plays Seeker
Draco's favorite colors: ['red', 'gold', 'green', 'silver'] and he plays None
```

What in the name of Godric Gryffindor is going on here?? Why do Harry AND Draco have each others favorite colors??

This is because in Python, **variables assigned in the class body but outside a method are class attributes**. This work similarly to **static variables** in languages like C#. Essentially, **class attributes are shared amongst all instances of a class**. Our variable `favorite_colors` is actually defined on the class itself, rather than an instance of the class. This is why Harry and Draco's favorite colors are intermingled. We can see this a little better with this code snippet

```Python
print(Person.favorite_colors)
```

or better yet

```Python
# Our Person class has variables name, quidditch_position, and favorite_colors on the class
print(Person.__dict__)
{'__module__': '__main__', 'name': '', 'quidditch_position': 'None', 'favorite_colors': ['red', 'gold', 'green', 'silver'],...}
```

So why are we able to access it using `self.favorite_colors`? Well, this is a Python behavior that differs from C# static variables. When resolving a attribute it will first look and see if there is an instance attribute defined, if not, then it will check if there's a class attribute defined. In this case there is, so that's what it uses.

So what about about our string `quidditch_position`? Why is it not being shared between Harry and Draco? Another quirk of Python is that **you can declare instance attributes from any method, not just the constructor**. If you take a look at our `set_position` function, you can see that we create an instance attribute in this case by executing `self.quidditch_position = position`. So now we have BOTH and instance attribute and a class attribute. However, with our `favorite_colors` list, we never actually initialize it within a method; we just utilize it with `self.favorite_colors.append(color)`, so it uses the existing class attribute. If instead we updated our `add_favorite_color` method to the following, we would have avoided the problem altogether.

```Python
def add_favorite_color(self, color):
    if not self.favorite_colors:
        self.favorite_colors = list()
    self.favorite_colors.append(color)

Harry's favorite colors: ['red', 'gold'] and he plays Seeker
Draco's favorite colors: ['green', 'silver'] and he plays None
```

While this works, it is not very "Pythonic". Instead, with Python, there is no need to declare your variables at the beginning of your class. This is a dynamically typed language after all! Instead our original code should have been:

```Python
class Person:
    def __init__(self, name):
        self.name = name
        self.quidditch_position = ""
        self.favorite_colors = list()

    def add_favorite_color(self, color):
        self.favorite_colors.append(color)

    def set_position(self, position):
        self.quidditch_position = position
```

This leaves us with NO class attributes, only instance attribute, which is what we wanted all along.

# Getting Weirder with Dataclasses

Python 3.7 comes with a new features called `dataclasses`. Essentially, dataclasses are a way to avoid some of the boilerplate code required to create classes that are defined by their data rather than reference. This is similar to the Domain Driven Design concept of Value Objects, but to understand this better take a look at [this article](https://realpython.com/python-data-classes/). While the concept is powerful, it muddies the water between class attributes and instance attributes, since you no longer have a constructor. Take for example the following dataclass:

```Python
@dataclass
class DataClassPerson:
    name: str
    quidditch_position: str = "None"
```

At first glance, you might think these are both class attributes since they are declared outside a method body. However, you would be mistaken. Earlier we defined a class attribute as **variables assigned in the class body but outside a method**. Looking at our example again, we don't actually assign anything to name, so school is a class attribute and name is not. Let's take a closer look at this in the code.

```Python
# Our DataClassPerson has annotations for name and quidditch_position, but doesn't actually have a variable
print(DataClassPerson.__dict__)
{'__module__': '__main__', '__annotations__': {'name': <class 'str'>, 'quidditch_position': <class 'str'>}, 'quidditch_position': 'None',...}
```

Now if we write a script to utilize this new class, we can see a weird quirk of dataclasses.

```Python
if __name__ == '__main__':
    viktor = DataClassPerson("Viktor", "Seeker")
    hermione = DataClassPerson("Hermione")
    print(viktor.__dict__)
    print(hermione.__dict__)
    print(DataClassPerson.quidditch_position)

{'name': 'Viktor', 'quidditch_position': 'Seeker'}
{'name': 'Hermione', 'quidditch_position': 'None'}
None
```

Both Viktor and Hermione have instance attributes for BOTH `name` and `quidditch_position`. If we compare this to our non-dataclass from before

```Python
harry = Person("Harry")
harry.set_position("Seeker")
draco = Person("Draco")
print(harry.__dict__)
print(draco.__dict__)

{'name': 'Harry', 'quidditch_position': 'Seeker'}
{'name': 'Draco'}
```

We can see Draco does not have an instance attribute for `quidditch_position`. This is just how dataclasses work under the hood - everything you declare is going to become an instance attribute, though there are also class attributes for the ones you assign to.

Through this essay we've explored how C# and Python handle class variables differently. We've seen how class attributes vs instance attributes are declared in Python and how to instantiate attributes correctly in Python to avoid unexpected behavior. Lastly, we took a look at dataclasses and how they work under the hood in ways we might not expect.
