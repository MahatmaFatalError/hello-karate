### Karate Starter

This is a GSG for Karate. Two parts

1. A SpringBoot based Rest service that acts as the system under test
2. Karate DSL scripts to test this

### The Rest Service

This is a standard springboot application that exposes two APIs

#### Hello API
```
$ curl localhost:8080/api/hello
Hello world!
```
The fancy version when a name is passed in as a parameter...
```
$ curl localhost:8080/api/hello?name=Daas
Hello Daas!
```

#### Person API 

Create a person
```
$ curl -X POST localhost:8080/api/person -H 'Content-type:application/json' -d '{"firstName": "John", "lastName" : "Doe", "age" : 30}'
42
```
Get a person by his/her id
```
$ curl localhost:8080/api/person/42
{"firstName":"John","lastName":"Doe","age":30}
```

### Setting Up Karate

The folder structure for Karate tests is given in the Karate documentation on
[folder structure](https://github.com/intuit/karate#folder-structure), but the 
summary is that:

* All tests are defined in `*.feature` files
* For every feature file package, you need to have an empty test-class in the same pacakge under `src/test/java`
* Karate recommends to keep the `*.feature` files in the same folder as the test-class
* The `<build>` section of your `pom.xml` needs a small tweak for this ..

In this example, we have two features `hello` and `person`. Their `*.feature` files and test-classes
are kept in `src/test/java/features/hello` and  `src/test/java/features/person` respectively

Note that the test-class file do NOT use the `*Test.java` file naming convention used by JUnit4 classes. This actually ensures
that these tests will not be picked up when when invoking mvn test (for the whole project) from the command line. 
But they can still be invoked from the IDE.

A `*.feature` file has the same syntax as [Gherkin/Cucumber](https://cucumber.io/docs/gherkin/reference/) 
and is also described in Karate [documentation](https://github.com/intuit/karate#script-structure). The
key points are 

* Lines that start with `#` are comments
* There are three sections
    * `Feature` : A name for the tests in this feature file
    * `Background` : The steps in this section are executed before every `Scenario` in that file.
    * `Scenario` : Each scenario is a test. A file can contain multiple Scenarios.
* Each scenario is described using
    * `Given` : setting up the test
    * `When` : the test action that will be performed
    * `Then` : the expected outcome(s)
    

The [`karate-config.js`](https://github.com/intuit/karate#karate-configjs) file (typically kept in the `src/test/resources` folder) contains the environment 
and global variables used by Karate. This is is basically a javascript function that returns
a JSON object. Which means that the file cannot contain any comment statements before the function body. 


### Running the tests

We have three types of tests - unit tests, Spring integration tests, and Karate tests. Ideally we want 
to be able to run them from both the command-line and the IDE. 

* Unit tests : are meant to run super fast
* Spring integration tests : run slower because the entire application context has to be created
* Karate tests : require the system under test to be running  


#### Running the Unit and Spring integration test

From the IntelliJ IDE, right click on `/test/java/com.daasworld.hellokarate` and "Run all tests"
From the command line, run `mvn test`

Note that the `maven surefire plugin` is configured to treat all `.java` files in `com.daasworld` as Test classes
and to ignore all tests in the `feature` folder


##### Running the Karate Tests

Karate does NOT start up the system under test. So first start up the application by running
```
mvn spring-boot:run
```

From the IntelliJ IDE right click on `/test/java/feature` and "Run all tests". You can also right click on the
Java test-classes or the `*.feature` files to run a subset of the tests

From the command-line, run `mvn test -Dtest=HelloRunner`

SCRATCHPAD

* Work in progress - how to run all the Karate Tests from the command line



### References

* [Karate](https://github.com/intuit/karate) github repo
* [Spring Boot](https://spring.io/projects/spring-boot)
