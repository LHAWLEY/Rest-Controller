# REST Controller Challenge

## Summary
In this challenge we'll learn the conventions around request types (e.g., GET and POST) and paths (e.g., `/dogs`) in a CRUD application.

[CRUD][wikipedia crud] stands for create, read, update, and delete.  These are the basic behaviors for persisting data.  We insert new records into the database, read them from the database, update their values, and delete them from the database.  In order to trigger these behaviors in a web application, we need to make requests to specific routes: a route for inserting data into the database, a route for getting all the records from the database, a route for updating a record, etc.

| Description                        | Behavior | Request Type | Request Path     |
|------------------------------------|----------|--------------|------------------|
| Insert a dog record                | Create   | POST         | `/dogs`          |
| Select all dogs records            | Read     | GET          | `/dogs`          |
| Select one dog record              | Read     | GET          | `/dogs/:id`      |
| Form for creating a new dog record | Read     | GET          | `/dogs/new`      |
| Form for editing a dog record      | Read     | GET          | `/dogs/:id/edit` |
| Update a dog record                | Update   | PUT          | `/dogs/:id`      |
| Delete a dog record                | Delete   | DELETE       | `/dogs/:id`      |
*Table 1*.  Conventional request types and paths for CRUD behaviors (based on [table][railsguides routes table] in RailsGuides).

Imagine a dog adoption application with a `Dog` model. If a user wants to list a new dog for adoption, the user needs to navigate to the page with the form for providing information about the dog. Let's say that to get to the page with the form, the user clicks a "List Dog" link. Following convention, clicking that link should trigger a GET request to the path `/dogs/new`.  The response to that request would return the page with the form.  When the user completes the form and clicks the submit button, this should conventionally trigger a POST request to the path `/dogs`.  And there are conventions for the other behaviors as well (see Table 1).

These conventions are part of an approach to structuring an application known as [REST][wikipedia rest] (representational state transfer).  In this challenge we are going to refactor a working application whose developer did not follow REST-ful routing conventions.  We're going to update the application to follow routing conventions.


### Defining Route Handlers
```ruby
get '/dogs/:id' do
  # Implement route handler
end

put '/dogs/:id' do
  # Implement route handler
end

delete '/dogs/:id' do
  #Implement route handler
end
```
*Figure 1*. Defining route handlers for PUT and DELETE requests.

We can define route handlers for PUT and DELETE requests the same way that we define them for GET and POST request.  The method we use to define a handler (e.g, `:get` or `:put`) matches the request type, and we pass in the path and a block that says what to do when the request is made.  See how the request types and paths from Table 1 are translated into the route handlers in Figure 1.


### Making PUT and DELETE Requests in Sinatra
There is a browser limitation that we need to work around.  Browsers make GET and POST requests.  In addition to these, we need to make PUT and DELETE requests.

```HTML
<form method="post" action="dogs/<%= @dog.id %>">
  <input type="hidden" name="_method" value="put">

  <!-- continue form ... -->
</form>
```
*Figure 2*. Adding a `_method` parameter with the value `"put"` to a `POST` request made from a form submission.

We can tell Sinatra to interpret a POST request as a PUT or DELETE request.  We do this by sending a parameter named `_method` along with the other data.  We set the value of this parameter to the name of one of the browser-unsupported request types (i.e., PUT or DELETE).  This can be done in a form by adding a hidden input (see Figure 2).


## Releases
### Pre-release:  Set up the Working Application
We'll begin with a functioning application, but we need to do some setup to get it working.  First, we need to create, migrate, and seed the database.

Once the database is setup, let's start the server, and explore the application.  It's a simple, one-resource CRUD application.  It's an app for signing up to sing karaoke.  Users submit their names and the title of the song they'll perform, and an entry is added to the list.  Entries can be edited and deleted.


### Release 0:  Refactor Routes
We're going to refactor the routes in this application to follow REST-ful routing conventions.  Our application has all the behavior that it needs; we just need to refactor the code.

This refactor will involve updating our route handlers to match the conventional request types and paths.  We'll also need to update anywhere the old paths appear:  form actions, links, redirect paths, etc.


## Conclusion
We've been following conventions for Ruby style, table and class names, etc.  Now we can add conventions for routing to the list.  Why would it be beneficial to follow REST-ful routing conventions?  How would following the conventions affect other developers on our project?


[railsguides routes table]: http://guides.rubyonrails.org/routing.html#crud-verbs-and-actions
[wikipedia crud]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[wikipedia rest]: https://en.wikipedia.org/wiki/Representational_state_transfer
