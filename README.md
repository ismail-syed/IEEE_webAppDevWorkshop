# IEEE Web Application Development Workhop

An IEEE workshop to introduce students to full stack web application development using Meteor

### Prerequisites: HTML, CSS, JavaScript
This workshop assumes you have a general understanding of the basics concepts in HTML, CSS, JavaScript, Bootstrapn and Git. Below, I've listed some excellent resources which you can use to either brush up or thoughly learn these concepts. 

###### Comprehensive & Interactive Learning
  * [HTML & CSS Codecademy (~7 hours)](https://www.codecademy.com/learn/web)
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
- It's a 'hot' framwork [http://hotframeworks.com/](http://hotframeworks.com/)

### Section 1.1: Installation & Setup
- [Install Meteor](https://www.meteor.com/install)

Windows users may need to add meteor to there system path:  `%USERPROFILE%\AppData\Local\.meteor;`

Meteor comes with a command-line tool belt `meteor` used to create your project and run many of meteors commands. 

Create a Meteor project:
```bash
$ meteor create myTwitterClone
$ cd myTwitterClone
$ meteor
```

Open browser to localhost:3000 to see your application running.

### Section 1.2: Exploring the barebone Meteor app

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
- If the value of the `counter` changes, the helper tell the front-end that the HTML should render a new value

#### What is a Template?
- Where your actual UI lives 
- Primary composed of HTML which can be paired up with a front end templating engine
- Meteor uses [Blaze](https://www.meteor.com/blaze) for this

##### The HTML
```html
<head>  
  <title>twitterClone</title>
</head>

<body>  
  <h1>Welcome to Meteor!</h1>

  {{> hello}}
</body>

<template name="hello">  
  <button>Click Me</button>
  <p>You've pressed the button {{counter}} times.</p>
</template>
```
The head & body html syntax should be familiar to you all. Let's cover the blaze components now. 

##### Template - hello
- `{{> hello}}` is a blaze/handlebar syntax for adding in a template
- The hello template is defined in the HTML block `<template name="hello">`
- Using 'variables' like `{{counter}}` in your HTML template is another Blaze feature
- `{{counter}}` renders the output of you counter method defined in the Javascipt helper block `counter: function()`




## Part 2: Client Template JS

- In this part, we will be learning about client side template of Meteor
- The goal is to build a tweetbox
![Tweetbox](http://randomdotnext.com/content/images/2015/07/Screen-Shot-2015-07-11-at-10-47-06-AM.png)

##### Key features to implement
1. Change the character count as you type
2. The Button should be disabled if there is no text
3. Save the data to mongodb when you click Tweet

### Section 2.1: Adding Bootstrap to our Meteor project
- `meteor add twbs:bootstrap` and were done!
- Meteor offers LOTS of 3rd packages/libraries through [AtmosphereJS](https://atmospherejs.com/)
- `meteor add package-name` is the syntax for addding these packages
- If your getting errors try `sudo meteor add twbs:bootstrap`

##### twitterClone.css
```css
.tweetbox-container {
  margin: 45px;
}

.tweetbox {
  background-color: rgb(240, 247, 254);
}

.btnGroup {
  margin-top: 10px;
  display: flex;
  align-items: center;
}

.charCount {
  margin-right: 10px;
  color: gray;
}

.errCharCount {
  margin-right: 10px;
  color: red;
}
```

##### twitterClone.html
```html
<head>  
  <title>twitterClone</title>
</head>

<body>  
  {{> tweetBox}}
</body>

<template name="tweetBox">  
  <div class="tweetbox-container">
    <div class="panel panel-default tweetbox col-md-6">
      <div class="panel-body">
        <!-- Text box for tweet content -->
        <textarea class="form-control" id="tweetText" placeholder="What's happening?" rows="3"></textarea>

        <!-- Character count & button -->
        <div class="pull-right btnGroup">
          <strong class="{{charClass}}">{{charCount}}</strong>
          <button class="btn btn-info pull-right" type="button" {{disableButton}}>Tweet</button>
        </div>

      </div>
    </div>
  </div>
</template>  
```

##### What is all this gibberish?
1. The textbox:
`<textarea class="form-control" id="tweetText" placeholder="What's happening?" rows="3"></textarea>`
- This is simply a textbox for users to input the tweet content


2. The character countdown:
`<strong class="{{charClass}}">{{charCount}}</strong>`
- We need to implment two function in the javascript helper blocks to handle `{{charClass}}` and `{{charCount}}`
- `{{charClass}}` should return the correct css class to be used. If # of characters is greater than max (140), then the text should be red. Blaze allows you to inject this HTML paramter from the Javascript!
- `{{charCount}}` should return an integer representing the # of characters remaining. This logic will be in javascript as well

3. The tweet button:
`<button class="btn btn-info pull-right" type="button" {{disableButton}}>Tweet</button>`
- If were over 140 characters, disable the button
- We'll implement disableButton function in the javascript helpers. 
- It should return a string, either be 'disabled' or empty.

##### Take-away
- Dynamically insert html elements, tags, classes, components, etc by using Meteor's blaze framework
- Easily modify what the UI render will look like without worrying about complicated front-end logic


### Section 2.3: Counter and Tweet Button Javascript
- Keeping a counter of character count in the tweetbox
- Meteor templates offer three lifecycle callback functions: `onCreated`, `onDestroyed`, and `onRendered`
- We can set the initial character counter to zero when the tweetBox template is rendered

``` javascript
Template.tweetBox.onRendered(function () {  
  Session.set('numChars', 0);
});
```
- We want to update the character counter when the user types content into the textbox
- We can add an event listener for input into #tweetText element. This syntax is quite similar to that of jQuery. tweetText is the id of the DOM element.
```javascript
Template.tweetBox.events({  
  'input #tweetText': function(){
    Session.set('numChars', $('#tweetText').val().length);
  }
});
```
- Lastly, we want helper methods to push session variable changes into the HTML
- Again, from the HTML, there are three variables we need to implement: {{charClass}}, {{charCount}}, and {{disableButton}}
```javascript
Template.tweetBox.helpers({  
  charCount: function() {
    return 140 - Session.get('numChars');
  },

  charClass: function() {
    if (Session.get('numChars') > 140) {
      return 'errCharCount';    //css class name
    } else {
      return 'charCount';       //css class name
    }
  },

  disableButton: function() {
    if (Session.get('numChars') <= 0 ||
        Session.get('numChars') > 140) {
      return 'disabled';
    }
  }
});
```
- Notice that the changes to numChars actually reactively notify these helper methods to push new values into the DOM
- However, if the helper method only has static values, it will not run when you update Session variables

### Section 2.4: Add Tweets to MongoDB:
- We need to add one more event listener inside of Template.tweetBox.events to insert data into mongoDB
```javascript
'click button': function() {  
  var tweet = $('#tweetText').val();
  $('#tweetText').val("");
  Session.set('numChars', 0);
  Tweets.insert({message: tweet});
}
```
Before the `isClient` block, simply define the mongoDb collection:
```javascript
Tweets = new Meteor.Collection("tweets");  
```
- This Mongodb collection is accessible by both the client and the server without any additional code!
- Now you can to try to tweet in the barebone tweetBox. This is the UI that you should see if you followed along:
![Tweetbox](http://randomdotnext.com/content/images/2015/07/Screen-Shot-2015-07-11-at-10-47-06-AM.png)

- We are not showing any tweets in the UI yet, so we need to use the mongodb console tool to see that the data is getting stored correctly.
- Meteor makes this process easy. In your project directory, you can type in
```bash
meteor mongo
```
- Mongo console should now show up in your terminal/cmd. 
- type in `db.tweets.find()`
```bash
meteor:PRIMARY> db.tweets.find()  
{ "_id" : "BbQczRMQp5FGQ39qa", "message" : "meteor is cool" }
```

- This is actually Amazing!
- We are inserting into the database directly in the client code.
- No need to be proxying db operations through a REST point
- This is the power of Meteor

## Part 3: User Accounts
- In this part we will be building a user authentication system

![User Authentication](http://randomdotnext.com/content/images/2015/07/loginExp.gif)

### Section 3.1: Meteor User Accounts Package
- Meteor makes the process of user management very easy. Let's go ahead and add the meteor packages:
```bash
meteor add accounts-ui accounts-password  
```
- The meteor packages comes with HTML template, you can go ahead and drop this in html:
```bash
{{> loginButtons}}
```

You will see a super basic signup/login UI:

![User Authentication](http://randomdotnext.com/content/images/2015/07/Screen-Shot-2015-07-11-at-1-51-50-PM-1.png)

- Meteor gives you user account management, complete with bcrypt, user db operations, session management, auth logic right out of the box
- We basically did a million things in one line...

If you try to create a user now, you can see in the mongodb console:
``` bash
meteor:PRIMARY> db.users.find()  
{ "_id" : "iw75fJLwPNXwDrPWN", "createdAt" : ISODate("2015-07-11T20:49:30.702Z"), "services" : { "password" : { "bcrypt" : "$2a$10$2Od1oJZm693aRzIyYNIVVO4XyAuU0pXrcsrHgvuu0p0MWbp1aSzGW" }, "resume" : { "loginTokens" : [ ] } }, "emails" : [ { "address" : "test@test.com", "verified" : false } ] }
```

### Section 3.2: Customize Login UI (Signup)
- Let's customize the user experience so it conforms to a twitter-like experience
- Let's create a user management template:

#### twitterClone.css
[CSS on GitHub](https://github.com/ruler88/twitterClone/blob/master/part3_userAuth/twitterClone.css)

#### twitterClone.html
```html
<template name="userManagement">

  <h4>New User?</h4>
  <div class="form-group">
    <input class="form-control input-sm" id="signup-username" placeholder="Username">
    <input class="form-control input-sm" id="signup-fullname" placeholder="Full Name (Optional)">
    <input class="form-control input-sm" id="signup-password" placeholder="Password" type="password">
  </div>

  <button type="button" class="btn btn-info fullbutton" id="signup">Sign up</button>

</template>  
```

#### twitterClone.js
```javascript
Template.userManagement.events({  
  'click #signup': function() {
    var user = {
      username: $('#signup-username').val(),
      password: $('#signup-password').val(),
      profile: {
        fullname: $('#signup-fullname').val()
      }
    };

    Accounts.createUser(user, function (error) {
      if(error) alert(error);
    });
  }
});
```
- We are creating a new template `userManagement`
- The javascript code is listening to clicks on `signup` button
- A new user variable is created and processed through `Accounts.createUser`

- `Accounts.createUser()` method is made available through the accounts-ui package
- It comes with a variety of customization and methods [(documentation)](http://docs.meteor.com/#/full/meteor_user)
- You can also add third-party authentication through services like twitter, facebook, google, etc. by adding their accounts packages.

### Section 3.3: Customize Login UI (Login)
Login module can be created similarly

#### twitterClone.html
```html
<template name="userManagement">  
  ...

  <h4>Already have an account?</h4>
  <div class="form-group">
    <input class="form-control input-sm" id="login-username" placeholder="Username">
    <input class="form-control input-sm" id="login-password" placeholder="Password" type="password">
  </div>

  <button type="button" class="btn btn-info fullbutton login" id="login">Log in</button>

</template>  
```

#### twitterClone.js
```javascript
Template.userManagement.events({  
  'click #login': function() {
    var username = $('#login-username').val();
    var password = $('#login-password').val();

    Meteor.loginWithPassword(username, password, function(error) {
      if(error) alert(error);
    });
  }
});
```
- `Meteor.loginWithPassword()` will try to log the user in via the username and password provided. 
- If the validation is correct, browser cookie will be set for you. If validation fails, the callback function will have error.

- Bring out your browser console and type in `Meteor.user()` to see the logged in user
- If your not logged in, this will return null

### Section 3.4: Customize Login UI 
- User should know when they're logged in and they can logout
- Let's put all the pieces we have built together to complete the user management lifecycle

#### twitterClone.html
```html
<body>  
  <div class="row">
    <div class="col-md-4 col-sm-4">{{> userManagement}}</div>
    <div class="col-md-8 col-sm-8">{{> tweetBox}}</div>
  </div>
</body>

<template name="userManagement">  
  <div class="user-container">
    <div class="panel panel-default userBox">
      <div class="panel-body">
        {{# if currentUser}}
        <!-- Message for logged in user -->
        <p>Hello <strong>@{{currentUser.username}}</strong>, welcome to twitterClone</p>

        {{else}}
        <!-- Log in module -->
        <h4>Already have an account?</h4>
        <div class="form-group">
          <input class="form-control input-sm" id="login-username" placeholder="Username">
          <input class="form-control input-sm" id="login-password" placeholder="Password" type="password">
        </div>

        <button type="button" class="btn btn-info fullbutton login" id="login">Log in</button>


        <!-- Sign up module -->
        <h4>New User?</h4>
        <div class="form-group">
          <input class="form-control input-sm" id="signup-username" placeholder="Username">
          <input class="form-control input-sm" id="signup-fullname" placeholder="Full Name (Optional)">
          <input class="form-control input-sm" id="signup-password" placeholder="Password" type="password">
        </div>

        <button type="button" class="btn btn-info fullbutton" id="signup">Sign up</button>
        {{/if}}

      </div>
    </div>
  </div>
</template>  
```
- We have put in some Blaze template code to show logged in user 
- `{{# if currentUser}}` `{{else}}` `{{/if}}` block will show a welcome message if user is logged in, or the login/signup form if the user is not logged in
- `currentUser` is calling a helper method that is part of Meteor user-accounts package [(doc)(http://docs.meteor.com/#/full/template_currentuser)

```html
<p>Hello <strong>@{{currentUser.username}}</strong>, welcome to twitterClone</p>  
```
- access the user's username through {{currentUser.username}} 
- currentUser returns the entire user object to the browser

Our user management module is now complete!

### Section 3.4: Logging out
Logging a user out is as easy as logging a user in.

#### twitterClone.html
```html
<button type="button" class="btn btn-info fullbutton" id="logout">Log out</button>  
```
#### twitterClone.js
```javascript
'click #logout': function() {  
  Meteor.logout();
}
```

### Section 3.5: Assign Tweets to Users
Now that we have user information. We can assign the currentUser username to each tweet

#### twitterClone.js
```javascript
Template.tweetBox.events({  
  'click button': function() {
    ...
    Tweets.insert({message: tweet, user: Meteor.user().username});
  }
});
```
- Meteor.user() is an accessibility method for developers to get current logged in user's information
 
We should also make sure only logged in users can post stuff 
```javascript
if (Meteor.user()) {  
  Tweets.insert({message: tweet, user: Meteor.user().username});
}
```

Additionally, let's prevent users from even clicking tweet if they aren't logged in:
```javascript
Template.tweetBox.helpers({

  disableButton: function() {
    if (Session.get('numChars') <= 0 ||
        Session.get('numChars') > 140 ||
        !Meteor.user()) {
      return 'disabled';
    }
  }

});
```
We will change the button message to make sure users understand why the post button is disabled in the UI

#### twitterClone.html
```html
{{#if currentUser}}
  <button class="btn btn-info pull-right" type="button" {{disableButton}}>Tweet</button>
{{else}}
  <button class="btn btn-info pull-right" type="button" disabled>Please Log In</button>
{{/if}}
```

Let's add a tweet and check out our database again!
```bash
meteor:PRIMARY> db.tweets.find()  
{ "_id" : "myxrQmPWbKDrKDcr9", "message" : "hello world", "user" : "l33thax" }
```


