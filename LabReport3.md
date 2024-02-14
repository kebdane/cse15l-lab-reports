# Lab Report 3

## Part 1

### Test case
```
Before:
@Test 
	public void testFilter() {
    List<String> classes = new ArrayList<>();

    classes.add("CSE15L");
    classes.add("CSE12");
    classes.add("CSE20");
    classes.add("CSE11");

    List<String> Expected = new ArrayList<>();

    Expected.add("CSE15L");
    Expected.add("CSE12");
    Expected.add("CSE20");

    StringChecker sc = new StringChecker();

    assertSame(Expected, ListExamples.filter(classes, sc));
	}

After:
@Test 
	public void testFilter() {
    List<String> classes = new ArrayList<>();

    classes.add("CSE15L");
    classes.add("CSE12");
    classes.add("CSE20");
    classes.add("CSE11");

    List<String> Expected = new ArrayList<>();

    Expected.add("CSE15L");
    Expected.add("CSE12");
    Expected.add("CSE20");

    ListExamples list = new ListExamples();

    assertEquals(Expected, list.filter(classes, "CSE11"));
	}
```
* The failure inducing output would be the parameter of the `filter` method, `sc`, which is type `StringChecker`. This is because `StringChecker` is an interface to which we cannot instantiate and to be used, the List class should implement it and include an implementation of the method `checkString` as required by the `StringChecker` interface.
  
```
StringChecker sc = new StringChecker();

assertSame(Expected, ListExamples.filter(classes, sc));
```
* A non-inducing failure would be if this `StringChecker` parameter is replaced by a `String` parameter that will account for the element the user wants to filter out of the list.
  
```
assertSame(Expected, ListExamples.filter(classes, "CSE 11"));
```

* Symptom (with and without bug)
  
```
Before:
danilaebdane@Danilas-MacBook-Pro lab4 % javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
ListTests.java:23: error: StringChecker is abstract; cannot be instantiated
    StringChecker sc = new StringChecker();
                       ^
1 error

After:
danilaebdane@Danilas-MacBook-Pro lab4 % java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListTests
JUnit version 4.13.2
JUnit version 4.13.2
.
Time: 0.01

OK (1 test)
```

* Before and After bug
  
```
Not fixed:
interface StringChecker { boolean checkString(String s); }

class ListExamples{

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }
}

Fixed:
interface StringChecker { boolean checkString(String s, String remove); }

class ListExamples implements StringChecker{

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  public List<String> filter(List<String> list, String remove) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(this.checkString(s, remove)) {
        result.add(s);
      }
    }
    return result;
  }

  public boolean checkString(String s, String remove){
    if(s.equals(remove)){
        return false;
    }
    return true;
}
}
```

* The fix addressed the issue as instead of creating an instantation of the interface, we made the `ListExample` class to implement it along with the method `checkString`. In this case, the interface is properly used and with the right `checkString` method implementation we are able to make the `filter` method work.

## Part 2

### grep command-line options:
```
-v, --invert-match
             Selected lines are those not matching any of the specified pat-
             terns.

Ex. 1
danilaebdane@Danilas-MacBook-Pro docsearch % grep -v '2' find-results.txt
technical/biomed/1468-6708-3-1.txt
technical/biomed/1468-6708-3-10.txt
technical/biomed/1468-6708-3-3.txt
technical/biomed/1468-6708-3-4.txt
technical/biomed/1468-6708-3-7.txt
technical/biomed/1471-5945-1-3.txt
technical/biomed/1471-5945-3-3.txt
technical/biomed/1476-069X-1-3.txt
technical/biomed/1476-4598-1-3.txt
technical/biomed/1476-4598-1-5.txt
technical/biomed/1476-4598-1-6.txt
technical/biomed/1476-4598-1-8.txt
technical/biomed/1476-9433-1-3.txt
technical/biomed/1477-5956-1-1.txt
technical/biomed/1477-7819-1-10.txt
technical/biomed/1478-1336-1-3.txt
technical/biomed/1478-1336-1-4.txt
technical/biomed/1478-7954-1-3.txt
...(cut short, so report is not too long)
danilaebdane@Danilas-MacBook-Pro docsearch %  

Ex. 2
danilaebdane@Danilas-MacBook-Pro docsearch % grep -v '1' find-results.txt
technical/biomed/ar297.txt
technical/biomed/ar309.txt
technical/biomed/ar328.txt
technical/biomed/ar383.txt
technical/biomed/ar387.txt
technical/biomed/ar407.txt
technical/biomed/ar408.txt
technical/biomed/ar409.txt
technical/biomed/ar422.txt
technical/biomed/ar429.txt
technical/biomed/ar430.txt
technical/biomed/ar624.txt
technical/biomed/ar68.txt
technical/biomed/ar745.txt
technical/biomed/ar750.txt
technical/biomed/ar774.txt
technical/biomed/ar778.txt
technical/biomed/ar79.txt
technical/biomed/ar792.txt
technical/biomed/ar795.txt
technical/biomed/ar799.txt
technical/biomed/ar93.txt
...(cut short, so report is not too long)
danilaebdane@Danilas-MacBook-Pro docsearch % 
```

* `grep -v '___' fileName` prints out the files that does not contain a specific pattern. This is useful when trying to cut the length of the file list depending on a pattern that we do not want to include.
  
```
-c, --count
             Only a count of selected lines is written to standard output.

Ex 1:
danilaebdane@Danilas-MacBook-Pro docsearch % grep -c '1' find-results.txt 
745
danilaebdane@Danilas-MacBook-Pro docsearch % 

Ex 2:
danilaebdane@Danilas-MacBook-Pro docsearch % grep -c '2' find-results.txt
748
danilaebdane@Danilas-MacBook-Pro docsearch % 
```

* `grep -c '___' fileName` counts and prints out the number of files containing the specific pattern. This is useful when we want to see how many files have the pattern we specified.
  
```
-e pattern, --regExp=pattern
     Specify a pattern used during the search of the input: an input
     line is selected if it matches any of the specified patterns.
     This option is most useful when multiple -e options are used to
     specify multiple patterns, or when a pattern begins with a dash
     (`-').
Ex 1:
danilaebdane@Danilas-MacBook-Pro docsearch % grep -e '1' find-results.txt
technical/biomed/1468-6708-3-1.txt
technical/biomed/1468-6708-3-10.txt
technical/biomed/1468-6708-3-3.txt
technical/biomed/1468-6708-3-4.txt
technical/biomed/1468-6708-3-7.txt
technical/biomed/1471-2091-2-10.txt
technical/biomed/1471-2091-2-11.txt
technical/biomed/1471-2091-2-12.txt
technical/biomed/1471-2091-2-13.txt
technical/biomed/1471-2091-2-16.txt
technical/biomed/1471-2091-2-5.txt
technical/biomed/1471-2091-2-7.txt
technical/biomed/1471-2091-2-9.txt
technical/biomed/1471-2091-3-13.txt
technical/biomed/1471-2091-3-14.txt
technical/biomed/1471-2091-3-15.txt
technical/biomed/1471-2091-3-16.txt
technical/biomed/1471-2091-3-17.txt
technical/biomed/1471-2091-3-18.txt
technical/biomed/1471-2091-3-22.txt
technical/biomed/1471-2091-3-23.txt
technical/biomed/1471-2091-3-30.txt
technical/biomed/1471-2091-3-31.txt
technical/biomed/1471-2091-3-4.txt
technical/biomed/1471-2091-3-8.txt
...(cut short, so report is not too long)
danilaebdane@Danilas-MacBook-Pro docsearch % 

Ex 2:
danilaebdane@Danilas-MacBook-Pro docsearch % grep -e '2' find-results.txt
technical/biomed/1471-2091-2-10.txt
technical/biomed/1471-2091-2-11.txt
technical/biomed/1471-2091-2-12.txt
technical/biomed/1471-2091-2-13.txt
technical/biomed/1471-2091-2-16.txt
technical/biomed/1471-2091-2-5.txt
technical/biomed/1471-2091-2-7.txt
technical/biomed/1471-2091-2-9.txt
technical/biomed/1471-2091-3-13.txt
technical/biomed/1471-2091-3-14.txt
technical/biomed/1471-2091-3-15.txt
technical/biomed/1471-2091-3-16.txt
technical/biomed/1471-2091-3-17.txt
technical/biomed/1471-2091-3-18.txt
technical/biomed/1471-2091-3-22.txt
technical/biomed/1471-2091-3-23.txt
technical/biomed/1471-2091-3-30.txt
technical/biomed/1471-2091-3-31.txt
technical/biomed/1471-2091-3-4.txt
technical/biomed/1471-2091-3-8.txt
technical/biomed/1471-2091-4-1.txt
technical/biomed/1471-2091-4-5.txt
technical/biomed/1471-2105-1-1.txt
technical/biomed/1471-2105-2-1.txt
technical/biomed/1471-2105-2-8.txt
technical/biomed/1471-2105-2-9.txt
...(cut short, so report is not too long)
danilaebdane@Danilas-MacBook-Pro docsearch % 
```

* `grep -e '___' fileName` opposite of `-v`, `-e` prints out the files that does contain a specific pattern. This is useful when trying to make a list of files depending on a pattern that we do want include.
  
```
-H      Always print filename headers with output lines.

Ex 1:
danilaebdane@Danilas-MacBook-Pro docsearch % grep -H '2002' find-results.txt
find-results.txt:technical/biomed/gb-2002-3-10-research0052.txt
find-results.txt:technical/biomed/gb-2002-3-10-research0053.txt
find-results.txt:technical/biomed/gb-2002-3-10-research0054.txt
find-results.txt:technical/biomed/gb-2002-3-10-research0055.txt
find-results.txt:technical/biomed/gb-2002-3-10-research0056.txt
find-results.txt:technical/biomed/gb-2002-3-11-research0059.txt
find-results.txt:technical/biomed/gb-2002-3-11-research0060.txt
find-results.txt:technical/biomed/gb-2002-3-11-research0061.txt
find-results.txt:technical/biomed/gb-2002-3-11-research0062.txt
find-results.txt:technical/biomed/gb-2002-3-11-research0065.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0071.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0072.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0075.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0077.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0078.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0079.txt
find-results.txt:technical/biomed/gb-2002-3-12-research0080.txt
...(cut short, so report is not too long)
danilaebdane@Danilas-MacBook-Pro docsearch %

Ex 2:
danilaebdane@Danilas-MacBook-Pro docsearch % grep -H '1468' find-results.txt
find-results.txt:technical/biomed/1468-6708-3-1.txt
find-results.txt:technical/biomed/1468-6708-3-10.txt
find-results.txt:technical/biomed/1468-6708-3-3.txt
find-results.txt:technical/biomed/1468-6708-3-4.txt
find-results.txt:technical/biomed/1468-6708-3-7.txt
danilaebdane@Danilas-MacBook-Pro docsearch % 
```
* `grep -H '___' fileName` prints out the files that does contain a specific pattern but with the fileName in front. This is useful when trying to make a lis depending on a pattern that we do want include but also knowing which file each content came from.
  
## Source for all command-line descriptions : built-in on mac terminal "man grep"


