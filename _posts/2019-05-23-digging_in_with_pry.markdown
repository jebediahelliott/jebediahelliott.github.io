---
layout: post
title:      "Digging in with Pry"
date:       2019-05-24 01:44:30 +0000
permalink:  digging_in_with_pry
---

Do you ever have trouble holding the values of all your variables in your head? Lose track of what something represents when your working with a complex data structure? Wish you could stop your code and see what on earth is going on. Lets talk about `pry`. This wonderful little gem allows you to stop the execution of your code and gives you a console wih access to everything that is in scope at that point in time. What does this mean? Lets look at an example. If you would like to follow along, use your terminal to `cd` into the directory/folder you would like to work in and create a file called `pry-playground.rb` using the command `touch pry-playground.rb` and open it in your favorite editor. Add this code:


```
require 'pry'

def add(a, b)
  binding.pry
  puts a + b
end

add(1, 4)
```

Here's the breakdown. At the top of the file we require the pry gem. This is an import and tells our program what pry is so that it knows what to do when we call it. Then we define a method `add` that adds 2 numbers and prints the result in the terminal. We've also put `binding.pry` at the beginning or our method. `binding` is a built in part of the ruby language that stops the code at the point where it is called an provides access to our information at that point in time. `pry` is what we use to interact with the information that `binding` gives us access to. At the bottom of this snippet we execute our `add` method and give it arguments of 1 and 4. 

*Note: You will need to have run* `gem install pry`*in order for pry to work. As of this writing you may receive the following error when you run your file.*
```
Sorry, you can't use Pry without Readline or a compatible library.
Possible solutions:
 * Rebuild Ruby with Readline support using `--with-readline`
 * Use the rb-readline gem, which is a pure-Ruby port of Readline
 * Use the pry-coolline gem, a pure-ruby alternative to Readline
```
*I used the third option suggested and installed* `pry-coolline` *via the command* `gem install pry-coolline` *and everything worked as it should, *

Ok, that was a whole lot of explaining, lets see some action. In the terminal run `ruby pry-playground.rb` and you should see the following output:

```
    3: def add(a, b)
 => 4:   binding.pry
    5:   puts a + b
    6: end

[1] pry(main)>
```

We have now entered a pry session. Line 4, as indicated by the hash rocket, is where our code has paused execution. We are now in a console setting and can check the values of our variables and try out code. If you type `a` in the terminal you should see this output: 

```
[1] pry(main)> a                                                           
=> 1
```

If we try out `b`:

```
[2] pry(main)> b                                                           
=> 4
```

Now here is the fun part, and where pry really earns its chops. We can test out code in the pry console and immediately see the result:

```
[3] pry(main)> a + b                                                     
=> 5
```

*Note: To end a pry session, type* `exit` *in the terminal. If your pry is in a loop this will advance your code to the next iteration of the loop, otherwise it will end the pry session. If you are in a loop and want to end the pry session you can use* `exit!` *to beak out of the loop.*

Now I know `a + b` is by now means groundbreaking, we already knew where that would lead. But what about when we don't know where the code will lead? What about when we are writing something What if you are dealing with a 7 layer deep hash and want to make sure you are accessing the right key? Now you can be sure. What if you are trying to select words that start with the letter 'A' from an array of a hundred different words? Now you can just pop your code in the pry and make sure it is indeed working the way you expect. Let's demonstrate with a more complex example.

```
RICKS = ["Cop Rick", "Rick Prime", "Pickle Rick", "Cronenberg Rick", "John Rick", "Rick C-137"]

def find_the_rick(rick)
  RICKS.find do |r|
    binding.pry
    r == rick
  end
end

find_the_rick("Pickle Rick")
```

Here we have a method that will find a particular Rick from an array of Ricks. Not super useful, but good for demonstration purposes.

If we run this in the terminal, `ruby pry-playground.rb`, we will hit our pry:

```
    13: def find_the_rick(rick)
    14:   RICKS.find do |r|
 => 15:     binding.pry
    16:     r == rick
    17:   end
    18: end

[1] pry(main)> 
```

Now we can check our values:

```
[1] pry(main)> rick                                                      
=> "Pickle Rick"
[2] pry(main)> r                                                         
=> "Cop Rick"
[3] pry(main)> RICKS                                                     
=> ["Cop Rick",
 "Rick Prime",
 "Pickle Rick",
 "Cronenberg Rick",
 "John Rick",
 "Rick C-137"]
[4] pry(main)> 
```

And we can check to see if our expression is behaving as we expect:

```
[4] pry(main)> r == rick                                                 
=> false
```

If we want to move to the next iteration we type in `exit` and we loop and hit the pry again:

```
[5] pry(main)> exit                                                      

From: /Users/jeb/code/labs/pry-playground.rb @ line 15 Object#find_the_rick:

    13: def find_the_rick(rick)
    14:   RICKS.find do |r|
 => 15:     binding.pry
    16:     r == rick
    17:   end
    18: end

[1] pry(main)>  r                                                         
=> "Rick Prime"
[2] pry(main)> 
```

If we keep going we will eventually find "Pickle Rick" or if we are satisfied that everything is working properly we can use `exit!` to break out of the loop and let the program finish.

You may be thinking that everything we've seen so far we could have deduced on our own and you would be right. These examples have been pretty simple. Let's look at something that will get your hampspster spinning the wheel a little faster.

*Warning: Game of Thrones spoilers ahead*

```
HOUSES = {
  regions: {
    north: {
      stark: {
        patriarch: {
          name: 'Ned',
          status: 'dead'
        },
        matriarch: {
          name: 'Catelyn',
          status: 'dead'
        },
        children: {
          robb: {
            gender: 'male',
            status: 'dead'
          },
          sansa: {
            gender: 'female',
            status: 'alive'
          },
          arya: {
            gender: 'female',
            status: 'alive'
          },
          brandon: {
            gender: 'male',
            status: 'alive'
          },
          rickon: {
            gender: 'male',
            status: 'dead'
          }
        }
      },
      bolton: {
        patriarch: {
          name: 'Roose Bolton',
          status: 'dead'
        },
        matriarch: {
          name: 'unknown',
          status: 'dead'
        },
        children: {
          ramsay: {
            gender: 'male',
            status: 'dead'
          }
        }
      }
    },
    south: {
      baratheon: {
        patriarch: {
          name: 'Robert',
          status: 'dead'
        },
        matriarch: {
          name: 'Cersei',
          status: 'dead'
        },
        children: {## on paper at least
          joffrey: {
            gender: 'male',
            status: 'dead'
          },
          myrcella: {
            gender: 'female',
            status: 'dead'
          },
          tommen: {
            gender: 'male',
            status: 'dead'
          }
        }
      },
      tyrell: {
        patriarch: {
          name: 'Mace',
          status: 'dead'
        },
        matriarch: {
          name: 'Alerie',
          status: 'dead'
        },
        children: {
          loras: {
            gender: 'male',
            status: 'dead'
          },
          margaery: {
            gender: 'female',
            status: 'dead'
          }
        }
      }
    },
    east: {
      arryn: {
        patriarch: {
          name: 'Jon',
          status: 'dead'
        },
        matriarch: {
          name: 'Lysa',
          status: 'dead'
        },
        children: {
          robin: {
            gender: 'male',
            status: 'alive'
          }
        }
      },
      tully: {
        patriarch: {
          name: 'Hoster',
          status: 'dead'
        },
        matriarch: {
          name: 'Minisa',
          status: 'dead'
        },
        children: {
          catelyn: {
            gender: 'female',
            status: 'dead'
          },
          lysa: {
            gender: 'female',
            status: 'dead'
          },
          edmure: {
            gender: 'male',
            status: 'dead'
          }
        }
      }
    },
    west: {
      lannister: {
        patriarch: {
          name: 'Tywin',
          status: 'dead'
        },
        matriarch: {
          name: 'Joanna',
          status: 'dead'
        },
        children: {
          cersei: {
            gender: 'female',
            status: 'dead'
          },
          jaime: {
            gender: 'male',
            status: 'dead'
          },
          tyrion: {
            gender: 'male',
            status: 'alive'
          }
        }
      },
      greyjoy: {
        patriarch: {
          name: 'Balon',
          status: 'dead'
        },
        matriarch: {
          name: 'Alannys',
          status: 'dead'
        },
        children: {
          rodrik: {
            gender: 'male',
            status: 'dead'
          },
          maron: {
            gender: 'female',
            status: 'dead'
          },
          yara: {
            gender: 'female',
            status: 'alive'
          },
          theon: {
            gender: 'male',
            status: 'dead'
          }
        }
      }
    }
  }
}
```

Now this is something that could leave you scratching your head as you try to drill in for specific pieces of information. There are many different things that you might want to extract from this hash. It could be a specific as which female members of a particular house are still alive, or as general as what are the names of each of the houses. Let's do something in between, we will grab the name of each member of a given house and print them to the console. We'll start by creating the empty array to hold the names in. Then we'll start iterating over the hash.

```
def house_members(house)
  names = []

 HOUSES[:regions].each do |region, house_names|
    binding.pry
  end
end
```
	
Notice that I haven't written any code in my loop. The first thing I always do is check to see what my values are with pry. Also notice that I have made my variables in the pipes(`||`) descriptive. When we check the values in our pry we get this:

```

    200: def house_members(house)
    201:   names = []
    202: 
    203:   HOUSES[:regions].each do |region, house_names|
 => 204:       binding.pry
    205:   end
    206: end

[1] pry(main)> region                                                                                 
=> :north
[2] pry(main)> house_names                                                                            
=> {:stark=>
  {:patriarch=>{:name=>"Ned", :status=>"dead"},
   :matriarch=>{:name=>"Catelyn", :status=>"dead"},
   :children=>
    {:robb=>{:gender=>"male", :status=>"dead"},
     :sansa=>{:gender=>"female", :status=>"alive"},
     :arya=>{:gender=>"female", :status=>"alive"},
     :brandon=>{:gender=>"male", :status=>"alive"},
     :rickon=>{:gender=>"male", :status=>"dead"}}},
 :bolton=>
  {:patriarch=>{:name=>"Roose Bolton", :status=>"dead"},
   :matriarch=>{:name=>"unknown", :status=>"dead"},
   :children=>{:ramsay=>{:gender=>"male", :status=>"dead"}}}}
[3] pry(main)>  
```

Now, instead of trying to hold what structure of the hash is in our head and remember what level we are on, we can just keep working the pry on each level to get where we want to go. We add our next level of iteration and check the values:

```
def house_members(house)
  names = []

  HOUSES[:regions].each do |region, house_names|
    house_names.each do |house_name, positions|
      binding.pry
    end
  end
end
```

And get:

```
[1] pry(main)> house_name                                                                             
=> :stark
[2] pry(main)> positions                                                                              
=> {:patriarch=>{:name=>"Ned", :status=>"dead"},
 :matriarch=>{:name=>"Catelyn", :status=>"dead"},
 :children=>
  {:robb=>{:gender=>"male", :status=>"dead"},
   :sansa=>{:gender=>"female", :status=>"alive"},
   :arya=>{:gender=>"female", :status=>"alive"},
   :brandon=>{:gender=>"male", :status=>"alive"},
   :rickon=>{:gender=>"male", :status=>"dead"}}}
[3] pry(main)>
```

At this point we have found the first place where we want to check for conditions. Remember, we are looking for a specific house. So we set up the following condition:

```
def house_members(house)
  names = []

  HOUSES[:regions].each do |region, house_names|
    house_names.each do |house_name, positions|
      if house_name.to_s == house.downcase
        positions.each do |position, info|
				  binding.pry
				end
      end
    end
  end
end
```

With this condition, we will only continure drilling in to the hash if we have matched the name of the house we are looking for. Now we check out our values in pry:

```
[1] pry(main)> position                                                                               
=> :patriarch
[2] pry(main)> info                                                                                   
=> {:name=>"Mace", :status=>"dead"}
[3] pry(main)> info[:name]                                                                            
=> "Mace"
[4] pry(main)>
```

Finally! We made it to the names we were looking for. In this case I had been looking for Tyrell house members. So now we can add the names to our array and we should be good to go:

```
def house_members(house)
  names = []

  HOUSES[:regions].each do |region, house_names|
    house_names.each do |house_name, positions|
      if house_name.to_s == house.downcase
        positions.each do |position, info|
          names << info[:name]
        end
      end
    end
  end
  puts names
end
```

If we call our method(you can do this at the end of your file and provide an argument) with the argument of 'Tyrell' we should see all of the Tyrell names printed out.

```
ruby pry-playground.rb 
Mace
Alerie
```

Hmmm, something isn't right, we didn't get the children. If we look at the pry session above where we checked the `postion` variable, we will notice that children are nested one level deeper, so we need to account for when we hit that key. One last pry should do the trick:

```
positions.each do |position, info|
  if position == :children
    binding.pry
  end
  names << info[:name]
end
```

Whick gives us:

```
pry(main)> info                                                                                   
=> {:loras=>{:gender=>"male", :status=>"dead"}, :margaery=>{:gender=>"female", :status=>"dead"}}
[2] pry(main)>
```

Excellent! Now we can put the final touches on our code:

```
def house_members(house)
  names = []

  HOUSES[:regions].each do |region, house_names|
    house_names.each do |house_name, positions|
      if house_name.to_s == house.downcase
        positions.each do |position, info|
          if position == :children
            info.each do |name, attributes|
              names << name.to_s.capitalize
            end
          end
          names << info[:name]
        end
      end
    end
  end
  puts names
end
```

And if we run it we end up with the desired result:

```
ruby pry-playground.rb 
Mace
Alerie
Loras
Margaery
```

Whew! That was quite a lot. I think it's time to go relax with a nice horn of giants milk. Cheers!
