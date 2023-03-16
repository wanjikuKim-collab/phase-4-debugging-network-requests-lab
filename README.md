# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted
On submission, the following error is displayed on the Network tab of the web console
```c
  {status: 500, error: "Internal Server Error",…}
error
: 
"Internal Server Error"
exception
: 
"#<NameError: uninitialized constant ToysController::Toys>"
status
: 
500
traces
: 
{,…}
Application Trace
: 
[{exception_object_id: 7280, id: 0, trace: "app/controllers/toys_controller.rb:10:in `create'"}]
Framework Trace
: 
[{exception_object_id: 7280, id: 1,…}, {exception_object_id: 7280, id: 2,…},…]
Full Trace
: 
[{exception_object_id: 7280, id: 0, trace: "app/controllers/toys_controller.rb:10:in `create'"},…]
```
  - How I debugged:
  Normally with this error(`status 500` - NameError) as described in the previous labs could be as a result of a typo. So I checked for this

  problem `Toys.create` instead of `Toy.create`. Modules are in singular.

- Update the number of likes for a toy

The number of likes wasn't going up. Error displayed on web console:
```c
VM370:1 Uncaught (in promise) SyntaxError: Unexpected end of JSON input
    at ToyCard.js:27:1
```
  - How I debugged:

  `Unexpected end of JSON input` error indicates a JSON formatted string wasn't returned

  On checking the `#update` action, it did not render json. So solution was to do this

  ```code
  render json: toy, status: :accepted
  ```

- Donate a toy to Goodwill (and delete it from our database)
The toy wasn't getting deleted on the database. it returned a ROuting Error on my terminal. On checking the Network tab it displayed this:
```c
{status: 404, error: "Not Found",…}
error
: 
"Not Found"
exception
: 
"#<ActionController::RoutingError: No route matches [DELETE] \"/toys/5\">"
status
: 
404
traces
: 
{Application Trace: [],…}
```

  - How I debugged:

  A 404 error indicates that we should check the rails server logs in the backend and add the required route to handle the HTTP verb + path.
  This is an error with the routing.

  solution: added the delete route in `routes.rb`
