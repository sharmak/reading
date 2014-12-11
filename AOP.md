Aspect Oriented Programming
---------------------------
Definition
----------
Aspect oriented programming tries to solve the problem with cross cutting measures e.g. Let's say you have banking application where you have various functions like debit, credit. To be sure about what is happening in the system during run-time you might want to log all the function before and after the execution. In this case Logging is an aspect which is added to banking application.

Python
-------
Simple decorators and context libraries are examples of aspect oriented programming in python. There are few modules as well to do some more features but you can achieve most of the things using decorator pattern.

Java
-----
Spring uses aspect oriented programming. You inject object to the class using interceptors. I will write some examples on how to use it. 
