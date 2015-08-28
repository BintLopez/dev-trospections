rake db:migrate:up VERSION=[insertversionnumber]
rake db:migrate:down VERSION=[insertversionnumber]

the magic that comes w/ rake db:migrate
	-- rake db:structure:dump
	-- rake db:schema:dump
	(takes what db looks like and turns it into file)

rake db:schema:load
rake db:structure:load
(takes file and creates db out of its instructure)
(would overwrite my current project db into blank db based on the schema)

rake db:migrate:status

rake db:migrate:reset