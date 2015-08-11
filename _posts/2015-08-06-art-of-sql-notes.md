---
layout: post
title:  "Data Normalization SQL"
date:   2015-06-30 22:43:00
categories: sql relational database
---

violations are russian dolls

Generally, w/ relational dbs, id should not have meaning

First normal form (1NF):  atomicity (RECTANGLE)
* All characteristics and attributes dealing with must be atomic
* More efficient searches
* Database-guaranteed data correctness
* functional dependencies exist b/t cols
* attr in your columns are determinants of other attrs in your 

Second normal form (2NF):  check dependence on the whole key
* data redundancy
* query performance
* violation is when you can't add data w/o meeting other unrelated data

Third normal form (3NF):  check attribute independence (SQUARE)
* a properly normalized model protects against the evolution of requirements
* normalization minimizes data duplication
* no transitive dependencies (vs. functional dependencies)

BAD TABLE
Cats

(name, colors, age, long-hair, short-hair, diff-tail-color)

1NF
cats(id, name, age, hair-type)
hair-type(id, type)
color(id, cat-id, color)

...

cats (id, name, age, hair_type_id) --> cat belongs_to hairtype
hair_types (id, type_name) --> hairtype has_many cat
colors (id, cat_id, color)

...

cats (id, name, age, hair_type_id) --> cat belongs to hairtype
hair_types (id, type_name) --> hairtype has one cat
colors (id, color)
cats_colors (id, cat_id, color_id)


BUT if we did table cats_hair_types(id, cat_id, hair_type_id)
cats has_many hair_types through cats_hair_types

Natural join -- db assumes it's joining on columns that share the same name
- can't do this in rails due to naming conventions

POLYMORPHISM

brings together different types of objects in one single table
think identical schemas across tables made for different objects


BELONGS TO VS HAS ONE
* `has_one` It's the 'has many' side of a belongs_to/has_many relationship, when you only need one and now many.
* You know that x `belongs_to` y if y.id is a column on the x table. 

nils are smells that your table should be split up or redesigned

