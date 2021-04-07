---
layout: page
title: Lab 2
published: true
comment_term: lab-quizzical
---

# Build a Buzzfeed® Quiz Site! :squirrel:

![](img/quiz.gif){: .fancy .small}

All the kids are doing it these days. This is where we'll learn a little bit about making interactive sites. We won't need to deal with any backend servers, just some nice clientside code (no server) to build up a fun quiz and show some calculated results.

🚀 head over to buzzfeed and take [this quiz](https://www.buzzfeed.com/elainawahl/which-disney-animal-are-you). I'll wait.

[![](img/buzzfeed.png){: .fancy .medium}](https://www.buzzfeed.com/elainawahl/which-disney-animal-are-you)

Doesn't that make you want to make your own quiz? Ok, you could use buzzfeed to do this, but you want to make one that doesn't have ugly ads. And looks classier too!

This assignment is going to *flex* your html and css skills some more as well as some javascript!  We'll use some JS to compute the results of the quiz as well as to check that the quiz was filled out correctly.

**Note:** You are allowed to use HTML5/CSS3/JS/jQuery in this assignment but no other javascript libraries or frameworks. Try to use compatible plain ES5 JS/jQuery without any newer syntax so that your page will work in browsers without the need for anything aside from jQuery (if you choose to use it). You may use CSS libraries to help with styling but it is recommended to practice your CSS skills and try to do it from scratch. 🔩

**Note:** You can and should use the [inspector 🔍](https://developer.chrome.com/devtools) to examine the structure pages you see. Although it is a good idea to inspect individual elements (for instance: a button to see how they styled the borders) you'll find that the way they do things is ugly anyway and I would recommend not imitating them at all. If your code blatantly includes [un-cited code](/logistics/#citation-format) copied from buzzfeed.com, that will be considered an honor code violation :warning: Ask me if you have questions about this.


## Plan

You have a lot of flexibility in this assignment.  This is your content, so be creative. How much you abstract out the problem is up to you.  You can start by hard-coding every question and answers in HTML, that is 100% acceptable.  If you choose you could implement reading the questions from say a separate JSON file - this would be extra credit. More about this later.  How you score the quiz is completely up to you.  Just a counter is sufficient but you could try something more complicated with different weights associated with the various answers. And finally,  the look and feel is all up to you.  There are just a few minimum requirements.


### Minimal Functional Specs:

* Quiz Page:
  * Questions:
    * display several questions (enough for this to be interesting)
    * have a header image and some text
    * have some number of potential answers
  * Question Answers:
    * be either text or an image (demonstrate some of both)
      * *text answers are just clickable text boxes (not an image)*
    * have 4 potential display states:
      * initial none selected state
      * have a `:hover` effect
      * have a clicked/selected state
      * when one answer is chosen all other answers should change to a not-selected state (different from the initial no answer chosen state).
    * have an associated value to be used to calculate the overall quiz output.
  * Done Button:
    * calculate the quiz output
  * Output Display
    * should have text and an image
    * should not show unless the calculations are finished
    * should display an error if not all questions were answered


Here's some design specs thrown together in [Figma](https://www.figma.com/)

1. Quiz page with a question and some image answers - nothing selected:

    ![](img/main.png){: .medium}

1. An example hover effect:

    ![](img/hover.png){: .medium}

1. An answer selected:

    ![](img/checked.png){: .medium}

1. Answer display:

    ![](img/done.png){: .medium}


## Where to Start?!

🚀 Please use the github classroom link provided in the Canvas assignment to create the repo automatically.

🚀 Open up your code editor and create an `index.html`, a `style.css`, and a `main.js` file.  

🚀 Link your `style.css` file into the head of your html file.

```html
<link rel="stylesheet" type="text/css" href="style.css">
```

🚀 Link in your `main.js` file. Best place for this is right before your closing `</body>` tag.  

Extra credit:  why might it be better for this to be placed at the bottom rather than in `<head> ... </head>`?

```html
<script src="main.js"></script>
```


## HTML

Start with coding up a single question and answer - then use flexboxes to arrange them. At this point using flexboxes for this should be almost second nature. You don't need any of those fancy grid frameworks.

One thing you may have noticed is that when you select an answer it is a exclusive selection.  There is an input type that already does that automatically.  Yup, [radio boxes](http://learn.shayhowe.com/html-css/building-forms/#radio-buttons)!

But the default styling of radio boxes is so ugly!  Sure, but you can do something similar to the checkbox hack and hide the actual radio box but use it for some display logic.

Here is the basic syntax for a radio button with an image label.

```html
<label for="puppy_question"><img src="http://onlines/someimage.png" /></label>
<input type="radio" id="puppy_question" value="puppy"/>
```

```css
#puppy_question {
  display: none;
}
```

You can actually nest the input inside of the label and then you don't have to do the `for="elementID"` part of the label, it will just automatically serve as the label for the input inside it.

```html
<label>
  <img src="http://onlines/someimage.png" />
  <input type="radio" value="puppy"/>
</label>
```

And you can suppress all radio inputs like so:

```css
input[type=radio] {
  display: none;
}
```

It is important to note that the way that you group radio buttons together
(i.e make it so clicking one unclicks others) is by their 'name' property.
Giving two radio buttons the same name groups them together.

For example, these two radio buttons could both be selected at the same time
(you can have a favorite dog and a favorite cat):

```html
<label>
  <img src="http://onlines/someimage.png" />
  <input type="radio" name="favorite_dog" value="dalmation"/>
</label>
<label>
  <img src="http://onlines/someimage.png" />
  <input type="radio" name="favorite_cat" value="tiger"/>
</label>
```

While these two radio buttons will automatically unselect one when the other is clicked
(you cannot have two favorite dogs):

```html
<label>
  <img src="http://onlines/someimage.png" />
  <input type="radio" name="favorite_dog" value="dalmation"/>
</label>
<label>
  <img src="http://onlines/someimage.png" />
  <input type="radio" name="favorite_dog" value="retriever"/>
</label>
```

## Javascript Logic

Once you have the basic structure up you can dive into getting the logic working. At this point you should have some radio boxes on the screen that are checkable.  But how do you collect the input from them?

For this assignment you are allowed to use [jQuery](https://jquery.com/) if you wish. Just load it before your own javascript in your `index.html` file:

```javascript
<script
  src="https://code.jquery.com/jquery-3.3.1.min.js"
  integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
  crossorigin="anonymous"></script>
```

jQuery is purely a personal choice. It helps simplify some things. If you have used it before, challenge yourself and use plain vanilla JS this time around.  Many sites on the internet use jQuery so it doesn't hurt to be familiar with it.

Most likely you have a button that you would like to attach a javascript action to.  Here's a tiny bit of code to get you started:

```javascript
$('#idOfYourButton').on('click', function(e) {
  // gather all checked radio-button values
  var choices = $("input[type='radio']:checked").map(function(i, radio) {
    return $(radio).val();
  }).toArray();
  // now you have an array of choices = ["valueofradiobox1", "valueofradiobox2", "valueofradiobox2"]
  // you'll need to do some calculations with this
  // a naive approach would be to just choose the most common option - seems reasonable
});
```

Some jQuery/JS that will come in handy:

* [show()/hide()](https://www.w3schools.com/jquery/jquery_hide_show.asp) // equivalent to `document.getElementById('someid').style.display = 'none';`
* [text()/html()](https://www.w3schools.com/jquery/jquery_dom_set.asp) // equivalent to `document.getElementById('someid').textContent='newtext';`

There's a lot you can do here, but the basics that you want are a button that calculates an answer and then shows it on the page!

The answer could be displayed as a [simple modal](https://www.w3schools.com/howto/howto_css_modals.asp) or inline like how buzzfeed does it.


## Styling

Now get it looking good!  If you need suggestions on how to improve your design come by any of our office hours.  This assignment will be partially graded on styling.  The minimum effort would be to do something like the primitive mockups or an equivalent attempt at improving the design of buzzfeed.

## Choose Your Own Adventure Part

You are required to **choose 1 of the next 2 options** to complete.  Extra credit if you do both, but 1 is part of the assignment.

At this point you should tag your working base version first:

```bash
git tag v1
git push origin --tags
```

## Javascript Focus (Option 1)

🍸 Create a generalized framework for quizzes. This will help you understand frontend frameworks and get a better feel for javascript and how to work with the DOM.

This will involve reading in quiz questions and answers from a [JSON file](http://api.jquery.com/jquery.getjson/) and then looping through them and [appending](http://api.jquery.com/append/) to the html of the page with javascript.

Imagine you had a `data.json` file that looked like this:

```json
{
    "title": "quiz title",
    "outcomes": {
            "outcome1": {"img": "img/outcome1.jpg", "text": "outcome 1"},
            "outcome2": {"img": "img/outcome2.jpg", "text": "outcome 2"}
    },
    "questions": [
        {
            "question_name": "question1",
            "question_img_url": "img/question1.jpg",
            "answers": [
                {
                    "text": "",
                    "img_url": "img/image1.jpg",
                    "outcome": "outcome2"
                },
                {
                    "text": "",
                    "img_url": "img/image2.jpg",
                    "outcome": "outcome2"
                },
                {
                    "text": "",
                    "img_url": "img/image3.jpg",
                    "outcome": "outcome1"
                }

            ]
        },
        {/*........*/}
    ]
}
```

You may load this file into your main javascript using the [jQuery getJSON](https://api.jquery.com/jquery.getjson/) function:

```js
  $.getJSON("data.json", function(data) {
    // now you can do something with this data. 
    // remember you can only work with the data in this callback
    // data.title has the title
    // maybe you want to loop through data.questions? 
  });
```

💰 Most likely you'll want to take your already working hardcoded HTML, abstract out some section, and output it from a javascript loop that constructs a big string of all the html you need and then append it to some element in the page. You may find it irritating to do: `"<div>"+question.title+"</div>"`... as such if you don't care about backwards compatibility (Internet Explorer) you may find [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) useful. 


## Styling Focus (Option 2)

You second option is make your quiz site as beautiful as possible with as much fancy CSS as you can muster. 

Minimum Styling Requirements: 
* No radio buttons here folks.
* Hovers and clicks have animation/transition effects.
* Your output is a modal, or a new page, or a scroll, it is not a alert, and it is animated.
* and finally, your page works in mobile! 

This will push your CSS skills and will be super helpful later as you become the style wizard for your team.  You will make your product look amazing, and honestly isn't that the part that everybody appreciates the most? 

[![modal animations](img/cool-modal-animations.gif){: .fancy .medium}](https://codemyui.com/cool-modal-animations/) 

[![expanding](img/fullscreen-expanding-div.gif){: .fancy .medium}](https://codemyui.com/div-fullscreen-click/) 

[![zoom expanding](img/pure-css-zoom-in-blocks-on-hover.gif){: .fancy .medium}](https://codemyui.com/pure-css-zoom-blocks-hover/) <br>*might be overkill*

[![light sweep button](img/light-sweep-button.gif){: .fancy .medium}](https://codemyui.com/light-sweep-effect-on-button-hover/) <br>*sweet button*

Lots of inspiration at [codemyui.com](https://codemyui.com).

Some more resources: 
* [Kevin's CSS Styles](https://thekevhu.github.io/Styles/)
* [https://css-tricks.com](https://css-tricks.com)



## Host It and You Are Done!

You should host this on github pages as you have in the past.  Remember to have fun with this.  We'll share the quizzes in class!


![what I got](img/scar.png){: .fancy .medium}


## To Turn In:

* urls to your hosted page (gh-pages or netlify - separate hosting from other assignments) **and** a link to your classroom repo
* your page should have all the [MVP specs](#minimal-functional-specs) in addition to:
  * have clear document structure with proper semantic naming
  * functional calculations and error checking to get quiz results
  * look reasonable, styling should be at least as classy as the mockups...
* your repo should include a README.md file with:
  * a couple sentence description of what you did and what worked / didn't work.
  * what focus you chose: javascript or style or both
  * screen caps with anything special you want to point out


## Extra Credit
*feel free to suggest other possibilities*
*always mention your extra credit in the README.md file*

* More complex quiz scoring (weighted options, real science, probabilities)
* Multiple quizzes / pages
* ? (propose something)


## Resources:

* [https://css-tricks.com](https://css-tricks.com)
* [http://www.w3schools.com](http://www.w3schools.com)
* [https://philipwalton.github.io/solved-by-flexbox/](https://philipwalton.github.io/solved-by-flexbox/)
* [http://learn.shayhowe.com/html-css/building-forms/#radio-buttons](http://learn.shayhowe.com/html-css/building-forms/#radio-buttons)
* [css-animation-for-beginners](https://thoughtbot.com/blog/css-animation-for-beginners)
* [Chrome Devtools](https://developer.chrome.com/devtools)
* [mycolor.space/](https://mycolor.space/)
* [color.adobe.com/create/color-wheel](https://color.adobe.com/create/color-wheel)
