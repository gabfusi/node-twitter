# Twitter for Node.js


This is a fork of [https://github.com/desmondmorris/node-twitter/](https://github.com/desmondmorris/node-twitter/).

---

An asynchronous client library for the Twitter [REST](https://dev.twitter.com/rest/public) and [Streaming](https://dev.twitter.com/streaming/overview) API's.

[![wercker status](https://app.wercker.com/status/624dbe8ad011852d1e01d7dc03941fc5/s/master "wercker status")](https://app.wercker.com/project/bykey/624dbe8ad011852d1e01d7dc03941fc5) [![NPM](https://nodei.co/npm/twitter.png?mini=true)](https://nodei.co/npm/twitter/)

```javascript
var Twitter = require('twitter');

var client = new Twitter({
  consumer_key: '',
  consumer_secret: '',
  access_token_key: '',
  access_token_secret: ''
});

var params = {screen_name: 'nodejs'};
client.get('statuses/user_timeline', params, function(error, tweets, response) {
  if (!error) {
    console.log(tweets);
  }
});
```

## Installation

`npm install twitter-enhanced`

## Quick Start

You will need valid Twitter developer credentials in the form of a set of consumer and access tokens/keys.  You can get these [here](https://apps.twitter.com/).  Do not forgot to adjust your permissions - most POST request require write permissions.

```javascript
var Twitter = require('twitter-enhanced');
```

## For User based authentication:

```javascript
var client = new Twitter({
  consumer_key: '',
  consumer_secret: '',
  access_token_key: '',
  access_token_secret: ''
});
```

Add your credentials accordingly.  I would use environment variables to keep your private info safe.  So something like:

```javascript
var client = new Twitter({
  consumer_key: process.env.TWITTER_CONSUMER_KEY,
  consumer_secret: process.env.TWITTER_CONSUMER_SECRET,
  access_token_key: process.env.TWITTER_ACCESS_TOKEN_KEY,
  access_token_secret: process.env.TWITTER_ACCESS_TOKEN_SECRET
});
```
## For Application Only based authentication:

You will need to fetch a bearer token from Twitter as documented [Here](https://dev.twitter.com/oauth/application-only), once you have it you can use it as follows.

```javascript
var client = new Twitter({
  consumer_key: '',
  consumer_secret: '',
  bearer_token: ''
});
```

Add your credentials accordingly.  I would use environment variables to keep your private info safe.  So something like:

```javascript
var client = new Twitter({
  consumer_key: process.env.TWITTER_CONSUMER_KEY,
  consumer_secret: process.env.TWITTER_CONSUMER_SECRET,
  bearer_token: process.env.TWITTER_BEARER_TOKEN
});
```

NB - You will not have access to all endpoints whilst using Application Only authentication, but you will have access to higher API limits.

## Requests

You now have the ability to make GET and POST requests against the API via the convenience methods.

```javascript
client.get(path, params, callback);
client.post(path, params, callback);
client.stream(path, params, callback);
```

## REST API

You simply need to pass the endpoint and parameters to one of convenience methods.  Take a look at the [documentation site](https://dev.twitter.com/rest/public) to reference available endpoints.

Example, lets get a [list of favorites](https://dev.twitter.com/rest/reference/get/favorites/list):

```javascript
client.get('favorites/list', function(error, tweets, response) {
  if(error) throw error;
  console.log(tweets);  // The favorites.
  console.log(response);  // Raw response object.
});
```

How about an example that passes parameters?  Let's  [tweet something](https://dev.twitter.com/rest/reference/post/statuses/update):

```javascript
client.post('statuses/update', {status: 'I Love Twitter'},  function(error, tweet, response) {
  if(error) throw error;
  console.log(tweet);  // Tweet body.
  console.log(response);  // Raw response object.
});
```

## Streaming API

Using the `stream` convenience method, you to open and manipulate data via a stream piped directly from one of the streaming API's. Let's see who is talking about javascript:

```javascript
var stream = client.stream('statuses/filter', {track: 'javascript'});
stream.on('data', function(event) {
  console.log(event && event.text);
});

stream.on('error', function(error) {
  throw error;
});

// You can also get the stream in a callback if you prefer.
client.stream('statuses/filter', {track: 'javascript'}, function(stream) {
  stream.on('data', function(event) {
    console.log(event && event.text);
  });

  stream.on('error', function(error) {
    throw error;
  });
});
```

**Note** twitter stream several types of events, see [the docs](https://dev.twitter.com/streaming/overview/messages-types) for more info. There is no canonical way of detecting tweets versus other messages, but some users have had success with the following strategy.

```javascript
_ = require('lodash')
const isTweet = _.conforms({
  contributors: _.isObject,
  id_str: _.isString,
  text: _.isString,
})
```

## Examples

* [Tweet](https://github.com/desmondmorris/node-twitter/tree/master/examples#tweet)
* [Search](https://github.com/desmondmorris/node-twitter/tree/master/examples#search)
* [Streams](https://github.com/desmondmorris/node-twitter/tree/master/examples#streams)
* [Proxy](https://github.com/desmondmorris/node-twitter/tree/master/examples#proxy)
* [Media](https://github.com/desmondmorris/node-twitter/tree/master/examples#media)
* [Chunked Media](https://github.com/desmondmorris/node-twitter/tree/master/examples#chunked-media)

## Contributors

Originally authored by  [@technoweenie](http://github.com/technoweenie)
 and maintained by [@jdub](http://github.com/jdub)

Currently maintained by [@desmondmorris](http://github.com/desmondmorris)

[And we cannot forget the community](https://github.com/desmondmorris/node-twitter/graphs/contributors)
