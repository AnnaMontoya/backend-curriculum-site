---
layout: page
title: Models, Migrations, and Databases
tags: migrations, databases, relationships, rails, migrations, activerecord
---

## Overview

In this lesson, we'll be coding along to create an example of an app that demonstrates a one-to-many and a many-to-many relationship.

We're going to add two tables (`directors`, and `actors`) to our database, and connect them to our existing `films` table.

What might the relationships look like? Let's emphasize figuring out the entities (aka tables), but also figure out some of the key data columns.

## Learning Goals

* Create one-to-many relationships at the database level using foreign keys.
* Create many-to-many relationships at the database level using join tables with foreign keys.
* Use `has_many` and `belongs_to` to create one-to-many and many-to-many relationships at the model level.
* Define primary and foreign keys, and describe conventions used to name the columns containing each in a database.

## Slides

Available [here](../slides/models_migrations_databases)

## Warmup

* In your own words, what is a migration?
* What are some things that we can do with a migration?
* What is the relationship between a migration, our database, and our schema?
* What does `rake db:rollback` do? When **wouldn't** I want to use it?

## One-to-Many Relationships

### At the Database Level: Directors

Let's do:

1. First, we'll create a migration and a model:

`rails generate migration CreateDirectors name:string`

or

`rails g migration CreateDirectors name`

The migration generator creates a migration and if we follow the working convention for rails the migration will be pre-populated.

Let's look at the migration inside of `db/migrate`:

```ruby
class CreateDirectors < ActiveRecord::Migration
  def change
    create_table :directors do |t|
      t.string :name

      t.timestamps
    end
  end
end
```

Now create a model file. `touch app/models/director.rb`
Inside `director.rb` add the code that hooks up our model to ActiveRecord.

```ruby
class Director < ApplicationRecord
end
```

### What about Movies?

What's the relationship between movie and director?

```bash
rails g migration AddDirectorsToMovies director:references
```

Let's look at what this migration creates.

What else do we need?

## Associations

### One-to-Many Relationships at the Model Level: Movie/Director

Let's implement some model-level associations using our handy methods.

* `has_many`
* `belongs_to`

Why do we need a foreign key at the database level and the `belongs_to` method in the model? What do each of these things allow for?

*In the console*:

Create a director. Create a movie.

What are different ways to associate movies with directors?

## Many-to-Many: Movies and Actors?

Let's first draw out the relationship for movies and actors.

```bash
rails g migration createActors name
```

```bash
rails g migration createActorMovies actor:references movie:references
```

```bash
rake db:migrate
```

Now create the models to go with these new tables.

*In the console*:

Create a actor.

What are different ways to associate movies with actors?

Need a refresher on associations? Click [here](http://guides.rubyonrails.org/association_basics.html).

## Notes

* common column types: `boolean`, `string`, `text`, `integer`, `date`, `datetime`
* `rake db:migrate` migrates development
* you (generally) don't need to run `rake db:test:prepare`; running `rake test` will load the schema to the test database

## Checks for Understanding

* What is a primary key?
* What is a foreign key?
* In what situation would one row of data have both primary and foreign keys?
* In what situation would two entities be related but *not* have foreign keys stored on either of the tables?
* What is Rails' convention for the column name of the primary key?
* What is Rails' convention for the column name of a foreign key?
* What are dev, test, and prod databases all about?
* What is the database.yml and how is it used?
