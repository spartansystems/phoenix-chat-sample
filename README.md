# Phoenix Booster Kit

The Phoenix Booster kit is a starting point for an umbrella app with web
delivery performed by the Phoenix Framework. The booster kit is meant to be a
stand alone json api.

## Development

This booster kit has been setup with docker so the only development dependencies
are Docker and Docker Compose. However, we also understand that everyone does
not like to use Docker. If you wish to manage your dependencies manually, a
brief list of dependencies is included.

### Usage

If you do wish to use Docker and Docker Compose, here are some commands you will
find helpful.

#### Installing Dependencies

Installing dependencies can be performed in a one step command.

```bash
$ docker-compose run web mix do deps.get, deps.compile, dialyzer.plt
```

Dialyzer is a static code analysis tool that is extemely helpful for catching
type related issues. Its usage is documented later in this readme.

#### Environment Files

The docker compose definition is configured to reference environment variables
from an `.envrc` file. A sample file is stored at `.envrc.sample` and the
environment file is gitignored.

#### Running the API

The API can be started with the standard Docker Compose `up` command. This will
ensure that Postgresql and the Phoenix server have started. You can still access
the api at `http://localhost:4000/api`.

```bash
$ docker-compose up
```

#### Running the Tests

It is highly recommended the `MIX_ENV` is set to `test` when executing the test
suite. You can do so with the `-e` option before specifying the Docker Compose
service you wish to run.

```bash
$ docker-compose run -e MIX_ENV=test web mix test
```

#### Running the Tests with Coverage

The `MIX_ENV` must be set to `test` in order to generate a coverage report.
Also, since this is an umbrella app, the `--umbrella` flag also needs to be
specified.

```bash
$ docker-compose run -e MIX_ENV=test web mix coveralls --umbrella
```

#### Running the Tests with HTML Coverage

In addition to generating a coverage report, a detailed HTML report is generated
for more in depth analysis and stored at `cover/excoveralls.html`.

```bash
$ docker-compose run -e MIX_ENV=test web mix coveralls.html --umbrella
```

#### Running Linters

For the Phoenix Booster Kit we are using
[credo](https://github.com/rrrene/credo) to check the codebase for consistency.
All of the CLI options for credo are available to you as well.

```bash
$ docker-compose run web mix credo
```

#### Running Static Code Analysis

The Phoenix Booster Kit is using
[dialyxir](https://github.com/jeremyjh/dialyxir) for static code analysis. All
of the CLI options for dialyzer are available to you.

```bash
$ docker-compose run web mix dialyzer
```

#### Running a Bash Prompt

If you don't feel like typing any of those commands, you can start a bash prompt
and run everything like normal.

```bash
$ docker-compose run web bash
```

## Deployment

The Phoenix Booster Kit can be deployed to heroku relatively easily. You should
install the [heroku cli](https://devcenter.heroku.com/articles/heroku-cli), once
you have done so, create a project with:

```bash
$ heroku create #{PROJECT_NAME} --buildpack "https://github.com/HashNuke/heroku-buildpack-elixir.git"
$ heroku config:set SECRET_KEY_BASE=$(mix phoenix.gen.secret) HOST_NAME=#{PROJECT_NAME} MIX_ENV=prod
```

The Phoenix build pack is not needed since the project was instantiated with
`--no-brunch`. Deployment can be configured from CircleCI or automatically with
GitHub. We recommend deploying from CircleCI since you will have more fine
grained control of your deployments.

## Dependencies

- Erlang (latest)
- Elixir (latest)
- Hex
- Rebar
- Phoenix
- Postgresql
