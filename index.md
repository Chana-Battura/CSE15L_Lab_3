# CSE 15L Lab Report 2
### by Charan Battula A17750911

---

This lab report will describe the analysis of discovering bugs in code and various commands that are used in a UNIX command.

---
## PART ONE

> The following code has an error, I checked with two test cases and fixed my code to show the process of correcting a bug.

#### Failure-inducing code block:
```java
static double averageWithoutLowest(double[] arr) {
  if(arr.length < 2) { return 0.0; }
  double lowest = arr[0];
  for(double num: arr) {
    if(num < lowest) { lowest = num; }
  }
  double sum = 0;
  for(double num: arr) {
    if(num != lowest) { sum += num; }
  }
  return sum / (arr.length - 1);
}
```

#### Successful input for the program
```java
@Test
public void testAverageDuplicate(){
  double[] input = {3, 2, 1, 4, 5, 6};
  assertEquals(4.0, ArrayExamples.averageWithoutLowest(input), 0);
}
```

#### Failure-inducing inputs for the program
```java
@Test
public void testAverageDuplicate(){
  double[] input = {3, 2, 1, 1, 4, 5, 6};
  assertEquals(4.0, ArrayExamples.averageWithoutLowest(input), 0);
}
```
> Here, the code fails when inputting a list where the lowest value is duplicated.

#### The symptoms from running the test
```bash
C:\Users\chara\OneDrive\Documents\GitHub\lab3>java -cp ".;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
..E.
Time: 0.011
There was 1 failure:
1) testAverageDuplicate(ArrayTests)
java.lang.AssertionError: expected:<4.0> but was:<3.3333333333333335>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:555)
        at org.junit.Assert.assertEquals(Assert.java:685)
        at ArrayTests.testAverageDuplicate(ArrayTests.java:22)

FAILURES!!!
Tests run: 3,  Failures: 1
```

#### The program with bug
```
static double averageWithoutLowest(double[] arr) {
  if(arr.length < 2) { return 0.0; }
  double lowest = arr[0];
  for(double num: arr) {
    if(num < lowest) { lowest = num; }
  }
  double sum = 0;
  for(double num: arr) {
    if(num != lowest) { sum += num; }
  }
  return sum / (arr.length - 1);
}
```

#### The program without bug
```
static double averageWithoutLowest(double[] arr) {
  if(arr.length < 2) { return 0.0; }
  double lowest = arr[0];
  for(double num: arr) {
    if(num < lowest) { lowest = num; }
  }
  double sum = 0;
  int count = 0;
  for(double num: arr) {
    if(num != lowest) { sum += num; }
    else{count++;}
  }
  return sum / (arr.length - count);
}
```
> The code did not correct the average with the count of duplicate values, which I corrected with the count variable.

---

## PART TWO
### The `find` command

> The `find` command is used to search for files and directories that we would have from the working directory.  We can add modifications 

#### `ls` command without any arguments
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java messages README
```
- When the command was run, the working directory was `~/lecture1` and after the command was run, the working directory stayed `~/lecture1` but listed out the directories and files in the directory.
- When this command is run without any arguments, the command lists out all of the directories and files in the directory itself.  The directories found in the working directory are listed as bold while the files are listed regularly 
- The output is not an error as shown above, the command was able to list all the files and directories found in the working directory.
- *It is important to note that **`messages`** was bold in the terminal and for formatting reasons couldn't be bold in the code block above.*

#### `ls` command with the path to a directory
```
[user@sahara ~]$ ls lecture1/
Hello.class  Hello.java  messages  README
```
- When the command was run, the working directory was `~/`, and after the command was run, the working directory stayed `~/` but listed out the directories and files in the directory specified.
- When this command is run with the path to a directory, the command lists all the files and directories in the specified directory.  The directories found in the working directory are listed as bold while the files are listed regularly
- The output is not an error as shown above, the command was able to list all the files and directories found in `lecture1\`.
- *It is important to note that **`messages`** was bold in the terminal and for formatting reasons couldn't be bold in the code block above.*

#### `ls` command with the path to a file
```
[user@sahara ~]$ ls lecture1/messages/en-us.txt 
lecture1/messages/en-us.txt
```
- When the command was run, the working directory was `~/`, and after the command was run, the working directory stayed as `~/` and instead listed the path to the file itself.
- When this command is run with the path to a file, the command `ls`, does not have a directory to list out files and directories simply list out the path of the file specified.
- The output is not an error as shown above, the command was able to list the path of the file itself
