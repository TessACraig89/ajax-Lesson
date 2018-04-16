<!--
Creator: Cory Fauver
Adapted by: Alex White
Market: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# AJAX

| Objectives |
| :--- |
| Understand the purpose of AJAX |
| Use $.get to GET data from a web API |
| Use $.post to POST data from a web API |
| Understand $.ajax for more complex requests |
| Get familiar with $.ajax long form |
| describe the meaning of `method:`, `url:`, and `success:` keys in AJAX|


### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This lesson is important because:*
In the modern web, people expect to get the data they need on the page when they need it, without having to reload the entire page! There is one way to do this, and one way only. And that is AJAX. 

### Prerequisites
*Before this workshop, developers should already be able to:*

- be able to use functions and callbacks
- describe the request-response cycle and HTTP's role in it
- use jQuery

## APIs
![API gif](https://github.com/Giphy/GiphyAPI/blob/master/api_giphy_header.gif?raw=true)

An Application Program Interface (API) is the way in which you interact with a piece of software. In other words it is the interface for an application or a program.

  * You've browsed jQuery's API documentation - http://api.jquery.com/.  jQuery's API is basically the set of objects and functions it gives us above and beyond standard JavaScript.
  * Even an `Array` has an API. Its API consists of all the methods that can be called on it, such as: `.forEach`, `.pop`, `.length` etc. See the full list: `Object.getOwnPropertyNames(Array.prototype)`.
  * Organizations have *web APIs* to publicly expose parts of their services to the outside world, allowing people to send them queries and receive data (e.g. [GitHub API](https://developer.github.com/v3) ).  Web APIs are the kind of APIs we'll focus on today. 

When we read the documentation for an API, you can compare it to a contract. The documentation explains how you can interact with an API, and defines how the API will respond to your requests.
  

#### Some useful APIs
[Food2Fork](http://food2fork.com/about/api), [Twitter](https://dev.twitter.com/overview/documentation), [Spotify](https://developer.spotify.com/web-api/), [Google Books](https://developers.google.com/books/), [Google Maps](https://developers.google.com/maps/), [WeatherUnderground](https://www.wunderground.com/weather/api/), [Giphy](https://github.com/Giphy/GiphyAPI), [YouTube](https://developers.google.com/youtube/?csw=1#data_api),  etc.

#### Simplest API Ever

I made a tiny little JSON API for skateboard tricks! [Click here](https://kickflip-api.herokuapp.com/tricks) to check it out. 

It's what's known as a REST API, which we will talk much more about in the future. For now, it just means that you can see a collection of all skateboard tricks at `/tricks`, and see individual tricks by id at `/tricks/:id` where :id is the id of some trick that exists within the database. 

Check it out!

This is a boiled down web API. It has data you can fetch by hitting the right endpoint. You can GET or POST data to this API! We'll do that later. 


#### Learning to use a more complex API

A **GUI** (graphical user interface) exists to make an application more convenient for a sighted user to interact with visually. An **API** does something similar - except its users are developers, and they interact with the API through programming languages and specific protocols.  Learning to use an API is a little like learning to use a new application on your computer. Here are a few ways they're alike:

- You have to follow the rules set out by the API or GUI.  
- You can make guesses and experiment, but it's easier to start with some documentation.
- Practice using APIs or GUIs helps you find patterns that carry over to other programs. 



#### First Encounter with an API

Follow along as I show you how I'd initially investigate the [Spotify API](https://developer.spotify.com/web-api/).

1. Find documentation. 
1. Check for any restrictions (authorization, API key, wait time for approval, etc.). 
2. Pick an endpoint.
3. Try to go to that endpoint and inspect some data.


&#x1F535; **Activity**
```
With a partner, spend 10 minutes on the following:
- Pick one of the following APIs: Giphy, or OMDB (open movie database)
- look at the documentation, and answer these questions:
    1. What format of data does this API return?
    1. How would you access the data that you're interested in? For example if you want the weather in Dallas, but your API gives you weather in nested objects for the entire state of Texas, how would you access the Dallas weather?
    1. Does this API require an API key?
    1. Can you view an API endpoint url in your browser? Do it!
- 10 minutes
```   

## AJAX

__Asynchronous JavaScript And XML__ (AJAX) allows us to make requests to a server (ours or another application's) without refreshing the page.

Let's break that down:

__Asynchronous__ - not happening at the same time. Some of your code will be executed at a later time. Specifically, a callback will run when you get the results of your request - even if takes a while.  This waiting time won't hold up the performance of the rest of your page.

__XML__ - a format for structuring data so that it can be sent and received across the web. XML has mostly been replaced by JSON, and AJAX can be used with either JSON or XML. We will use JSON. 

You may also hear the term `XMLHttpRequest`. This is how vanilla JavaScript does AJAX. In fact, `window` object in the Browser has available to it another object, `XMLHttpRequest`. JavaScript's `XMLHttpRequest`s syntax is a bit wordy, so jQuery's shorthand (the `$.ajax()` and `$.get` function) is one of jQuery's most popular features.

* **Limiting page reloads makes our web apps feel *faster* and mostly gives our users a *better experience*.** Imagine if you experienced a full page refresh every time you "liked" a post on Facebook....  The requests we've made so far have been synchronous. 


![](http://www.cs.uky.edu/~paulp/CS316/CS316AJAX_html_m79697898.png)

![](http://www.cs.uky.edu/~paulp/CS316/CS316AJAX_html_m2fd58f08.png)


AJAX is the doorman! It knows what requests are out and what to do when they return. The code (hotel) can keep operating without waiting for a single request (guest) to complete.

![](http://photos.mandarinoriental.com/is/image/MandarinOriental/dmo-people-london-doorman?$DMOBanner$&crop=355,272,2624,913&anchor=1667,728)

#### How do we use AJAX?

## GET

The HTTP protocol was designed specifically for web browsers and servers to communicate with each other in a request/response cycle.

`GET` and `POST` are the most common verbs used in HTTP requests:

  * A browser will use `GET` to indicate it would like to receive a specific web page or resource from a server.
  * A browser will use `POST` to indicate it would like to send some data to a server.

Conveniently can use AJAX to make both `GET` and `POST` requests to servers. From the perspective of the server, it is just another request.

## AJAX Setup

Using jQuery's `$.ajax()` method, we can specify several parameters, including:

* What kind of request
* request URL
* data type
* callback function (which will run on successful completion of the AJAX request)

Here's some examples of the data we'll be looking at:
https://api.spotify.com/v1/artists/4TnXB5yHH3ACRKmSKhXmNx
https://api.spotify.com/v1/artists/0k17h0D3J5VfsdmQ1iZtE9

Let's try sending a `GET` request to my Trick API

```js
$.ajax({
  // What kind of request
  method: 'GET',

  // The URL for the request
  url: 'https://kickflip-api.herokuapp.com/tricks',

  // The type of data we want back
  dataType: 'json',

  // Code to run if the request succeeds; the JSON
  // response is passed to the function as an argument.
  success: onSuccess
});

// defining the callback function that will happen
// if the request succeeds.
function onSuccess(responseData) {
    console.log(responseData);
    // celebrate!
};
```

If we're doing a simple `GET` request, we can (and should) avoid the `$.ajax()` method and use the helper method `$.get()` instead. Here, we only need to pass in the request URL and callback function for the same AJAX request as the example above. The default dataType here is JSON, so no need to specify. 

Here's the same code using the nice `$.get()` syntax: 

```js
$.get("https://kickflip-api.herokuapp.com/tricks", (response) => {
  processResponse(response);
});
```

&#x1F535; **Activity**
```
- Create an HTML page with a JS page linked to it. 
- At the top of the JS page, write a `$.get` request that gets the trick with an id of 3
- console.log that trick object from within the callback of the `$.get` function
```


#### AJAX and Event Handlers

We can combine AJAX calls with any jQuery event handlers. You may want to execute an AJAX call when the user clicks and button or submits a form.

```js
var endpoint = 'https://api.spotify.com/v1/search?q=goodbye&type=artist'

// click event on button
$('button').on('click', function(event) {
  $.get(endpoint, (data) => {
    onClickReqSuccess(data)
  })
});

function onClickReqSuccess(data){
  console.log(responseData);
  // process data
}

```

Sometimes we'll need to send data to an API in order for it to process our requests. For example, the Giphy API requires a `q` query paramater for the search endpoint.  When searching Giphy with `$.ajax()`, you CAN include a `data` object or query string with a key-value pair inside indicating the value of `q`. 

```javascript
// submit event on form
$('form').on('submit', function(event){
  event.preventDefault();
  $.ajax({
    method: 'GET',
    url: endpoint,
    data: 'q=cats',
    dataType: 'json',
    success: onSubmitReqSuccess
  });
});

function onSubmitReqSuccess(responseData){
  console.log(responseData);
  // process data
}
```

Or you can just leave the query params in the string like we did with the spotify example :)

Sometimes we do have more complex requests where we would want to use `$.ajax`. For example if we want to send data from a form as a query string. Luckily, jQuery provides a method called `serialize()` that transitions form data into a string that we can easily plug into the `data` attribute, like this:

```javascript
// submit event on form
$('form').on('submit', function(event){
  $.ajax({
    method: 'GET',
    url: endpoint,
    // The data to send aka query parameters
    data: $("form").serialize(),
    dataType: 'json',
    success: onSubmitReqSuccess
  });
});

function onSubmitReqSuccess(responseData){
  console.log(responseData);
  // process data
}
```

As long as the form `<input>` fields have the proper `name` attribute (in this case, `q`), `serialize()` will make our perfect object!

```html
<input type="text" class="form-control gif-input" name="q" placeholder="search gifs">
```

&#x1F535; **Activity**
```
- Wrap your request in an event handler! Make a button and call the get request on a click
- Bonus: Append the data to the DOM!
- 8 minutes
```

## Post

For posting data, we can do things much the same way. But before we get into JS code, let's look at the simplest way to POST to an API. curl! 

#### Curl
```bash
curl -H "Content-Type: application/json" -X POST -d '{"name":"dolphin flip","trick_type":"flip trick", "example_video":"https://www.youtube.com/watch?v=ohSmVYEtlAo"}' https://kickflip-api.herokuapp.com/tricks
```

Curl is a way to make requests from your terminal and it's a great way to test out API endpoints. It has much of the same configuration as `$.ajax` has. POST declares the method, the url is at the end, the data we are sending goes after the -d tag. We specify we are sending JSON. You get the idea. 

#### $.post

Posting to our API is easy using jQuery's $.post method:

```js
let data = {"name":"varial flip","trick_type":"flip trick", "example_video":"https://www.youtube.com/watch?v=V-_O7nl0Ii0"};
$.post( "http://localhost:3000/tricks", {'trick': data}, (response) => {
	console.log(response)
})
```

OK, its a little trickier. (Get it?) 

You have to make sure the data you are posting is EXACTLY how the API wants it, or it won't work. If your POST isn't working, that's probably why. 

#### $.ajax for POSTs

```js
var book_data = {
  title: "The Giver",
  author: "Lowis Lowry"
};

$.ajax({
  type: "POST",
  url: "/books", // this is a "relative" link
  data: book_data,
  dataType: "json",
  success: function(data) {
    console.log(data);
  }
});
```

&#x1F535; **Activity**
```
- Add a POST method to your file posting some hardcoded data (as opposed to data taken from a form) to our trick API!
- Don't know skateboarding? Just type trickpedia into youtube and see what you come up with!
- 8 minutes
```

#### Handling Success and Failure

When everything goes well, we'll get a response with an HTTP *status* in the 200s. The `$.ajax` function checks this status to see if the request was sucessful.  If the status is in the 200s, it runs the success callback. 

We can't guarantee that an API will respond or respond quick enough. In these cases, the AJAX request will fail or give an error. Using the `$.ajax()` syntax, we can handle these cases by including `error` and `complete` attributes on our initial request:

```js
$.get("https://kickflip-api.herokuapp.com/tricks", (response) => {
  console.log(response);
}).success(function() {
	console.log('success');
}).error(onError);

function onError(xhr, status, errorThrown){
  /* perform this function if the
     response timed out or if the
     status code of the response is
     in the 400s or 500s (error)
     xhr: the full response object
     status: a string that describes
     the response status
     errorThrown: a string with any error
     message associated with that status */
};
```

&#x1F535; **Activity**
```
- Add a success and failure function
- Use an incorrect URL to test for failure
- 8 minutes
```

## Closing Thoughts
- APIs open an entire world of more complex projects! All you need to access them is an understanding of HTTP.  Now, though, you can access them using AJAX for a smoother, faster user experience. 
- The syntax of the `$.ajax()` function is complicated, but more practice will familiarize you with its uses and complexity. Check in on whether you can explain `method:`, `url:`, and `success:` without any outside resources.
- Also you can do all of this in Vanilla JS! The syntax is a bit more difficult, but it's absolutely doable. See references below.

## Additional Resources
- [You Might Not Need jQuery](http://youmightnotneedjquery.com/)
- [JavaScript docs for AJAX without jQuery](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [jQuery's `$.ajax()` documentation](http://api.jquery.com/jquery.ajax/)
- [Other possible syntax for `$.ajax()`](http://jqfundamentals.com/chapter/ajax-deferreds)
