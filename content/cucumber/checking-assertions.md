---
menu:
- reference
source: https://github.com/cucumber/cucumber/wiki/RSpec-Expectations/
title: Checking Assertions
polyglot: true
---

Part of your tests will be to make assertions about your application. To do so, you can make use of a testing framework.

{{% block "ruby" %}}
# RSpec
If you're using bundler, add the `rspec-expectations` gem to your `Gemfile`.
Cucumber will automatically load RSpec's matchers and expectation methods to be
available in your Step Definitions. For example:

```ruby
Given /^a nice new bike$/ do
  expect(bike).to be_shiny
end
```

If you want to configure RSpec, you'll need to also add the `rspec-core` gem
to your `Gemfile`. Then, you can add to your `features/support/env.rb`
configuration, such as:

```ruby
RSpec.configure do |config|
  config.expect_with :rspec do |c|
    c.syntax = :expect
  end
end
```

# Test Unit

If you don't like RSpec's `should` methods for assertions, you can use the familiar `Test::Unit` `assert` methods by mixing it into
your [`World`](/wiki/a-whole-new-world).

```ruby
require 'test/unit/assertions'

World(Test::Unit::Assertions)
```

<!-- TODO: You can see a full example under the [examples](https://github.com/cucumber/cucumber/tree/master/examples%2Ftest_unit) -->
{{% /block %}}

{{% block "java" %}}
# JUnit

[JUnit](http://junit.org/junit4/) is a unit testing framework designed for the Java programming language. It is an instance of the xUnit architecture for unit testing frameworks.

If you are using Maven, just add the following to your `pom.xml`:

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-junit</artifactId>
    <version>{{ site.versions.cucumber_jvm }}</version>
</dependency>
```

{{% note "Cucumber version"%}}
Make sure to use the same version for `cucumber-junit` that you are using for `cucumber-java` or cucumber-java8`.
{{% /note %}}

{{% note "JUnit 5"%}}
At the moment, JUnit 5 is not yet supported by Cucumber.
{{% /note %}}

Using assertions in JUnit is very easy. For example:

```java
import static org.junit.Assert.assertEquals;

public class Example {

        @Then("^the result should be (.+)$")
        public void the_result_should_be(String expectedResult) {
            assertEquals(expectedResult, result);
        }
}
```

For more examples of how to use JUnit assertions, see the [JUnit wiki on github](https://github.com/junit-team/junit4/wiki/Assertions)
For a more extensive example of how to use JUnit with Cucumber, see the [java-calculator example](https://github.com/cucumber/cucumber-jvm/tree/master/examples/java-calculator).

# TestNG

[TestNG](http://testng.org/doc/) is a testing framework designed for the Java programming language and inspired by JUnit and NUnit.

If you are using Maven, just add the following to your `pom.xml`:
```xml
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-testng</artifactId>
    <version>6.10</version>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-testng</artifactId>
    <version>{{ site.versions.cucumber_jvm }}</version>
</dependency>
```

Using assertions in TestNG is similar to using them in JUnit.
For a more extensive example of how to use TestNG with Cucumber, see the [java-calculator-testng example](https://github.com/cucumber/cucumber-jvm/tree/master/examples/java-calculator-testng).
{{% /block %}}

{{% block "javascript" %}}
If you are using cucumber-js, there are many test frameworks to choose from.
Which one you use, may depend on other javascript frameworks your project is using and / or personal preference.

Here's an example using [Node.js and Chai](https://github.com/cucumber/cucumber-js/blob/master/docs/nodejs_example.md).
{{% /block %}}
