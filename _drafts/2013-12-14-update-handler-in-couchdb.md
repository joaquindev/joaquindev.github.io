---
layout: post
title:  "Update handler function using patch in CouchDB"
date:   2013-12-14 13:28
categories: couchdb update+handler patch 
---

Sources: 
http://www.packtpub.com/article/getting-started-with-couchdb-futon
https://github.com/moonmaster9000/intro_to_couchdb

#About CouchDB

##Commands

###Start up CouchDB server

```ruby
$couchdb
>Server: Apache CouchDB 1.5.0 (LogLevel=info) is starting.
Apache CouchDB has started. Time to relax.
[info] [<0.31.0>] Apache CouchDB has started on http://127.0.0.1:5984/
```
###Get all available databases

$curl http://127.0.0.1:5984/_all_dbs
>Server:[info] [<0.116.0>] 127.0.0.1 - - GET /_all_dbs 200

###Create a new database

$curl -X PUT http://127.0.0.1:5984/basictest
{"ok":true}
>Server: [info] [<0.106.0>] 127.0.0.1 - - PUT /basictest 201
201: The 201 Created status code tells the client that the resource the request was made against was successfully created (Source: http://guide.couchdb.org/draft/api.html)

$curl -X PUT http://127.0.0.1:5984/basictest
{"error":"file_exists","reason":"The database could not be created, the file already exists."}
>Server: [info] [<0.107.0>] 127.0.0.1 - - PUT /basictest 412
412:  If a database already exists a 412 error is returned. (Source: http://wiki.apache.org/couchdb/HTTP_database_API)

$curl -X GET http://127.0.0.1:5984/basictest
{"db_name":"basictest","doc_count":0,"doc_del_count":0,"update_seq":0,"purge_seq":0,"compact_running":false,"disk_size":79,"data_size":0,"instance_start_time":"1387445321680660","disk_format_version":6,"committed_update_seq":0}
>Server: [info] [<0.943.0>] 127.0.0.1 - - GET /basictest 200

###Create a document

$curl -X POST -H "Content-Type:application/json" -d '{"type":"customer", "name":"James Cameron", "location":"Boston, MA"}' http://127.0.0.1:5984/basictest
{"ok":true,"id":"69367e8c0805c80fc25341a50700015e","rev":"1-c3636f3db8b99a002dc65dec4fe60824"}
>Server: [info] [<0.366.0>] 127.0.0.1 - - POST /basictest 201

$curl -X POST -H "Content-Type:application/json" -d '{"type":"customer", "name":"Name2 Surname2", "location":"New York, LA"}' http://127.0.0.1:5984/basictest
{"ok":true,"id":"69367e8c0805c80fc25341a507000191","rev":"1-016431581216d403459a9f5a0c49bc6a"}
>Server:[info] [<0.1300.0>] 127.0.0.1 - - POST /basictest 201

We can also upload the document: 
 
$curl -X PUT http://127.0.0.1:5984/test/doc/file.txt -H "Content-Type: application/octet-stream" -d@file.txt




###Retrieve a document

$curl -X GET http://localhost:5984/basictest/69367e8c0805c80fc25341a50700015e
{"_id":"69367e8c0805c80fc25341a50700015e","_rev":"1-c3636f3db8b99a002dc65dec4fe60824","type":"customer","name":"James Cameron","location":"Boston, MA"}
>Server: [info] [<0.1069.0>] 127.0.0.1 - - GET /basictest/69367e8c0805c80fc25341a50700015e 200

With a JSON format using python mjson.tool: 
curl -X GET http://localhost:5984/basictest/69367e8c0805c80fc25341a50700015e 7a000015 | python -mjson.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   152  100   152    0     0  32326      0 --:--:-- --:--:-- --:--:-- 76000
curl: (6) Could not resolve host: 7a000015; nodename nor servname provided, or not known
{
    "_id": "69367e8c0805c80fc25341a50700015e",
    "_rev": "1-c3636f3db8b99a002dc65dec4fe60824",
    "location": "Boston, MA",
    "name": "James Cameron",
    "type": "customer"
}

###Adding Admin user 

1. Open Futon to the Overview and look at the bootom right corner
2. There is a message saying: "Everyone is admin. Fix this"
3. Click on Fix this.
4. Enter in a username and password that you want to use for your administrator account. 
>user: couchadmin / couchadmin (very unsecure password, for instance: http://strongpasswordgenerator.com/)
>Server: [info] [<0.1558.0>] 127.0.0.1 - - PUT /_users/org.couchdb.user%3Acouchadmin 201

###Anonymously accessing the _users database

$curl http://localhost:5984/_users/org.couchdb.user:your_username | python
-mjson.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    41  100    41    0     0   9697      0 --:--:-- --:--:-- --:--:-- 41000
>Server: [info] [<0.109.0>] 127.0.0.1 - - GET /_users/org.couchdb.user:your_username 404

$curl http://127.0.0.1:5984/couchadmin | python
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    44  100    44    0     0   7029      0 --:--:-- --:--:-- --:--:--  8800
>Server: 
[error] [<0.2502.0>] Could not open file /usr/local/var/lib/couchdb/couchadmin.couch: no such file or directory
[info] [<0.113.0>] 127.0.0.1 - - GET /couchadmin 404
404 Not Found

$curl http://127.0.0.1:5984/_users/couchadmin | python
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    41  100    41    0     0  20510      0 --:--:-- --:--:-- --:--:-- 41000
>Server: [info] [<0.2412.0>] 127.0.0.1 - - GET /_users/couchadmin 404
404 Not Found

$curl http://127.0.0.1:5984/_users/org.couchdb.user:couchadmin | python
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    41  100    41    0     0  16116      0 --:--:-- --:--:-- --:--:-- 20500
>Server: [info] [<0.2397.0>] 127.0.0.1 - - GET /_users/org.couchdb.user:couchadmin 404
404 Not Found

###Securing the _users database

1. Click on _users db
2. Click on Security
3. Each database contains lists of admins and members. Admins and members are each defined by "names and roles", which are lists of strings. 
4. Admins: Database admins can update design documents and edit the admin and member lists. 
5. Members: Database members can access the database. If no members are defined, the database is public. 
6. We are going to change the values of Roles for both Admins and Readers to ["admins"] so that only admins could read or alter the design documents and readers' list. We made the format of the roles ["admins"] because it accepts roles in the form of an array.

###Check the database is secure

$curl localhost:5984/_users/org.couchdb.user:admin
{"error":"unauthorized","reason":"You are not authorized to access this db."}
>Server: [info] [<0.2741.0>] 127.0.0.1 - - GET /_users/org.couchdb.user:your_username 401

###Access the database with security enabled

$curl couchadmin:couchadmin@localhost:5984/_users/_all_docs
{"total_rows":2,"offset":0,"rows":[
{"id":"_design/_auth","key":"_design/_auth","value":{"rev":"1-619db7ba8551c0de3f3a178775509611"}},
{"id":"org.couchdb.user:couchadmin","key":"org.couchdb.user:couchadmin","value":{"rev":"1-fa4d27e7df3077fe2e184e956c0358d5"}}
]}
>Server: [info] [<0.3234.0>] 127.0.0.1 - - GET /_users/_all_docs 200

##Design Documents

Source: http://guide.couchdb.org/draft/design.html

Design documents are a special type of CouchDB document that contains application code which is run inside a database. That's why the application API is highly structured. 

About **Document Modeling**: There are two main kinds of documents. The first kind is like a .doc file. Another idea is to create "virtual" documents by using views to collate data together. You can also treat documents as state machines, with a combination of user input and background processing managing document state. 

About the **Query Server**: It is written in JavaScript and can execute MapReduce views, update validation, show and list functions. 

About the **Design Document**: CouchDB is designed to work best when there is a one-to-one correspondence between applications and design documents. A *design document* is another CouchDB document with an special ID that begins with *_design/<name-of-id>*. 

For instance a DD can have sections like: _id, _rev, language, validate_doc_update, views, shows, _attachments, signatures, lib and so on.  In fact, MapReduce queries are stores in the view section. 

Once you define a DD it will be accesible over its URL like this: 

_design/calendar
curl http://127.0.0.1:5984/mydb/_design/calendar

###A basic design document example

1. Write a json file called *mydesign.json*: 
{
  "_id" : "_design/example",
  "views" : {
    "foo" : {
      "map" : "function(doc){ emit(doc._id, doc._rev)}"
    }
  }
}

2. Send it to CouchDB: 
$curl -X PUT http://127.0.0.1:5984/basictest/_design/example --data-binary @mydesign.json
{"error":"bad_request","reason":"invalid_json"}
>Server: [error] [<0.2739.0>] attempted upload of invalid JSON (set log_level to debug to log it)
[info] [<0.2739.0>] 127.0.0.1 - - PUT /basictest/_design/example 400

$curl -X PUT http://127.0.0.1:5984/basictest/_design/example --data-binary @mydesign.json
{"error":"unauthorized","reason":"You are not a db or server admin."}
>Server: [info] [<0.1781.0>] 127.0.0.1 - - PUT /basictest/_design/example 401

$curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example --data-binary @mydesign.json
{"ok":true,"id":"_design/example","rev":"1-230141dfa7e07c3dbfef0789bf11773a"}
>Server: [info] [<0.3306.0>] 127.0.0.1 - - PUT /basictest/_design/example 201

3. Query the view we've defined (a list of all documents except the design document)
$ curl http://127.0.0.1:5984/basic/_design/example/_view/foo
{"total_rows":2,"offset":0,"rows":[
{"id":"3a58fd61898faa0da9bbf09d69000438","key":"3a58fd61898faa0da9bbf09d69000438","value":"1-e7048977fc15cb476b0374199b8a4941"},
{"id":"3a58fd61898faa0da9bbf09d69000dba","key":"3a58fd61898faa0da9bbf09d69000dba","value":"1-967a00dff5e02add41819138abb3284d"}
]}
>Server: [info] [<0.2584.0>] 127.0.0.1 - - GET /basic/_design/example/_view/foo 200

*Remember: When trying to upload a JSON file, you need to specify the header like this: 
curl -X POST http://127.0.0.1:5984/basic -d '{}' -H "Content-Type: application/json"*

 ###Create a new Update Handler function

Source: http://stackoverflow.com/questions/2972068/couchdb-document-update-handlers-in-place-updates

1. Open your design document and add this code:
{"docs":
  [
    {"_id": "my_doc", "number": 23},
    {"_id": "_design/app",
      "updates": {
        "accumulate": "function (doc, req) {
                         var inc_amount = parseInt(req.query.amount);
                         doc.number = doc.number + inc_amount;
                         return [doc, \"I incremented \" +
                                      doc._id + \" by \" +
                                      inc_amount];
                       }"
      }
    }
  ]
}

Solution: Let's start easy: (Source: http://wiki.apache.org/couchdb/Document_Update_Handlers)


1. Add the section **updates** to your design document with a method we call *hello*:
   "updates": {
       "hello": "function(doc, req) {return;}"
   }

2. Now let's try to invoke our Update Handler, .To invoke a handler, use one of:
2.1 a PUT request against the handler function with a document id: /<database>/_design/<design>/_update/<function>/<docid>
Ex: curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example/_update/hello
{"error":"TypeError","reason":"{[{<<\"message\">>,<<\"point is undefined\">>},\n  {<<\"fileName\">>,\n   <<\"/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js\">>},\n  {<<\"lineNumber\">>,1500},\n  {<<\"stack\">>,\n   <<\"(\\\"_design/example\\\",[object Array],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1500\\n()@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1562\\n@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1573\\n\">>}]}"}
>Server: [error] [<0.4220.0>] OS Process Error <0.2032.0> :: {<<"TypeError">>,
                                                     {[{<<"message">>,
                                                        <<"point is undefined">>},
                                                       {<<"fileName">>,
                                                        <<"/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js">>},
                                                       {<<"lineNumber">>,1500},
                                                       {<<"stack">>,
                                                        <<"(\"_design/example\",[object Array],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1500\n()@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1562\n@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1573\n">>}]}}
[info] [<0.4220.0>] 127.0.0.1 - - PUT /basictest/_design/example/_update/hello 500
[error] [<0.4220.0>] httpd 500 error response:
 {"error":"TypeError","reason":"{[{<<\"message\">>,<<\"point is undefined\">>},\n  {<<\"fileName\">>,\n   <<\"/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js\">>},\n  {<<\"lineNumber\">>,1500},\n  {<<\"stack\">>,\n   <<\"(\\\"_design/example\\\",[object Array],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1500\\n()@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1562\\n@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1573\\n\">>}]}"}
We get an error:  Remember you have to write the whole function inline! It won't work otherwise

1. We write another Update Handler:
 "updates": {
       "in-place-query": "function(doc,req){ var field = req.query.field; var value= req.query.value; var message= 'set ' + field + ' to ' + value; doc[field] = value; return [doc, message];}"
   }

2. We try to call our update handler, following the Source: (http://stackoverflow.com/questions/2972068/couchdb-document-update-handlers-in-place-updates), we insert the example document: 

curl -X POST -H "Content-Type:application/json" -d '{"_id": "my_doc", "number": 23}' http://127.0.0.1:5984/basictest
{"ok":true,"id":"my_doc","rev":"1-8c9c19a45a7e2dac735005bbe301eb15"}
>Server:[info] [<0.23208.0>] 127.0.0.1 - - POST /basictest 201

Finally, we called the Update Handler: 
$curl -X PUT  http://localhost:5984/basictest/_design/example/_update/accumulate/my_doc?amount=15
I incremented my_doc by 15
>Server: [info] [<0.23132.0>] 127.0.0.1 - - PUT /basictest//_design/example/_update/accumulate/my_doc?amount=15 201
201 Success


CommonJS Modules 

Source: http://wiki.apache.org/couchdb/CommonJS_Modules

(See also the official documentation for this topic: http://docs.couchdb.org/en/latest/query-servers.html#require )

Since 1.1 release you can use CommonJS 1.0 modules in your map, show, list, update and validation function. Reduce functions can NOT use modules. When using CommonJS (CJS since now on) in map function you must place your CJS under the views/lib property in your Design document. 

So the first thing we learn is that CJS modules are placed under: views/lib, that is the path where you are going to find your data. Which means, our design document is going to look something similiar to this: 


{
"_id":"_design/test",
"language":"javascript",
"whatever": {},
"shows": {},
"views": {
lib: { 
foo: "exports.bar = 42;"
},
test: {
map: "function(doc) {emit(doc._id, require('views/lib/foo').bar);}"
}
}
}

Within a show function you can require CJS modules that are defined within objetct in your design document. The ID you pass to require() is a / delimited list of property names to resolve the module string within the design document. It's important to notice that all imports are relative to the design document root unless they start with ./ or ../, these are referred to as "relative require" statements. Relative require statments only work within CJS modules, they cannot be used diretly inside your show, list, update and validation functions. 

Let's incorporate  the example to our code, our test is going to try to use the *bar* property of *foo* which is being exposed using the CJS notation: *exports.*

Our document design looks like this right now: 

{
   "_id": "_design/example",
   "_rev": "6-f6c563e9e55c5daa50a90b9a7c67c42e",
   "views": {
       "foo": {
           "map": "function(doc){ emit(doc._id, doc._rev)}"
       }
   },
   "updates": {
       "in-place-query": "function(doc,req){ var field = req.query.field; var value= req.query.value; var message= 'set ' + field + ' to ' + value; doc[field] = value; return [doc, message];}",
       "accumulate": "function (doc, req) { var inc_amount = parseInt(req.query.amount); doc.number = doc.number + inc_amount; return [doc, \"I incremented \" + doc._id + \" by \" + inc_amount]; }",
       "lib": {
           "foo": "exports.bar = 42;"
       },
       "test": {
           "map": "function(doc) {emit(doc._id, require('updates/lib/foo').bar);}"
       }
   }
}

We have the "updates" section that allows us to use Update Handler functions, in this case, let's test our new update handler function: 

$curl -X PUT  http://localhost:5984/basictest/_design/example/_update/test/map

We get an error: "reason":"Expression does not eval to a function. ([object Object])"
>Server: [info] [<0.110.0>] 127.0.0.1 - - PUT /basictest/_design/example/_update/test 500
[error] [<0.110.0>] httpd 500 error response:
 {"error":"compilation_error","reason":"Expression does not eval to a function. ([object Object])"}

Solution: change the section test/map to "test":"function (...)" 

Now, we do: 

$curl -X PUT http://localhost:5984/basictest/_design/example/_update/test
{"error":"render_error","reason":"function raised error: (new TypeError(\"doc is null\", \"updates.test\", 1)) \nstacktrace: (null,[object Object])@updates.test:1\nrunUpdate(function (doc) {emit(doc._id, require(\"lib/foo\").bar);},[object Object],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:961\n(function (doc) {emit(doc._id, require(\"lib/foo\").bar);},[object Object],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1033\n(\"_design/example\",[object Array],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1517\n()@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1562\n@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1573\n"}
>Server: [info] [<0.1897.0>] 127.0.0.1 - - PUT /basictest/_design/example/_update/test 500
[error] [<0.1897.0>] httpd 500 error response:
 {"error":"render_error","reason":"function raised error: (new TypeError(\"doc is null\", \"updates.test\", 1)) \nstacktrace: (null,[object Object])@updates.test:1\nrunUpdate(function (doc) {emit(doc._id, require(\"lib/foo\").bar);},[object Object],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:961\n(function (doc) {emit(doc._id, require(\"lib/foo\").bar);},[object Object],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1033\n(\"_design/example\",[object Array],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1517\n()@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1562\n@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1573\n"}

You can see we are getting a 500 error, as we can see in the doc: Care should be taken to ensure that your JSON structures are valid, invalid structures will cause CouchDB to return an HTTP status code of 500 (server error). However, our error is due to the fact that we are not sending any DOC_ID, let's fix that: 

$curl -X PUT http://localhost:5984/basictest/_design/example/_update/test/my_doc
{"error":"invalid_require_path","reason":"Object has no property \"lib\". {\"_id\":\"_design/example\",\"_rev\":\"8-c3591fd24cf6ff066a80450317b3e08c\",\"views\":{\"foo\":{\"map\":\"function(doc){ emit(doc._id, doc._rev)}\"}},\"updates\":{\"in-place-query\":\"function(doc,req){ var field = req.query.field; var value= req.query.value; var message= 'set ' + field + ' to ' + value; doc[field] = value; return [doc, message];}\",\"accumulate\":\"function (doc, req) { var inc_amount = parseInt(req.query.amount); doc.number = doc.number + inc_amount; return [doc, \\\"I incremented \\\" + doc._id + \\\" by \\\" + inc_amount]; }\",\"lib\":{\"foo\":\"exports.bar = 42;\"}},\"_module_cache\":{}}"}
>Server: [info] [<0.2463.0>] 127.0.0.1 - - PUT /basictest/_design/example/_update/test/my_doc 500
[error] [<0.2463.0>] httpd 500 error response:
 {"error":"invalid_require_path","reason":"Object has no property \"lib\". {\"_id\":\"_design/example\",\"_rev\":\"8-c3591fd24cf6ff066a80450317b3e08c\",\"views\":{\"foo\":{\"map\":\"function(doc){ emit(doc._id, doc._rev)}\"}},\"updates\":{\"in-place-query\":\"function(doc,req){ var field = req.query.field; var value= req.query.value; var message= 'set ' + field + ' to ' + value; doc[field] = value; return [doc, message];}\",\"accumulate\":\"function (doc, req) { var inc_amount = parseInt(req.query.amount); doc.number = doc.number + inc_amount; return [doc, \\\"I incremented \\\" + doc._id + \\\" by \\\" + inc_amount]; }\",\"lib\":{\"foo\":\"exports.bar = 42;\"}},\"_module_cache\":{}}"}

Now, we have another 500 error but related to an "invalid_require_path", the reason is "Object has no property lib". So we FIX this by changing the path of the require statment to: "updates/lib/foo".

$curl -X PUT http://localhost:5984/basictest/_design/example/_update/test/my_doc
{"error":"render_error","reason":"function raised error: (new TypeError(\"result is undefined\", \"/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js\", 962)) \nstacktrace: runUpdate(function (doc) {emit(doc._id, require(\"updates/lib/foo\").bar);},[object Object],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:962\n(function (doc) {emit(doc._id, require(\"updates/lib/foo\").bar);},[object Object],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1033\n(\"_design/example\",[object Array],[object Array])@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1517\n()@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1562\n@/usr/local/Cellar/couchdb/1.5.0/share/couchdb/server/main.js:1573\n"}
>Server: [error] [<0.115.0>] httpd 500 error response:
 {"error":"render_error","reason":"function raised error: (new TypeError(\"result is undefined\", (...)

We get a "result is undefined" error, let's find out why. We FIX this problem by adding the return statement, the code finally looks like this: 

{
   "_id": "_design/example",
   "_rev": "20-4bdf0c4556585607b2d1f40e3251a945",
   "views": {
       "foo": {
           "map": "function(doc){ emit(doc._id, doc._rev)}"
       }
   },
   "lib": {
       "barexpuesta": "exports.bar = 42;"
   },
   "updates": {
       "accumulate": "function (doc, req) { var inc_amount = parseInt(req.query.amount); doc.number = doc.number + inc_amount; return [doc, \"I incremented \" + doc._id + \" by \" + inc_amount]; }",
       "test": "function(doc,req) {emit(doc._id, require('lib/barexpuesta').bar);return [doc, 'OK!'];}"
   }
}

$curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example/_update/test/my_doc/
OK!
>Server: [info] [<0.3751.0>] 127.0.0.1 - - PUT /basictest/_design/example/_update/test/my_doc/ 201

WE HAVE OUR FIRST UPDATE HANDLER FUNCTION + COMMONJS MODULE WORKING!

Now, let's install and try this NPM: 
JSON-Patch: A leaner and meaner implementation of JSON-Patch.

After giving a try to the JSON-Patch Usage code, let's try to include our lib in our doc design. We go back to our code and start trying to add the minified version of this lib. First of all we need to know how we can escape some characters like double quotes using this syntax: \"

Now, going back to our document that has this structure: 

{
   "_id": "my_doc",
   "_rev": "11-97a972e3b41ae2a84aab10f7524b071f",
   "number": 38
}

We want to use our JSON-Patch lib to change the number to the number we are going to send by our URL request, so the algorithm will be this: 

1. Client: send PUT request sending an object with the changes called "patches"
2. Server: in our Update handler, our doc will be visible just using "doc"
3. Server: executes the command -> jsonpatch.apply( doc, patches )

In fact, let's test this into the Chrome console: 

var doc = {_id:"my_doc",_rev:"11-8378437847384",number:38}
undefined
doc
Object {_id: "my_doc", _rev: "11-8378437847384", number: 38}
observer = jsonpatch.observe(doc)
Object {patches: Array[0], object: Object}
doc.number = 84;
84
var patches = jsonpatch.generate(observer);
undefined
patches
[
Object
op: "replace"
path: "/number"
value: 84
__proto__: Object
]

We see that this lib tells us that we will need to replace the property *number* in the server with the new value 84. So we will be sending our patches object. 

But first, let's create/hardcode our patch directly in the Update Handler function, so we can apply it directly to our doc which ID is *my_doc*

1. In our updates/test, we will define our patch object: 
var patches = [{op:"replace", path:"/number", value:3}]; 
Remember you have to escape ", we type in the code like this: 

   "test": "function(doc,req) {var patches = [{op:\"replace\", path:\"/number\", value:3}]; var ljp = require('updates/lib/libjsonpatch');return [doc, 'OK!'];}"
   }

2. We are going to try to apply the Patch to our Doc: 
jsonpatch.apply( doc, patches ); 

We are getting the error: {"error":"compilation_error","reason":"Expression does not eval to a function. (function(doc,req) {var patches = [{op:\"replace\", path:\"/number\", value:3}]; var ljp = require('updates/lib/libjsonpatch'); ljp.jsonpatch.apply( doc, patches );return [doc, 'OK!\n'];})"}

Let's fix this: 

1. We take out our array of patches: var patches = [{op:\"replace\", path:\"/number\", value:3}]; 
2. We take out everything and just leave this code: 

       "test": "function(doc,req) {var j=require('updates/lib/libjsonpatch').libjsonpatch;return [doc, 'OK!\n'];}"

And we still get the error, so there's something wrong with the way we are importing our Jsonpatch lib. 
Solved!

------------
------------
------------
------------

Sending the PATCH array in the request object 

Source: http://stackoverflow.com/questions/790757/import-json-file-to-couch-db

We already know how to send files with PUT command, so we are looking for a way of sending the array of changes to be applied directly: 

We create our json file *patches.json*: 

[{"op":"replace","path":"/number","value":3}]

$curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example/_update/test/my_doc --data @patches.json

Right now we need to know if the JSON object defined in patches.json is availabe inside the Update Handler function. 

Remember our DB: 

$curl -X GET http://127.0.0.1:5984/basictest
{"db_name":"basictest","doc_count":4,"doc_del_count":0,"update_seq":105,"purge_seq":0,"compact_running":false,"disk_size":426085,"data_size":5103,"instance_start_time":"1387639632240589","disk_format_version":6,"committed_update_seq":105}
>Server: [info] [<0.232.0>] 127.0.0.1 - - GET /basictest 200

And remember we can execute our Update Handler function: 

$curl -X PUT http://127.0.0984/basictest/_design/example/_update/test3/my_doc
>Server: [info] [<0.463.0>] 127.0.0.1 - - PUT /basictest/_design/example/_update/test3/my_doc 201

Now, let's remember our Update Handler function code: 

       "test3": "function(doc,req) {emit(doc._id);var patches = [{op:\"replace\", path:\"/number\", value:2}];require('updates/lib/libjsonpatch').apply( doc, patches );return [doc, 'OK! '];}"

So we need to access to the JSON data we send from our PUT client request, we send our JSON data: 

$curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example/_update/test/my_doc --data @patches.json

Talking in IRC channel, looking for an answer: 
Hello, I´m working on an Update Handler function that I´m calling like this: curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example/_update/test/my_doc --data @userdata.json. Then, userdata.json have this structure: {  "user": [  { "name": "john", "age": 18  } ] }. I'm having problems because I can't access to the variable "user" from my Update handler function. Is it under req.body or how can I access it?

Well... What we can do is try to understand and know more about the req object we receive in the Update Handler, to do so let's JSON.stringify in the Update Handler return the req object like this: 

return [doc, JSON.stringify(req)];

And the result is this: 

curl -X PUT couchadmin:couchadmin@localhost:5984/basictest/_design/example/_update/test3/my_doc --data @patches.json
{"info":{"db_name":"basictest","doc_count":4,"doc_del_count":0,"update_seq":118,"purge_seq":0,"compact_running":false,"disk_size":487525,"data_size":5485,"instance_start_time":"1387639632240589","disk_format_version":6,"committed_update_seq":118},"id":"my_doc","uuid":"366c23c6fa53148444f44961c30081e1","method":"PUT","requested_path":["basictest","_design","example","_update","test3","my_doc"],"path":["basictest","_design","example","_update","test3","my_doc"],"raw_path":"/basictest/_design/example/_update/test3/my_doc","query":{},"headers":{"Accept":"*/*","Authorization":"Basic Y291Y2hhZG1pbjpjb3VjaGFkbWlu","Content-Length":"121","Content-Type":"application/x-www-form-urlencoded","Host":"localhost:5984","User-Agent":"curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8y zlib/1.2.5"},"body":"{    \"patches\": [        {            \"op\": \"replace\",            \"path\": \"/number\",            \"value\": 8        }    ]}","peer":"127.0.0.1","form":{"{    \"patches\": [        {            \"op\": \"replace\",            \"path\": \"/number\",            \"value\": 8        }    ]}":""},"cookie":{},"userCtx":{"db":"basictest","name":"couchadmin","roles":["_admin"]},"secObj":{}}

Which after going and formating it with www.jsbeautifier.org we get: 

$curl - X PUT couchadmin: couchadmin@ localhost: 5984 / basictest / _design / example / _update / test3 / my_doc--data@ patches.json {
    "info": {
        "db_name": "basictest",
        "doc_count": 4,
        "doc_del_count": 0,
        "update_seq": 118,
        "purge_seq": 0,
        "compact_running": false,
        "disk_size": 487525,
        "data_size": 5485,
        "instance_start_time": "1387639632240589",
        "disk_format_version": 6,
        "committed_update_seq": 118
    },
    "id": "my_doc",
    "uuid": "366c23c6fa53148444f44961c30081e1",
    "method": "PUT",
    "requested_path": ["basictest", "_design", "example", "_update", "test3", "my_doc"],
    "path": ["basictest", "_design", "example", "_update", "test3", "my_doc"],
    "raw_path": "/basictest/_design/example/_update/test3/my_doc",
    "query": {},
    "headers": {
        "Accept": "*/*",
        "Authorization": "Basic Y291Y2hhZG1pbjpjb3VjaGFkbWlu",
        "Content-Length": "121",
        "Content-Type": "application/x-www-form-urlencoded",
        "Host": "localhost:5984",
        "User-Agent": "curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8y zlib/1.2.5"
    },
    "body": "{    \"patches\": [        {            \"op\": \"replace\",            \"path\": \"/number\",            \"value\": 8        }    ]}",
    "peer": "127.0.0.1",
    "form": {
        "{    \"patches\": [        {            \"op\": \"replace\",            \"path\": \"/number\",            \"value\": 8        }    ]}": ""
    },
    "cookie": {},
    "userCtx": {
        "db": "basictest",
        "name": "couchadmin",
        "roles": ["_admin"]
    },
    "secObj": {}
}

I've highlighted the fields we are interested in! So let's take a look at what we receive, in fact, we are now going to return just our JSON data file that we send: 

return [doc, JSON.stringify(req.body)];

Finally we basically have to use JSON.parse(req.body) and use the array of patches to update our document. 

THE END



