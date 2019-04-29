---
layout: post
title:      "What the Heck is Collection Select?"
date:       2019-04-29 20:31:50 +0000
permalink:  what_the_heck_is_collection_select
---


As I'm sure you have guessed, `collection_select` has to do with making a selction. More specifically, it is a Ruby on Rails form helper that allows you to display a collection of things in a dropdown and allow your user to, you guessed it, select one of them. As simple as the concept is, the implementation and usage can be rather unclear. The [official docs](https://api.rubyonrails.org/classes/ActionView/Helpers/FormOptionsHelper.html#method-i-collection_select) are, at least to me, a little bit cryptic. Much like one of Nostradamus' predictions, they only make perfect sense after you already know what's going on. I think one of the reasons(I would postit it is the primary reason) for confusion around the usage of `collection_select` is because it can take as many as 7 arguments. That's not a typo, it takes seven!! And 2 of them are hashes. Without further adieu, let's dig in and see how this sucker works. First we'll start with our models:
```
Class Owner < ApplicationRecord
  has_many :direwolves # Yes, ActiveRecord is smart enough to figure out this pluralization in most cases.
end
```
```
Class Direwolf < ApplicationRecord
  belongs_to :owner
end
```
For the purposes of this example, each of our models will only need one attribute: name. Now lets create some instances of each:
```
john = Owner.create(name: 'John')
arya = Owner.create(name: 'Arya')
ghost = Direwolf.create(name: 'Ghost')
nymeria = Direwolf.create(name: 'Nymeria')
```
Now let's get to the matter at hand. `collection_select` has the following general format: 
```
collection_select(object, method, collection, value_method, text_method, options = {}, html_options = {})
``` 
If this is the first time you're seeing this, I'd be willing to be that for most of you it's not exactly clear what this means. Here's the breakdown of what each of these are:

`object`: This is the object that we will be associating with what is selected in the dropdown

`method`: This is the attribute of `object` that we will be assigning a value to that creates the association. It is usually a foreign key

`collection`: This is what we will populate our dropdown with. Think `ModelName.all`

`value_method`: This is the attribute of an instance of `collection` whose value will be assigned to `method`. It is usually `id`

`text_method`: This is the attribute of each instance of `collection` that will be displayed in the dropdown.

`options`: The docs for what can be passed as options are blissfully more explanatory and can be viewed [here.](https://api.rubyonrails.org/classes/ActionView/Helpers/FormOptionsHelper.html)

`html_options`: This is where you would add any custom html such as a class.

Let's use an example to try and shed some light on what's going on here. We'll make a form that takes an Owner and associates a Direwolf to that Owner. 
```
<%= form_for owner do |f| %>
	<%= f.collection_select :direwolf_ids, Direwolf.all, :id, :name %> 
<% end %>
```
Now some of you might be thinking, "Hey man, you made a big deal about 7 arguments, but I'm only holding up 4 fingers," and that is an excellent place to start, because it is another point of confusion. Some of you readers will have surmised that the last two arguments, `options` and `html_options`, are, as the names suggest, optional. That still leaves us with 5 required arguments but we only have 4... or do we? Since we are using the form builder `form_for`, we are inheriting our first argument, `object`, from the form builder. In this case it is `owner` If we were using `form_tag` instead of `form_for`, we would have needed to pass `owner` as the first argument to `collection_select` followed by the other 4 arguments. 

Now lets talk about what this particular instance of `collection_select` will do. For this example, `owner` will represent the Owner object `arya` that we created earlier. Our first(non-inherited) argument, `direwolf_ids` is the attribute of `arya` that is going to get assigned a value. If you are wondering or are unfamiliar with how `arya` got the attribute `direwolf_ids`, it is a freebie from ActiveRecord. I highly reccommend that you make yourself familiar with all of the different [methods](https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html) that are inherited from each of the ActiveRecord associations. So, now we know that `arya.direwolf_ids` is going to get assigned a value. `Direwolf.all` is the collection that we will be populating our dropdown with. i.e. we are going to list all of the instances of the Direwolf class in our dropdown. Our next argument, `id`, is the attribute of the Direwolf instance that our user selects that will be assigned to `arya.direwolf_ids`. So if the user selects Nymeria, the value from `nymeria.id` is going to be assigned to `arya.direwolf_ids` It is this assignment that will create the ActiveRecord association between these two objects. Our final argument(in this case) is `name`. It is the attribute of each Direwolf instance that will be displayed in the dropdown. In this case we would have the values from `nymeria.name` and `ghost.name` displayed in our dropdown. Don't forget that we could have included two more argument via `options` and `html_options` I did not include them in this example because they do not help demonstrate the core functionality of `collection_select` and the documentation at the link provided already does a pretty good job. I hope this 

`collection_select` is a very useful tool when it comes to making associations in forms. I hope this has pulled back the curtains on how it works and how you can put it to work for you
