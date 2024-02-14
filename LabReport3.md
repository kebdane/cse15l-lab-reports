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

    List<String> expected = new ArrayList<>();

    expected.add("CSE15L");
    expected.add("CSE12");
    expected.add("CSE20");

    StringChecker sc = new StringChecker();

    assertSame(expected, ListExamples.filter(classes, sc));
	}

After:
@Test 
	public void testFilter() {
    List<String> classes = new ArrayList<>();

    classes.add("CSE15L");
    classes.add("CSE12");
    classes.add("CSE20");
    classes.add("CSE11");

    List<String> expected = new ArrayList<>();

    expected.add("CSE15L");
    expected.add("CSE12");
    expected.add("CSE20");

    ListExamples list = new ListExamples();

    assertEquals(expected, list.filter(classes, "CSE11"));
	}
```
* The failure inducing output would be the parameter of the `filter` method, `sc`, which is type `StringChecker`. This is because `StringChecker` is an interface to which we cannot instantiate and to be used, the List class should implement it and include an implementation of the method `checkString` as required by the `StringChecker` interface.
  
```
StringChecker sc = new StringChecker();

assertSame(expected, ListExamples.filter(classes, sc));
```
* A non-inducing failure would be if this `StringChecker` parameter is replaced by a `String` parameter that will account for the element the user wants to filter out of the list.
```
assertSame(expected, ListExamples.filter(classes, "CSE 11"));
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
--help  Print a brief help message.

EX. 1
danilaebdane@Danilas-MacBook-Pro docsearch % grep --help ./tec
hnical
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
danilaebdane@Danilas-MacBook-Pro docsearch %

EX. 2
danilaebdane@Danilas-MacBook-Pro docsearch % grep --help ./technical/biomed/1468-6708-3-1.txt
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
        [-e pattern] [-f file] [--binary-files=value] [--color=when]
        [--context[=num]] [--directories=action] [--label] [--line-buffered]
        [--null] [pattern] [file ...]
```
* `grep --help` is a command that prints out we can possibly use `grep`. It includes multiples command-line that we can combine when using `grep` such as typing in the terminal `grep [-A num] [-C[num]] [file ...]`, in this case we using `-A` and `-C` command-lines with `grep`. This is helpful because when we are unsure of what we can use `grep` for, we can use `--help` to remind us of some comman-line options for `grep`.
  
```
-q, --quiet, --silent
             Quiet mode: suppress normal output.  grep will only search a file
             until a match has been found, making searches potentially less
             expensive.
```
```
 --exclude
             If specified, it excludes files matching the given filename pat-
             tern from the search.  Note that --exclude patterns take priority
             over --include patterns, and if no --include pattern is speci-
             fied, all files are searched that are not excluded.  Patterns are
             matched to the full path specified, not only to the filename com-
             ponent.
```
```
-c, --count
             Only a count of selected lines is written to standard output.
```
Source for all command-line descriptions : built-in on mac terminal "man grep"


