---
layout: post
title:  "The magic of ActiveRecord"
date:   2015-06-30 22:43:00
categories: jekyll update
---

Active Record Intern Lecture notes

- find, save, delete

ActiveRecord(Actual DB Connection) -- ruby gem
-->
Arel gem (short form of "ActiveRelation")



Arel:  Abstract Syntax Tree Builder
--> transforms your code into exact syntax need
Look up "Abstract Syntax Tree"

arel = Loan.arel_table
=> #<Arel::Table:0x007fb8b5c83f78

Loan.active.to_sql --> returns sql equivalent

ActiveRecord is built on top of arel

Arel give more flexibility in way you query the db -- for ex, rails doesn't give you the option of using "or". Arel will let you do this

.project (word come from relational algebra)
 --> projecting a copy of the original but with modifications

 Arel.star is the same as * in pgsql

 ala fikra
 cmd shift d -- searches for all occurences and puts cursor there, then can arrow around and press enter

 ActiveRecord:  the gem that allows us to define a 'model' where the model class has a direct relationship to a database table

 Everything class/model in the models dir should inherit from ActiveRecord::Base and should be represented in the db. If class doesn't inherit from AR and is in models dir, code will blow up


 LOOK UP ACTIVE RECORD CALLBACKS
 - what happens on a 'save' or 'create'


The Errors object 
a = Animal.new
a.save
--> returns false
a.save!
--> returns the stack trace
a.errors
--> returns the exact errors that a has raised

before_save -- can be used as a way to prevent saving but there's nothing to validate obj.valid? will return true

validate is implicitly called in rails models at the time of saving/committing to db. However, can use it with .valid? at any point in the life of a Class that's not a model in the db

save vs commit
-- commit -- transaction is complete
if you raise an error in an after_save hook, could prevent the data from saving and being committed to the db
- commit guarantees that the info in IN the db and that the transaction is complete






