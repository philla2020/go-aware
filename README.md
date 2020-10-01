# How to use the template

This content shows how to install and deploy the site.

## Installation
To install the theme do the following:

```shell
git submodule add https://github.com/google/docsy.git themes/docsy
git submodule update --init --recursive

sudo npm install -D --save autoprefixer
sudo npm install -D --save postcss-cli
```

## Update the theme

To update the theme you have to update the submodules:

```shell
git submodule update --init --recursive
```

## Deploy the site

To test run:
```shell
hugo server
```

To build the site type:
```shell
hugo
```

it will create the public folder into the project. To execute in an `nginx container` type:

```
docker run --rm --name some-nginx \
-v $PWD/public:/usr/share/nginx/html:ro \
-p 8080:80 -d nginx
```
for major info take a look [here](https://hub.docker.com/_/nginx).
