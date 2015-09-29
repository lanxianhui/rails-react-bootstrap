# Rails-React Bootstrap

Handcrafted bootstrap for Rails & React development.

## Requirements

1. Ruby >= 2.2.3 (needed for newest rails build)
2. Node.js (for assets compilation)

## Decisions

Every feature in this repository is described below. Alternatives are evalutated.

### Rails 5 as default framework

- In my biased opinion Node.js is less reliable for web applications than Rails
- Rails still has active community and maintenance team
- Rails 5 incorporates [Rails API](https://github.com/rails-api/rails-api) that is better suited for SPA applications.
- In my experience developing web apps in light frameworks like pure Rack, Sinatra, Padrino are productive in very short run only. After a short time, you end up re-inventing the wheel.
- EventMachie-based frameworks like Cramp experience a lot of concurrency issues

Downsides:

- We don't need all features of Rails for good SPA application
- Rails & ActiveRecord are considered quite heavy and monolithic solutions

Becuse of those, I might consider [Lotus Framework](http://lotusrb.org/) after it gets stable release.

### PostgreSQL as default database

* After using MySQL for few years, I experienced a [lot of quirs](http://grimoire.ca/mysql/choose-something-else).
* PostgreSQL is [well-supported by Rails](http://blog.remarkablelabs.com/2012/12/a-love-affair-with-postgresql-rails-4-countdown-to-2013), most noticeably hstore.
* Heroku's preferred db is postgresql (there are mysql addons though).
* SQLite is easy for set-up but using it on development and postgresql on production can lead to production-only bug.
* We can use simple [queue_classic](https://github.com/QueueClassic/queue_classic) worker queue

Downsides:

* Currently there is no good, and free postgresql client for Mac. The ones I am aware of are pgAdmin, Navicat and PG Commander, and pgXplorer.

### Put ruby version in Gemfile

* It forces developers to use the same environment for application.
* Prevents application to run with different ruby version in production / staging.
* Use could .rbenv-version or .ruby-version file, but I don't find it good solution because it forces patch version of ruby, what can be cumbersome for collaborating developers. In practice there's not difference which patch version of ruby, so we shound not be forced to install 2.0.0-p247 if we have 2.0.0-p195 installed.

Downsides:

* You can't easily use different ruby version on development. For example you want to use Ruby 2.0.0 for faster execution and Ruby 1.9.3 on server. I think you shouldn't do that because it can introduce production-only bugs.
* You have to upgrade production to use ruby 2.0.0. It can be cumbersome, but definitely rewarding in long run.

### Don't user server-side templating languages or ruby helpers

It includes: [Slim](http://slim-lang.com/), [Haml](http://haml.info/), or even [ERB](http://apidock.com/ruby/ERB). Though beautiful they make it harder to create [universal](https://medium.com/@mjackson/universal-javascript-4761051b7ae9) (isomorphic) applications that can be rendered on both server-side and client-side. Universal applications make it easy to reason about front-end and back-end, develop them, and test them separately.

To achieve it, all rendering needs to happen either in browser, or in helper `node` process on the server-side. All such process needs is data to render: `view = render(data)`. The data can be passed directly or queried through an API: `data = query(params)`. All to all: `view = render(query(params))`.

### Use DATABASE_URL in production, instead database.yml config

* It is recommended for [12-factor applications](http://12factor.net/)
* It is common practice anyway

## License

As Rails, this project is [MIT-licensed](http://opensource.org/licenses/mit-license.php).
