# Lab Report 5 - Putting It All Together
### Part 1 - EdStem post
* Student Post:
  
  Hi! So I'm working on my ListExamples file and can't seem to make it work. I keep testing and keep getting this error
![Image](LR5ss.png)
  Based on this, I see that there is a bug with my merge method, although I am not sure what does it mean having error with java heap space. It also says OutOfMemoryError, and I am not sure how to fix this.
  
* TA Response:

  From what I am seeing here, your code is using large amount of memory. Try looking over your code where it creates a new object that may hold large datasets such as lists. 

* Student Response

  I tried your suggestion and as I was using a while method to add elements in a new list, I found out I am indexing it wrong thus causing an inifnite loop where the method keeps adding elements to the new list its supposed to return infinitely causing the out of memory error. Now the test cases seem to run fine! Thank you!

![Image](TestSuccess.png)

* Scenario Set up
  - buggy code
  ![Image](ListExamplesFile.png)
  - test case
  ![Image](TestFile.png)
  - bash script/ command lines ran
  ![Image](BashScriptFile.png)
  - To fix the bug, the indexing of the while loops are wrong. The student switched the indexing of the while loops thus need to change the indexing of `index1`, on lines 30 and 39 in the buggy code, to `index2` and indexing `index2`, on lines 34 and 43 in the buggy code, to `index1`. 


### Part 2 - Reflection
  On the second half of the quarter I learned so many things, but one thing that was interesting and new to me was the Vim editor. It was cool for me because I learned that we can edit our files with just using the terminal, although saving and commiting it takes more steps. I also like how we were able to build our own grading script to which allow us to know how some programs like gradescope works. Everything were pretty much new to me and was really helpful to be learned. 
