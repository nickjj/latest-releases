# latest-releases ![CI](https://github.com/nickjj/latest-releases/workflows/CI/badge.svg?branch=master)

A command line tool that lets you keep tabs on the latest releases of your
favorite tools and libraries.

- [Why?](#why)
- [Installation](#installation)
- [Adding Projects to Check](#adding-projects-to-check)
- [Dealing with API Rate Limits](#dealing-with-api-rate-limits)
- [About the Author](#about-the-author)

## Why?

Often times I'm looking forward to new releases of specific software but
manually going to a GitHub releases page for each repo is boring, especially
if I want to track a bunch of libraries for a specific programming language.

This tool allows you to define a list of repos that you want to check for new
releases and after running the `latest-releases` command it will output
something similar to what you see below:

```sh
nick@kitt:~ $ latest-releases
Latest releases on GitHub:

docker/docker-ce          |  v19.03.11      |  2020-06-01
phoenixframework/phoenix  |  v1.5.3         |  2020-05-22
pallets/flask             |  1.1.2          |  2020-04-03
turbolinks/turbolinks     |  v5.3.0-beta.1  |  2019-03-11
```

It places the newest releases on top.

### Why not use various package manager commands to do this?

Certain programming languages like Ruby and Elixir have commands built into
their package manager tool to see the current and latest releases of a package
based on a project's package dependency file.

That's handy but often times I just want to keep a pulse on when a new release
is out, and not necessarily compare it to what I have in an existing project.
Plus I often want to see a list of libraries and tools across multiple
languages, along with services like Docker.

This tool lets you do all of that in a unified way by running 1 command.

## Installation

This tool depends on the [jq](https://github.com/stedolan/jq) library which
lets you parse JSON from the command line. The quickest way to install that is
to `brew install jq` or `apt-get install jq` depending on what OS you have.
Additional installation methods can be found on [jq's installation
wiki](https://github.com/stedolan/jq/wiki/Installation).

### Installing the `latest-releases` command line tool

This allows you to run `latest-releases` from any directory. If this is on your
personal dev box or something like that, feel free to adjust this to put it
into a local `bin/` directory that's on your path for your user.

```sh
sudo curl \
  -L https://raw.githubusercontent.com/nickjj/latest-releases/v0.1.0/latest-releases \
  -o /usr/local/bin/latest-releases && sudo chmod +x /usr/local/bin/latest-releases
```

The snippet above is set to use the latest release. If you're living on the
edge you can replace the tag name with `master`, but I don't recommend that
since it's not guaranteed to be stable and may change in the future.

## Adding Projects to Check

Right now this tool only supports grabbing project releases from GitHub but
that may change in the future.

Each source (such as GitHub) has its own configuration file that has a list of
projects to check.

#### Create an initial config directory

All configuration for this tool will exist in this directory, which you can
create by copying the command below:

`mkdir -p ~/.config/latest-releases`

#### GitHub

Configure GitHub repos to check by creating the file below:

`touch ~/.config/latest-releases/github`

Then open it in your text editor of choice and add a new line separated list of
repos that you want to keep tabs on. Each line should only include the
`repo/owner`, not the entire GitHub URL.

Here's an example of what that file could look like:

```
pallets/flask
phoenixframework/phoenix
turbolinks/turbolinks
docker/docker-ce
```

Feel free to add as many repos as you want within reason.

## Dealing with API Rate Limits

### GitHub

By default GitHub allows you to perform 60 API requests per hour for
unauthenticated requests.

This tool creates 2 API calls for each repo, so unless you have over 30
repos you should not hit any type of API rate limit if you run this tool
once a day or every couple of hours.

But, if you do have more you can easily get up to 5,000 requests per hour by
authenticating your account. You can do that by following the steps below:

1. Go to <https://github.com/settings/tokens>
2. Create a personal access token with no special permissions
3. Run: `touch ~/.config/latest-releases/auth`

Add these 2 lines to the above file, modify the values for your info and save
it:

```sh
export GITHUB_USERNAME="yourname"
export GITHUB_PERSONAL_ACCESS_TOKEN="youraccesstoken"
```

Now when you run `latest-releases` it will use your token to authenticate to
GitHub's API.

## About the Author

I'm a self taught developer and have been freelancing for the last ~20 years.
You can read about everything I've learned along the way on my site at
[https://nickjanetakis.com](https://nickjanetakis.com/). There's hundreds of
[blog posts](https://nickjanetakis.com/blog/) and a couple of [video
courses](https://nickjanetakis.com/courses/) on web development and deployment
topics. I also have a [podcast](https://runninginproduction.com) where I talk
to folks about running web apps in production.
