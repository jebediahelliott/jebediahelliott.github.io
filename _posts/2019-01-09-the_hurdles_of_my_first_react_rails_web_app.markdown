---
layout: post
title:      "The Hurdles of My First React/Rails Web App"
date:       2019-01-09 05:09:35 +0000
permalink:  the_hurdles_of_my_first_react_rails_web_app
---


I've just finished my first app built with React in the front end and Rails in the back end, and I found it to be a pretty enjoyable experience. As is expected any time you're using tools for the first time there were new challenges and new ways of doing things to be figured out. For the most part, a little bit of googling will find you the answers you're looking for when these challenges arrise. I did however encounter two puzzles that google couldn't provide the typical wrapped up with a shiny bow solution for and I thought I would share how I dealt with them. I would like to say now that I would be in no way be surprised to learn that there are better solutions to these problems that will become apparent as my experience grows. 

The first problem I had was collecting information from a controlled form in a nice tidy format to send to the Rails back end. The form is collecting information for two models, one of which accepts nested attributes for the other. In my particular case I had a StaticPage model that accepted attributes for a Paragraph model, like so:
```
class StaticPage < ApplicationRecord
  has_many :paragraphs
  accepts_nested_attributes_for :paragraphs
end
```
```
class Paragraph < ApplicationRecord
  belongs_to :static_page
end
```
Previously, I would have structured a form to collect this information something like this:
```
<form>
  <input type='text' name='static_page[title]'>
  <input type='textarea' name='static_page[paragraphs_attributes][][content]'>
  <input type='textarea' name='static_page[paragraphs_attributes][][content]'>
  <input type='textarea' name='static_page[paragraphs_attributes][][content]'>
  <input type='textarea' name='static_page[paragraphs_attributes][][content]'>
  <input type='submit'>
</form>
```
The data from this form would be formatted exactly the way that Rails wants it, thanks for coming out, have a nice day. Not so with React. With React we want to have a controlled form, meaning we track every change to the form fields and store them in the local state. This is usually done by giving each input a unique name attribute and tracking it with a corresponding name key in the state. My first instict was to just use data attributes to set up the unique keys for state and then grab the formatted for Rails data that the above form would normally create when you hit submit. As I started putting it together, I discovered that there is no built in way(at least that I know of) to grab the data with the formatting I was hoping for. I wound up with two solutions, the first one is stupendously ugly and I won't share it here but it involves setting unique keys in the state and then reformatting the state for Rails when the form is submitted. As bad as it was, it did have the useful aspect of getting me to the second solution, which is to format the state to fit what Rails is going to want in the first place. I'm not sure why I didn't think of that in the first place, but I would posit that it has to do with trying to manipulate things to match the mental model I had for how form data is collected. Anyway, I ended up with a state that is structured as follows:
```
this.state = {
  title: '',
  paragraphs_attributes: {
    0: {
      content: ''
    },
    1: {
      content: ''
    }
    2: {
      content: ''
    }
  }
```
It took a little bit of doing to work out how to control the form with the state having three levels but it wasn't the end of the world and it worked out nicely in the end. With that problem taken care of I was back to trucking right along and had the app finished all but for one problem: Every time I submitted an edit form, which redirected back to the page that shows whatever changes you've made, the changes weren't there. The database was getting updated, and the store was getting updated, but I wasn't getting my re render that would show the updated info on the page. If I navigated to another page and then came back, the changes were there but never on the initial redirect from the edit form. This one I'm almost postive has to have a built in way of handling that I could not find and I will admit that what I came up with feels extra "hacky." I ended up storing all the pertinent information in the local state of the container component for the components in question. With the edit forms now triggering updates to the local state I got my re render and the pages displayed the updated information. The reason I don't like this solution is, as far as I can tell, it should be uneccessary since the update to props should re render the component, and it is definitely not DRY. On the other hand, it does work, and until I can find a way to make it better, its the best I've got. So goes the never ending cycle: Red, Green, Refactor.
