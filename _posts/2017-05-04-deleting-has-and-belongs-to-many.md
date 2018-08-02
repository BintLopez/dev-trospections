Deleting records from a has_and_belongs_to_many join table can be tricky.

Can either execute raw sql to do this or can use the power of monkey patching.

Write a new class on the fly in your console to define a model for your join.

class Foo < ActiveRecord::Base
	has_and_belongs_to_many :bars
end

class Bar < ActiveRecord::Base
	has_and_belongs_to_many :foos
end

^^ These are the classes that already exist, but lets say you want to delete only the join records.

In your console you can do a thing called monkey-patching which defines code on the fly. 

In your console you can then write...

class FoosBar < ActiveRecord::Base
	belongs_to :foo
	belongs_to :bar
end

Then you can use good ole ActiveRecord to query for FoosBar.where(foo_id: what_you_want_to_delete)

Note the naming here -- FoosBar. Why not foobar? You're working backwards from rails naming conventions here.

