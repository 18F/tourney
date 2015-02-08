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

### Simple

Let's say we want to cycle between multiple hostnames. We would pick a "base name", e.g. `WORKER_HOST`, and set up our environment variables like so (note these are dummy values):

```bash
WORKER_HOST_0=hostzero.foo.com
WORKER_HOST_1=hostone.foo.com
WORKER_HOST_2=hosttwo.foo.com
# ...
```

The environment variables are of the format `<base>_<n>`, where `n >= 0`.

#### `Tourney.fetch(base)`

Within our app, we would then use Tourney to retrieve a random `WORKER_HOST_*` for use in that process/request/iteration:

```ruby
Tourney.fetch('WORKER_HOST') # => 'hosttwo.foo.com'
Tourney.fetch('WORKER_HOST') # => 'hostzero.foo.com'
# ...
```

Note that you don't have to specify how many `WORKER_HOST`s are available from your code – Tourney will figure it out!

### Grouped

Some environments come in sets, e.g. API keys+secrets. For these, set up your environment variables like so:

```bash
SOME_API_KEY_0=keyzero
SOME_API_SECRET_0=secretzero
SOME_API_KEY_1=keyone
SOME_API_SECRET_1=secretone
# ...
```

where the variable names are of the format `<base>_<suffix>_<n>`. Note there can be any number of `base`s, `suffix`es, and groups.

#### `Tourney.fetch_group(base)`

You then call `.fetch_group` with the `base`:

```ruby
credentials = Tourney.fetch_group('SOME_API')
credentials # => {'KEY' => 'keyone', 'SECRET' => 'secretone'}
credentials['KEY'] # => 'keyone'
credentials['SECRET'] # = > 'secretone'
```

Each call to `.fetch_group` would then pull up a new random group of key+secret pairs, where the return value is a hash with the `suffix`es as the keys.
