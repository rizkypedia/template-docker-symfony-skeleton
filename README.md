# template-docker-symfony-skeleton

This is a Symfony skeleton application with docker environment

## Prerequisits

Docker >= 20

## Installation

```
git clone https://github.com/rizkypedia/template-docker-symfony-skeleton.git

cd to the folder

git checkout develop

docker exec symfony-php composer install

Open http://localhost:8100 in the browser of your choice and you'll see the Symfony default page

## Install Symfony Webapp

If the skeleton does not suit your needs and you want to use a complete Symfony Web Application instead, then execute the following command:

```
docker exec symfony-php composer require webapp