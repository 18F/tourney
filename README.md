# Tourney

A Ruby gem to cycle (round-robin style) through environment variables. Say you have servers you want to load-balance requests to in a simple way, or have multiple API keys that you need to switch between.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'tourney'
```

And then execute:

    $ bundle

## Usage

Let's say we want to cycle between multiple API keys. We would pick a "base name", e.g. `API_HOST`, and set up our environment variables like so:

```bash
API_HOST_0=hostzero.foo.com
API_HOST_1=hostone.foo.com
API_HOST_2=hosttwo.foo.com
# ...
```

Within our app, we would then use Tourney to retrieve a random `API_HOST_*` for use in that process/request/iteration:

```ruby
Tourney.fetch('API_HOST') # => 'hosttwo.foo.com'
Tourney.fetch('API_HOST') # => 'hostzero.foo.com'
# ...
```
