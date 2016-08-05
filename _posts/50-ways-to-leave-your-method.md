next takes an argument 

next, break, redo not methods; language syntax

next only works from w/in the block it's being called in

can retry from within rescue
retry unless retries >= 3

ensure after rescue -- makes sure that code will run

throw and catch -- catch blocks get executed

don't use exceptions for flow control -- very costly if the stack is deep