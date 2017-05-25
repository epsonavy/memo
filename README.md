Install Symfony
===============
~~~~
sudo mkdir -p /usr/local/bin
sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
sudo chmod a+x /usr/local/bin/symfony
sudo apt-get install php-xml
sudo apt-get install php7.0-intl
~~~

what files I need to git ignore?


Realtime ssh:
===========
~~~
ssh -i "OpenfaceAWS.pem" ubuntu@ec2-54-67-28-176.us-west-1.compute.amazonaws.com
~~
Installing postgres:
===========
~~
su
apt-get update
apt-get install postgresql postgresql-server-dev-all
su postgres
psql
>ALTER USER postgres WITH PASSWORD 'student';
~~
How to import new DB to psql:
===========
~~~
http://www.postgresqltutorial.com/postgresql-restore-database/
in psql terminal:
>psql -U username -f backupfile.sql

>\dt to show all tables in current database
~~~
How to kill a screen:
===========

screen -X -S 1403.pts-0.ip-172-31-17-236 quit

How to fix app can not run:
=============================

xattr -rc /Applications/Hyper\ Light\ Drifter.app 


Callback in Node.js
=====================
~~~
dbConnection.js:

var mysql = require('mysql');
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'user',
    password: 'password',
    database: 'db'
});

exports.getUsers = function (callback) {
    connection.connect(function () {
        connection.query('SELECT * FROM Users', function (err, result) {
            if(!err){
                callback(result);
            }
        });
    });
};

~~~
The module would be called that way from a different node module:

app.js:

var dbCon = require('./dbConnection.js');
dbCon.getUsers(console.log);


Promise example
========

I recommend to take a look at MDN's Promise docs(https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) which offer a good starting point for using Promises. Alternatively, I am sure there are many tutorials available online.:)

Note: Modern browsers already support ECMAScript 6 specification of Promises (see the MDN docs linked above) and I assume that you want to use the native implementation, without 3rd party libraries.

As for an actual example...

The basic principle works like this:

Your API is called
You create a new Promise object, this object takes a single function as constructor parameter
Your provided function is called by the underlying implementation and the function is given two functions - resolve and reject
Once you do your logic, you call one of these to either fullfill the Promise or reject it with an error
This might seem like a lot so here is an actual example.

~~~
exports.getUsers = function getUsers () {
  // Return the Promise right away, unless you really need to
  // do something before you create a new Promise, but usually
  // this can go into the function below
  return new Promise((resolve, reject) => {
    // reject and resolve are functions provided by the Promise
    // implementation. Call only one of them.

    // Do your logic here - you can do WTF you want.:)
    connection.query('SELECT * FROM Users', (err, result) => {
      // PS. Fail fast! Handle errors first, then move to the
      // important stuff (that's a good practice at least)
      if (err) {
        // Reject the Promise with an error
        return reject(err)
      }

      // Resolve (or fulfill) the promise with data
      return resolve(result)
    })
  })
}

// Usage:
exports.getUsers()  // Returns a Promise!
  .then(users => {
    // Do stuff with users
  })
  .catch(err => {
    // handle errors
  })
~~~
