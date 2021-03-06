---

# IMPORTANT NOTE: 

### Creating new OAuth clients on Heroku is currently disabled. If you want to spin up your own instance of log2viz, please contact me directly at dominic@heroku.com 

You can track progress on this issue here: https://github.com/heroku/log2viz/issues/36

---

# log2viz

http://log2viz.herokuapp.com/

Realtime analysis of your Heroku app logs.

## Installing Locally

### Requirements

* Heroku Toolbelt (https://toolbelt.heroku.com/)
* Ruby 1.9.3
* bundler

### Get the code

Clone the repository and install the required gems.

```bash
$ git clone git@github.com:heroku/log2viz.git
$ cd log2viz
$ bundle install
$ cp .env.sample .env
```

### Set up OAuth

`log2viz` uses OAuth to fetch your application’s logs. You can create a new OAuth client via the Heroku API:

```bash
$ curl -i -n -X POST \
-d "client[name]=myviz&client[redirect_uri]=http://localhost:5000/auth/heroku/callback" \
https://api.heroku.com/oauth/clients

HTTP/1.1 201 Created

{
  "id":"3f1057xxxxxxxxxxxxxxx",
  "name":"myviz",
  "description":null,
  "redirect_uri":"http://localhost:5000/auth/heroku/callback",
  "secret":"ac6f8a482c91b0540d8xxxxxxxxxxxxxxxxxxx",
  "trusted":false
}
```

As well as view your existing clients:

```bash
$ curl -i -n -X GET https://api.heroku.com/oauth/clients

HTTP/1.1 200 OK

[
  {
    "id":"3f1057xxxxxxxxxxxxxxx",
    "name":"myviz",
    "description":null,
    "redirect_uri":"http://localhost:5000/auth/heroku/callback",
    "secret":"ac6f8a482c91b0540d8xxxxxxxxxxxxxxxxxxx",
    "trusted":false
  },
  {
    ...
  }
]
```

In your application’s `.env`, set the `HEROKU_ID` and `HEROKU_SECRET` variables to those returned by the API.

### Start the server

```bash
$ foreman start
```

And you’re done! Your app will be running at http://localhost:5000

## Running on Heroku

### Create an application

```bash
$ heroku create -a myviz
```

### Create a new OAuth client

```bash
$ curl -i -n -X POST \
-d "client[name]=myviz-production&client[[redirect_uri]=https://myviz.herokuapp.com/auth/heroku/callback" \
https://api.heroku.com/oauth/clients
```

And set the appropriate variables on your Heroku app:

```bash
$ heroku config:set HEROKU_ID=xxxxxxxx HEROKU_SECRET=xxxxxx HEROKU_AUTH_URL=https://id.heroku.com
```

### Deploy

```bash
$ git push heroku master
```

Visit your app at https://myviz.herokuapp.com

## Meta

Released under the [MIT license](http://www.opensource.org/licenses/mit-license.php).
