test failure causes

* always sort queries (loan.payments.chronological.last vs loan.payments.last)
* prefer travel to freeze
* specific .days if adding/subtracting days (or else you might add/subtract seconds)
* always stub ENV vars... don't just set them (because it will cause tests after that to fail)
* use page load validations
* data cleanup -- don't use destroy because it will invoke validations; don't use DatabaseCleaner -- because will reuse ids; use delete_all for a table -- won't call validations, won't reuse ids

Software Craftsmanship Manifesto
Clean Code by Robert Martin (Uncle Bob)

Avant Event alert