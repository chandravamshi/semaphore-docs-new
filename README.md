# Semaphore Docs

Semaphore Docs, powered by [Middleman](http://middlemanapp.com) and Amazon S3.

[![Build Status](https://semaphoreci.com/api/v1/renderedtext/semaphore-docs-new/branches/master/badge.svg)](https://semaphoreci.com/renderedtext/semaphore-docs-new)


## Setup

Clone the repo and install all necessary gems with

```
$ git clone git@github.com:renderedtext/semaphore-docs-new.git
$ cd semaphore-docs-new
$ bundle install
$ cp data/credentials.yml.example data/credentials.yml
```

For writing new articles or making updates, feel free to leave dummy credentials
in `data/credentials.yml`.

## Writing

Pages are stored in `source/docs/`.

To view the blog locally run:

```
./server
```

which actually runs

```
$ bundle exec middleman server --port 5000
```

Now you can open [http://localhost:5000/docs](http://localhost:5000/docs).

To include a sign up link within a page, make sure the file extension is
`.md.erb` and use the following method:

```
[sign up for a free Semaphore account](<%= sign_up_path_with_referer %>)
```

### Troubleshooting

If you can't install `nokogiri` dependency on Mac OS, make sure to run in
terminal:

```
xcode-select --install
```

After `xcode-select` is finished installing, `nokogiri` should be able to
install.

### Categories

Categories will be automatically grabbed from the post heading:

```yml
---
layout: post
title: Custom database.yml
category: Ruby
---
```

If the page `/docs/ruby.html` exists, user will be able to reach it from the post
breadcrumbs. If the page doesn't exist, a page with the list of all posts in the
category will be automatically generated and displayed.

### Embedding images

All images must be in the PNG file format, and processed using
[ImageOptim](https://imageoptim.com/). If you do not have access to an OS X
machine, please notify us in the pull request, and we'll make sure to run them
through ImageOptim.

Give all images appropriate alt text, as well as the following CSS classes:

```html
<img src="/docs/assets/img/2012-06-14/semaphore-homepage.png" alt="Semaphore Homepage" class="img-responsive img-bordered">
```

### Escaping ERB

You must escape ERB code snippets in files with `.erb` extension
([via](https://github.com/middleman/middleman-syntax/issues/29)):

```
<%%= foo %>
```

## Deployment

_for Rendered Text people_

Simply run

```
./deploy
```

which does `bundle exec middleman build` and uploads the content to an S3
bucket using the AWS CLI. It requires a valid `~/.aws` configuration.

## Configuration

All sensitive credentials are stored in `data/credentials.yml` check
`data/credentials.yml.example` for more info about format of file.

## Importing content from Semaphore Blog

If you turn a blog post into a Semaphore Docs page you should include the
canonical url in the post meta data. For more info, visit the Semaphore Blog
[guidelines](https://github.com/renderedtext/semaphore-blog#moving-content-to-semaphore-docs).

## Updating APIv2 docs

To update the API v2 docs that are generated from the API specification file,
you need to update the specification file in the root of the repo:

For example, to fetch and use the API spec release '2.8.1' use the following
snippet in the root of the project:

``` bash
wget 'http://api-v2-specs.semaphoreci.com/api_specs_2_8_1.json' -O api_v2_specification.json
```

To list all available API specs releases, visit `http://api-v2-specs.semaphoreci.com`.

## Tags

We use tags in YML frontmatter to render topic-specific content or tracking code.
Currently only `ruby` tag is in use.
