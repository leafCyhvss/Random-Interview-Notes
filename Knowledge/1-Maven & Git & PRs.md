![[Pasted image 20230706131041.png]]
## Maven

### What is Maven ?

### Why do we need Maven?

### Roles

### Maven Project Architecture

### Respository & Pom.xml

#### Pom.xml Decomposing

#### Maven UIDs

#### Dependency

#### Dependency parent-inherit

#### Properties

#### Plugins

##### Example

##### Commands
```shell
mvn compile #Compiles source code and stores classes to target/classes

mvn validate #Validates project

mvn test #Runs tests

mvn clean #Deletes target directory

mvn clean package #Compiled code is packaged to WAR/JAR/deb etc

mvn clean install #Install the artifact in local repository(.m2 Repo)

mvn clean deploy #Copies the package(WAR/JAR/deb etc) to the remote

repository

mvn verify

mvn clean verify

# Options

mvn clean install -Dmaven.test.skip=true #Skips compiling and running tests

mvn clean install -DskipTests=true #Compiles but skips running tests

mvn clean install -Dmaven.test.failure.ignore=true #Compiles and executes tests but ignores if any tests failed

mvn verify Dit.test=TestName #Executes specified test

mvn clean install -T 4 # -T is used to specify number of threads used, default 2 i.e., 2 threads per CPU.

mvn clean install -X/--debug #Enables debug mode

mvn clean package -U/--update-snapshots #Force check on dependence updates

mvn dependency:purge-local-repository #Removes local repository

# Dependency Plugin

mvn dependency:analyze #Analyzes dependencies of the project

mvn dependency:tree #Prints dependency tree

mvn versions:display-dependency-updates #Displays dependency updates

mvn dependency:analyze -DignoreNonCompile=true #Shows unused dependencies
```

##### Intellij run maven

#### Maven Build Life Cycle (important)


### Maven Repositories

#### Maven Local Repository

#### Maven Central(Remote) Repository

#### Maven Remote(Private) Repository

#### Maven Dependency Search Sequence

## Questions

## Git(VCS - tool), Github(website)

### Commands

### Important Concepts

### How to raise a PR?(Pull Request)

### Git HW submission基本操作：（从git下载项⽬）

Referencies