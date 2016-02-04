# IEEE Web Application Development Workhop

An IEEE workshop to introduce students to full stack web application development using Meteor

### Prerequisites: HTML, CSS, JavaScript
This workshop assumes you have a general understanding of the basics concepts in HTML, CSS, JavaScript, Bootstrapn and Git. Below, I've listed some excellent resources which you can use to either brush up or thoughly learn these concepts. 

###### Comprehensive & Interactive Learning
  * [HTML & CSS Codecademy (~7 hours)](https://www.codecademy.com/learn/javascript)
  * [JavaScript (~10 hours)](https://www.codecademy.com/learn/javascript)
  * [15 minute Git tutorial](https://try.github.io/levels/1/challenges/1)

###### Quick Reads & Tutorials 
 * [W3 Schools: HTML](http://www.w3schools.com/html/default.asp)
 * [W3 Schools: CSS](http://www.w3schools.com/css/default.asp)
 * [W3 Schools: JS](http://www.w3schools.com/js/default.asp)
 * [W3 Schools: Bootstrap](http://www.w3schools.com/bootstrap/default.asp)
 
###### Code Examples
 * [W3 Schools: HTML Snippets](http://www.w3schools.com/html/html_examples.asp)
 * [W3 Schools: CSS Snippets](http://www.w3schools.com/css/css_examples.asp)
 * [Bootstrap themes](https://bootswatch.com/default/)

###### References
 * [W3 Schools: HTML Reference](http://www.w3schools.com/tags/default.asp)
 * [W3 Schools: CSS Reference](http://www.w3schools.com/cssref/default.asp)
 * [W3 Schools: JS Reference](http://www.w3schools.com/jsref/default.asp)


### Workshop Overview
- In this workshop we will be building a Twitter clone using [Meteor](https://www.meteor.com/), a full-stack web application framework. 
- We will be following an excellent tutorial  written by [Kai from randomdotnext.com](http://randomdotnext.com/) - [Meteor Tutorial - Let's build a Twitter clone](http://randomdotnext.com/meteor-tutorial-p1/)


## Part 1: Intro to Meteor

### What is Meteor
- Meteor is a full-stack JavaScript framework. What is a full-stack framework?
- Meteor is real time out the box. What the heck does that mean?
- Meteor is built on top of node.js. What is node.js? Do I need to learn it?
- The core of Meteor comes with MongoDB (Database), Blaze (front-end reactive framework)

### Section 1: Installation & Setup
- [Install Meteor](https://www.meteor.com/install)

Meteor comes with a command-line tool belt `meteor` used to create your project and run many of meteors commands. 

Create a Meteor project:
```bash
$ meteor create myTwitterClone
$ cd myTwitterClone
$ meteor
```

Open browser to localhost:3000 to see your application running.

### Section 1: Exploring the barebone Meteor app

Open your project folder in a file explorer. You should see the following files/folders:
- myTwitterClone.css
- myTwitterClone.html
- myTwitterClone.js
- .meteor (hidden folder)

__Notes__:
- Entire js logic of the application is in 1 file with serverside and client side blocks
- The server side and client side will eventually be broken up into seperate modules


*Server vs Client example*:
- When you log into Facebook, your browser doesn't get every single Facebook photo every uploaded. 
- The server will just send back what is revelant based on what the client logic requests

#### Events block
Within your myTwitterClone.js file:
```javascript
  Template.hello.events({
    'click button': function () {
      // increment the counter when button is clicked
      Session.set('counter', Session.get('counter') + 1);
    }
  });
```
- Similar to jQuery style of listening to events
- *Changes to variables stored inside the Session variable reactively notify all the listeners on that variable*

#### Helpers block
Within your myTwitterClone.js file:
```javascript
Template.hello.helpers({  
  counter: function () {
    return Session.get('counter');
  }
});
```
- This helper function listens to any changes happending to the Session variable `counter`
- If the value of the `counter' changes, the helper tell the front-end that the HTML should render a new value

#### What is a Template?


