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

#### Command-line Option #1: `-iname`

**Example One:**
Working Directory `./techical/plos`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/plos$ find -iname "*pbio*"
./journal.pbio.0020001.txt
./journal.pbio.0020010.txt
./journal.pbio.0020012.txt
```
> The command searches for all the entries that have the name "pbio" surrounded with some string next to them and before them.  This is called a partial search

**Example Two**
Working Directory `./techical/biomed`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/biomed$ find -iname "*research*"
./gb-2000-1-1-research002.txt
./gb-2000-1-2-research0003.txt
./gb-2001-2-10-research0041.txt
```
> The command searches for all the entries that have the name "research" surrounded with some string next to them and before them.  This is called a partial search

This command is useful because it allows you to search for files without having to know the exact case or words that may be used in the name.


#### Command-line Option #2: `-type`

**Example One:**
Working Directory `./techical/government`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/government$ find -type f
./About_LSC/Comments_on_semiannual.txt
./About_LSC/commission_report.txt
./About_LSC/conference_highlights.txt
./About_LSC/CONFIG_STANDARDS.txt
```
> The command searches and results all files that are found in the working directory. This is denoted by the `f` following `-type`

**Example Two**
Working Directory `./techical/government`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/government$ find -type d
.
./About_LSC
./Alcohol_Problems
./Env_Prot_Agen
./Gen_Account_Office
./Media
./Post_Rate_Comm
```
> The command searches and results all directories that are found in the working directory. This is denoted by the `d` following `-type`

This command is useful because it allows you to search for a specific type of result, narrowing the search to be far more useful.

#### Command-line Option #3: `-size`

**Example One:**
Working Directory `./techical/plos`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/plos$ find -size +30k     
./pmed.0010028.txt
./pmed.0010036.txt
./pmed.0020018.txt
./pmed.0020045.txt
./pmed.0020059.txt
./pmed.0020073.txt
./pmed.0020103.txt
```
> The command searches for all the files in the working directory that have a size that is larger than 30 kilobytes

**Example Two**
Working Directory `./techical/911reports`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/911report$ find -size -70k
.
./chapter-10.txt
./preface.txt
```
> The command searches for all the files in the working directory that have a size that is smaller than 70 kilobytes

This command is useful because it allows you to search for files based on the size, narrowing the search parameters with a useful addition

#### Command-line Option #4: `-maxdepth`

**Example One:**
Working Directory `./techical/government`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/government$ find -maxdepth 2 -name "*.txt"
./About_LSC/Comments_on_semiannual.txt
./About_LSC/commission_report.txt
./About_LSC/conference_highlights.txt
./About_LSC/CONFIG_STANDARDS.txt
```
> The command finds all text files within the `/government` directory and its subdirectories up to a maximum depth of 2

**Example Two**
Working Directory `./techical/biomed`
```bash
chanabattura@SSVCBITSupport:/mnt/c/Users/chara/OneDrive/Desktop/CSE 15L/docsearch/technical/biomed$ find -maxdepth 1 -name "*.txt"
./1468-6708-3-1.txt
./1468-6708-3-10.txt
./1468-6708-3-3.txt
./1468-6708-3-4.txt
./1468-6708-3-7.txt
```
> The command finds all text files within the `/biomed` directory and its subdirectories up to a maximum depth of 1

This command is useful because it allows users to control the depth of directory traversal, ensuring searches are limited to specific levels.

---

## Sources Used:

- https://www.redhat.com/sysadmin/linux-find-command
- (Linux Manual Page for `find`) `man find`
