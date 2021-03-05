# Running Locally

## Requirements

This is a static site that runs on Jekyll, running on Github. You can read all the [juicy details on their docs site](https://jekyllrb.com/docs). Jekyll requires the following to be installed:

* Ruby version 2.4.0 or higher
* RubyGems
* GCC and Make

## Running Locally

After getting all the prerequisites, download the repo:
`$ git clone https://github.com/switzerb/imposter.git`

Install the jekyll and bundler gems.
`$ gem install jekyll bundler`

Build the site and make it available on a local server.
`$ bundle exec jekyll serve --config _config.yml,_config_development.yml`

Browse to http://localhost:4000
