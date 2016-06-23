# Authentication, and Deploying Your App

### Objectives

_After this lesson, students will be able to:_

- Set up Google auth for a Firebase database
- Explain what hosting and deployment are, and why we need them.
- Explain the difference between a static site and a full-stack web application
- Name a couple of free hosting providers for static sites
- Deploy an app to Firebase

### Preparation

_Before this lesson, students should already be able to:_

- Explain the difference between front and and back end development.


--- 

## Firebase with Authentication (20 min)

We've been using Firebase with security controls off, but we can turn them on again and make people log in using Google OAuth, or through many other services, like Facebook and Twitter.

This is more secure, and it has the side benefit that we get access to their user information from those services so that we can greet them by name if we want, and also that we get an OAuth access token for that service, which can come in handy when, for example, accessing things like the Google Maps API.

Logging into Firebase through each of these services is slightly different, but it's all well-documented on the site. Google is the easiest, so we'll be doing that today.

### Setting up Google Auth in Firebase

#### Step One: Enable Google authentication

- Go to the Firebase console: [https://console.firebase.google.com/](https://console.firebase.google.com/)
- Click on the name of the database you created Monday for My Little Pony
- Click on "Auth" on the left
- Click on "Set up sign-in method"
- Click on "Google"
- Click on "Enable"
- Save

#### Step Two: Tighten up your security rules

Now that we have set up Google authentication, our next step is to reverse the change in the security rules that we made on Monday:

- Click on "Database" on the left
- Click on the "Rules" tab
- Click on "Learn More" (it should open a new browser tab and take you to it)
- Click on the "Default" tab down below.
- Copy the JSON code you find there.
- Go back to the Console tab in the browser.
- Paste the JSON code, replacing what is there.
- Click "Publish".

#### Step Three: Add authentication code to your `app.js` file

Add this code anywhere after your initial Firebase config code.

```js

// Set up auth:

var firebaseAuth = firebase.auth();
var provider = new firebase.auth.GoogleAuthProvider();

var token;
var currentUser;

// Log in
firebaseAuth.signInWithPopup(provider).then(function(result) {

  // This gives you a Google Access Token. 
  // You can use it to access the Google API
  // (can be useful for stuff like Google Maps).
  token = result.credential.idToken;
  console.log(result);

  // The signed-in user info.
  currentUser = result.user;
  console.log(currentUser);
  alert("Where were you on the night of June 2, 2014, " + 
        currentUser.displayName + "? Come on, 'fess up! We have your picture!");
  $('body').css('background-image', 'url(' + currentUser.photoURL + ')');

}).catch(function(error) {
  console.log(error);
});

// Logout, if you want to do that inside a click listener
// when the user clicks a "logout" button:

//firebaseAuth.signOut().then(function () {
//    console.log('logged out');
//}).catch(function (error) {
//    console.log(error);
//});

```

#### Step Four: Open your app in browser, from a locally run server.

- In the terminal, `cd` to the directory where your app's `index.html` file is.
- Type `http-server -p 3000`
- Go to `http://localhost:3000` in your browser




<a name = "opening"></a>
## Running your app locally, vs. hosted on a server (5 min)

So far everything we have developed has been developed _locally_, meaning what we've been building exists solely on our machines. This is extremely convenient, as you have complete control over your machine and can make changes to files or other aspect of your machine's environment whenever you want, as you're developing your app.

If we want users to be able to see our app, though, it has to be served from some other computer somewhere, that has a web server running and taking HTTP requests 24/7, and has the capacity and the reliability to be able to send responses to many users at a time. In other words, your app has to be "hosted" by a hosting provider.



---

<a name = "introduction1"></a>
## Deployment Introduction (15 min)

"Deployment" is the process of packaging up all your files and sending them to the hosting provider, as well as setting up with your hosting provider anything else that will be necessary to test and run your app. This can include things like going into the hosting provider's console and setting up authentication rules and other configuration variables, and in cases where your app has its own application server code that you want to run (which ours do not), you have to provide adequate instructions to your hosting provider for how to get that code up and running.

Today we'll talk about two good, free hosting providers we can use to deploy our app -- Firebase and Github Pages (although we probably won't get to deploying on Github till next Monday) -- and we'll briefly mention another one you may have heard about, Heroku, which is probably the third easiest way out there to deploy an app.

There are basically two kinds of site hosting out there:

1. **Static site hosting:** This is when the host has your HTML, CSS and JavaScript files (and maybe other resources like images) stored in folders, and serve them up when requested.

2. **Full-stack application hosting:** This is when you write server code that then runs on the hosting machine. This server code serves up all the kinds of pages listed above, plus it can do all kinds of other things - perform calculations, store data from multiple users, make requests to other servers, etc.

Firebase hosting and Github Pages are both static site hosters, while Heroku can run full-stack applications with server code.

Note that this does not mean that applications deployed with Github Pages or Firebase cannot make powerful use of third-party APIs -- like the Firebase Backend-as-a-Service database system itself -- to accomplish a great deal that you could normally accomplish by running your own server code. It just means that you're not writing any _custom_ server code yourself; all the code that you write for your app is JavaScript code, that will be run in someone's browser.

>Note: In its documentation Firebase likes to style itself a hoster of full-stack applications because you can call their Backend-as-a-Service API and make it do stuff and store your information, but they're putting on airs. They're actually a static site hoster just like Github Pages.


---

## Deploy with Firebase - Demo (30 min)

Deploying with Firebase is the easiest.

#### Step 1: install the Firebase command-line tools:

```
npm install -g firebase-tools
```

#### Step 2: from the directory where your project is, run

```
firebase init
```

You'll be asked a series of questions:

>What Firebase CLI features do you want to setup for this folder?

- Press __Enter__ for default answer (both a database and hosting)

>What Firebase project do you want to associate as default?

- Use arrow keys to go down to the database you created Monday, then press __Enter__

>What file should be used for Database Rules?

- Press __Enter__ for default answer (database.rules.json)

>What do you want to use as your public directory?

- Type __"."__

>Configure as a single-page app (rewrite all urls to /index.html)?

- Type __"y"__ (yes)

>File ./index.html already exists. Overwrite?

- Press __Enter__ for default answer (No)

#### Step 3: 

```
firebase deploy
```

and you're done! Go to the web address it provides you, and you should see My Little Pony.



---

## Independent Practice (? min)

Create a new app, that does anything you want, and deploy it to Firebase hosting. It can be as simple as an html page with an h1 tag that says "Hello World".

Bonus: Have it also use a Firebase database (without using Google auth), and store and retrieve information from that database.

Double bonus: Use Google Auth, and display the user's name (or other information).


<a name = "conclusion"></a>
## Conclusion (5 min)

Review class objectives and the following questions:

- How can you set up Google authentication to a Firebase database?
- What's the difference between a static site and a full-stack web application?
- How can you deploy an app to Firebase hosting?
- What is the purpose of a server?

