version: '2'

services:
  jekyll:
    image: raveious/jekyll-website
    volumes:
      - .:/site
    ports:
      - '4000:4000'
    environment:
      - TARGET_REPO=https://github.com/clarklab/chowdown.git
      - TARGET_BRANCH=master
    env_file:
      - ./.env
