avoid nested if staements.
use descriptive names.
if you need to write a whole line of comment to describe a method then that method name is wrong.comments explain why not what, because the code already says what it does
a method's scope should not go beyond one specific functionality.
we do not push code to main, first is feature branch then pr to develop branch if available then main or feature pr to main if develop does not exist

a variable, function, or classname should say what it does.if you need to write a whole line of comment to describe then that method name is wrong 
delete commented out code intead of leaving it as souvenir.
keep functions as small as possible with one level of abstraction with as few parameters as resonably possible
DRY Dont Repeat yourself. if youre pasting the same logic a second time then it belongs in a shared function instead.
a class or module should have one responsibility, if you're using and to describe it then its doing too much

handle errors properly, use exceptions, not silently ignored error codes or magic return values, do not pass or return null where it can be avoided
keep tests clean too, a messy test file is technical debt, tests should be fast, independent of each other,repeatable, self verifying
