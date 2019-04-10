# Sanic + Vue.js + Webpack Web Application Template

## Features
* Minimal Sanic 19.3 App
* Vue.js 2.0
* [vue-cli 3](https://github.com/vuejs/vue-cli/blob/dev/docs/README.md) + yarn
* [Vuex](https://vuex.vuejs.org/)
* [Vue Router](https://router.vuejs.org/)
* [Axios](https://vuex.vuejs.org/) for backend communication

_inspired by https://github.com/gtalarico/flask-vuejs-template_

## Demo
[Live Demo](https://sanic-vue-template.herokuapp.com/#/api)

## Template Structure

The template uses Sanic to create a minimal REST style API, and let's VueJs + vue-cli handle the front end and asset pipline.
Data from the python server to the Vue application is passed by making Ajax requests.

### Application Structure

#### Rest Api

The Api is served using a Sanic blueprint at `/api/v1/`.

#### Client Application

A Sanic view is used to serve the `index.html` as an entry point into the Vue app at the endpoint `/`.

The template uses vue-cli 3 and assumes Vue Cli & Webpack will manage front-end resources and assets, so it does overwrite template delimiter.

The Vue instance is preconfigured with Filters, Vue-Router, Vuex; each of these can easilly removed if they are not desired.

#### Important Files

| Location             |  Content                                   |
|----------------------|--------------------------------------------|
| `/app`               | Sanic Application                          |
| `/app/api_v1`        | Sanic Rest Api (`/api/v1/`)                |
| `/app/client.py`     | Sanic Client (`/`)                         |
| `/src`               | Vue App .                                  |
| `/src/main.js`       | JS Application Entry Point                 |
| `/public/index.html` | Html Application Entry Point (`/`)         |
| `/public/static`     | Static Assets                              |
| `/dist/`             | Bundled Assets Output (generated by `yarn build` |


## Installation

##### Before you start

Before getting started, you should have the following installed and running:

- [X] Yarn - [instructions](https://yarnpkg.com/en/docs/install#mac-stable)
- [X] Vue Cli 3 - [instructions](https://cli.vuejs.org/guide/installation.html)
- [X] Python 3
- [X] Pipenv (optional)
- [X] Heroku Cli (if deploying to Heroku)

##### Template and Dependencies

* Clone this repository:

	```
	$ git clone https://github.com/kevinqqnj/sanic-vue-template.git
	```

* Setup virtual environment, install dependencies, and activate it:

	```
	$ pipenv install --dev
	$ pipenv shell
	```

* Install JS dependencies

	```
	$ yarn install
	```


## Development Server

Run Sanic Api development server:

```
$ python run.py
```

From another tab in the same directory, start the webpack dev server:

```
$ yarn serve
```

Open `localhost:8080` in Browser during development.

The Vuejs application will be served from `localhost:8080` and the Sanic Api
and static files will be served from `localhost:5000`.

The dual dev-server setup allows you to take advantage of webpack's development server with hot module replacement.

Proxy config in `vue.config.js` is used to route the requests back to Sanic's Api on port 5000.

If you would rather run a single dev server, you can run Sanic's development server only on `:5000`, but you have to build build the Vue app first and the page will not reload on changes.

```
$ yarn build
$ python run.py
```


## Production Server

This template is configured to work with Heroku + Gunicorn and it's pre-configured
to have Heroku build the application before releasing it.

#### JS Build Process

Heroku's nodejs buidlpack will handle install for all the dependencies from the `packages.json` file.
It will then trigger the `postinstall` command which calls `yarn build`.
This will create the bundled `dist` folder which will be served by whitenoise.

#### Python Build Process

The python buildpack will detect the `Pipfile` and install all the python dependencies.

#### Production Sever Setup

Here are the commands we need to run to get things setup on the Heroku side:

	```
	$ heroku apps:create sanic-vue-template
	$ heroku git:remote --app sanic-vue-template
	$ heroku buildpacks:add --index 1 heroku/nodejs
	$ heroku buildpacks:add --index 2 heroku/python
	$ heroku config:set SANIC_ENV=production
	$ heroku config:set SANIC_SECRET=SuperSecretKey

	$ git push heroku
	```

## Advanced Template
Below features are considered in `sanic-vue-template-advanced` (release date: TBD):
* admin dashboard
* database: asyncpg + peewee
* aioredis/cache
* Sanic-Auth
* sanic-session
* aiojobs
* sanic-sentry
* sanic-jwt
* sanic-zipkin
* arq: async job queue
* aiofiles
