# Learning More: Explore the App Used in This Walkthrough<a name="gettingstarted-linux-explore-app-source"></a>

This topic describes the app that AWS OpsWorks Stacks deploys to the instance for this walkthrough\.

To see the app's source code, extract the contents of the [opsworks\-windows\-demo\-nodejs](https://github.com/awslabs/opsworks-windows-demo-nodejs) GitHub repository to an empty directory on your local workstation\. You can also log in to the instance that you deployed the cookbook to and explore the contents of the `/srv/mylinuxdemoapp` directory\.

The `index.js` file contains the most significant code for the app:

```
var express = require('express');
var app = express();
var path = require('path');
var os = require('os');
var bodyParser = require('body-parser');
var fs = require('fs');

var add_comment = function(comment) {
  var comments = get_comments();
  comments.push({"date": new Date(), "text": comment});
  fs.writeFileSync('./comments.json', JSON.stringify(comments));
};

var get_comments = function() {
  var comments;
  if (fs.existsSync('./comments.json')) {
    comments = fs.readFileSync('./comments.json');
    comments = JSON.parse(comments);
  } else {
    comments = [];
  }
  return comments;
};

app.use(function log (req, res, next) {
  console.log([req.method, req.url].join(' '));
  next();
});
app.use(express.static('public'));
app.use(bodyParser.urlencoded({ extended: false }))

app.set('view engine', 'jade');
app.get('/', function(req, res) {
  var comments = get_comments();
  res.render("index",
    { agent: req.headers['user-agent'],
      hostname: os.hostname(),
      nodeversion: process.version,
      time: new Date(),
      admin: (process.env.APP_ADMIN_EMAIL || "admin@unconfigured-value.com" ),
      comments: get_comments()
    });
});

app.post('/', function(req, res) {
  var comment = req.body.comment;
  if (comment) {
    add_comment(comment);
    console.log("Got comment: " + comment);
  }
  res.redirect("/#form-section");
});

var server = app.listen(process.env.PORT || 3000, function() {
  console.log('Listening on %s', process.env.PORT);
});
```

Here's what the file does:
+ `require` loads modules that contain some dependent code that this web app needs to run as expected\.
+ The `add_comment` and `get_comments` functions write information to, and read information from, the `comments.json` file\.
+ For information about `app.get`, `app.listen`, `app.post`, `app.set`, and `app.use`, see the [Express API Reference](http://expressjs.com/4x/api.html)\.

 To learn how to create and package your app for deployment, see [Application Source](workingapps-creating.md#workingapps-creating-source)\.