# Development Environment Setup on Fedora 28

This is a short guide on how to set up the compiler and interpreter for prototype Xalgorithm Rules on **Fedora 28**. A similar process could be followed on most GNU/Linux distributions or within the Windows Subsystem for Linux. Current interpreter builds (July 2018) use JSON representations of tables, rules, package information and documents. Refer to the [project documentation](https://github.com/xalgorithms/general-documentation) for more information about the working implementation.

Configuring these repositories will prepare all prerequisites for contributing to the project by improving the rule interpreter.

## What is an Xalgorithm Rule?

A **Rule** is a table-oriented expression of a documented computational rule, typically in the domain of commerce or tax. Rules are part of a **Package**, which support the rule with revision history, tables, uniform resource identifier and terms of use.

The [Xalgorithms Alliance](https://www.xalgorithms.org) have laid the foundations for an *Internet of Rules*, where computational rules used by many parties in commerce and government are made accessible, enabling transparency and consistency in the aforementioned applications. Xalgorithms (External, Extensible Algorithms,) will support the automation of taxes, trade fees and algorithmic pricing, along with more ordinary transactions.

Please refer to the [project documentation](https://github.com/xalgorithms/general-documentation) or [Xalgorithms site](https://www.xalgorithms.org) to learn more about the project.

## Installation

After cloning these repositories and installing dependencies, all files that are required to build the compiler, run rules, and contribute to the interpreter will be installed.

**Project Repositories:**
- `lib-rules-int-scala`, the rule interpreter. [View on GitHub.](https://github.com/Xalgorithms/lib-rules-int-scala)
- `lib-rules-parse-ruby`, the rule parser. [View on Github.](https://github.com/Xalgorithms/lib-rules-parse-ruby)

**Rule Interpreter Dependencies:**
- [**SBT**](https://www.scala-sbt.org/), Scala Build Tool, for compiling and running the interpreter.
- [**OpenJDK 8**](http://openjdk.java.net/), a dependency for Scala.

**Rule Parser Dependencies:**
- [**RBENV**](https://github.com/rbenv/rbenv), "Ruby Environment" version manager.
- Dependencies for Ruby.
- Ruby packages to support the compiler.

### Clone Repositories 

It is recommended to clone the following repositories to the same folder in your home directory. For instructions on how to install and use git, please refer to [this tutorial](https://guides.github.com/introduction/git-handbook/). `sudo dnf install git` will install and prepare git for basic cloning.

```bash
mkdir Xalgo
cd Xalgo
git clone https://github.com/Xalgorithms/lib-rules-int-scala.git
git clone https://github.com/Xalgorithms/lib-rules-parse-ruby.git
```

### Install SBT

A version of SBT *is* available in the Fedora package manager, but we are going to use the official SBT repositories to download and install the latest stable SBT.

First, install the prerequisite **OpenJDK**:
```sh
sudo dnf install java-1.8.0-openjdk-devel
```

Second, using the guide on the [SBT download page](https://www.scala-sbt.org/download.html), install **SBT**:
```sh
curl https://bintray.com/sbt/rpm/rpm > bintray-sbt-rpm.repo
sudo mv bintray-sbt-rpm.repo /etc/yum.repos.d/
sudo dnf install sbt
```
At this point, `cd` to the `lib-rules-int-scala` repository and run `sbt test` to check if everything is working correctly. SBT will download everything it needs to compile and run the project by itself. It will take roughly five minutes for everything to download, compile, install, and return `[success]`. 

### Install RBENV

Many of the following instructions are from the [Fedora Developer Documentation.](https://developer.fedoraproject.org/start/sw/web-app/rails.html)

Install all dependencies for Ruby and RBENV:
```sh
sudo dnf install git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
```

Execute the following commands to install **RBENV**:
```sh
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL
```

After rbenv has been installed, install **Ruby 2.4.2**, followed by **bundler** (via Ruby's `gem` package manager.)
```sh
rbenv install 2.4.2
gem install bundler
```

Having successfully installed Ruby 2.4.2 and bundler, the remainder of the process will be automated with bundler. Running bundler in the `lib-rules-parse-ruby` directory will install all other dependencies automatically.
```sh
bundle install
```

### Test the Rule Compiler

As per the guide [here](https://github.com/xalgorithms/lib-rules-parse-ruby), a smoke test can be run to confirm the correct version of Ruby has been installed, and the compiler can run correctly: 

```sh
bundle exec rspec
```

To run the compiler with a `rulename.rule` file, which produces a `rulename.rule.json` file that the interpreter can run:
```bash
bundle exec ruby cli.rb <path to input *.rule> <path to output *.rule.json > 
```

### Test the Rule Interpreter

As per the guide [here](https://github.com/xalgorithms/lib-rules-int-scala), a smoke test can be run to confirm SBT has been installed correctly. 

First, move to the `lib-rules-int-scala` directory and run:
```bash
sbt test
```

If no failures occur, use the test-run shell script. This will determine if the project can build and run.

```bash
./test-run.sh validate
```

The following output should be printed:

```bash
> compiling test-runs/validate/validate.rule to test-runs/validate/validate.rule.json
[info] Loading project definition from /home/rflec028/Xalgo/lib-rules-int-scala/project
[info] Loading settings from build.sbt ...
[info] Set current project to il-rules-interpreter (in build file:/home/rflec028/Xalgo/lib-rules-int-scala/)
[info] Compiling 2 Scala sources to /home/rflec028/Xalgo/lib-rules-int-scala/target/scala-2.12/classes ...
[info] Done compiling.
[info] Packaging /home/rflec028/Xalgo/lib-rules-int-scala/target/scala-2.12/il-rules-interpreter_2.12-0.0.5.jar ...
[info] Done packaging.
[info] Running org.xalgorithms.rules.Runner test-runs/validate
# discovered 1 test runs in test-runs/validate, executing all...
EXECUTE: VALIDATE
# no expectations exist, dumping tables
table:validate
     0 | a:              1 | b:              2 |
     1 | a:              2 | b:              4 |
     2 | a:              3 | b:              6 |

timing
> load: 348ms (348763495ns)
> populate_context: 0ms (211893ns)
> execute: 10ms (10706503ns)
> execute > step0: 9ms (9139067ns)
> load_expected: 0ms (289712ns)

[success] Total time: 11 s, completed Jul 30, 2018 8:18:23 AM
```

If all tests can be run, the compiler and interpreter have been set up correctly.

## Writing and Running Rules

In the root of `lib-rules-int-scala`, `./test-run.sh` is provided. This uses the latest stable version of the rule compiler, included in the directory, to compile the rule (provided as the first argument,) and run it with the following command:

```bash
./test-run.sh rule-name 
```

To run a rule, only the following command is needed:
```bash
sbt "runMain org.xalgorithms.rules.Runner test-runs/map/"
```

The `lib-rules-int-scala` repository contains a directory of `test-runs`. Rules are roughly organized like so:

```
example
|- example.context.json
|- example.expected.json
|- example.rule
|- tables
   |- area (ex. test)
      |- version (ex. 0.0.1)
         |- table_one.json 
         |- table_two.json 
         |- table_three.json 
```

* `example.context.json` includes data that will, as the system matures, be included when a document is submitted for processing.
* `example.expected.json` includes the correct final output for the rule, to ensure the rule is written and running correctly.
* `example.rule` is the rule file itself. Currently it must be compiled to `example.rule.json` before interpretation (`./test-run.sh` does this automatically.)

Tables are organized under area and rule version so they can be correctly stored in production databases. 

Instructions for writing a rule and rule syntax can be found [here](https://github.com/xalgorithms/general-documentation).

## TroubleShooting

This section will be updated as common installation problems are reported.

If a new problem is found when compiling or running rules, please check the existing issues for the compler ([link](https://github.com/Xalgorithms/lib-rules-int-scala)) and interpreter ([link](https://github.com/Xalgorithms/lib-rules-parse-ruby)). If the new problem is unique, please create a new issue within the relevant repository.

<!--![Image.]({{ site.url }}/assets/opinion.gif){: .center-image}-->

<br />

Thanks for reading,

<img src="{{ site.url }}/assets/art/s.png" alt="RCF" style="border-radius:0; width: 289px;"/>


