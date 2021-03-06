---
menu: reference
renderer: Cucumber::Website::Reference
title: Browser Automation
polyglot: true
---

Cucumber is not a Browser Automation tool, but it works well with the following Browser Automation tools.

# Selenium WebDriver

WebDriver is designed to provide a simpler, more concise programming interface in addition to addressing some limitations in the Selenium-RC API. Selenium-WebDriver was developed to better support dynamic web pages where elements of a page may change without the page itself being reloaded. WebDriver's goal is to supply a well-designed object-oriented API that provides improved support for modern advanced web-app testing problems.

Let us look at an example of Cucumber using Selenium-WebDriver in UI testing, by converting [Selenium-Web driver by example](http://docs.seleniumhq.org/docs/03_webdriver.jsp#introducing-the-selenium-webdriver-api-by-example).

 We can express the example as the following Scenario:

```gherkin
Scenario: Finding some cheese
   Given I am on the Google search page
   When I search for "Cheese!"
   Then the page title should start with "cheese"
```

```java
package class.exmple;

public class ExampleSteps {

    private final WebDriver driver = new FirefoxDriver();
    @Given("^I am on the Google search page$"\)
    public void I_visit_google() {
    driver.get("https:\\www.google.com");
   }

   @When("^I search for \"(.*)\"$")
     public void search_for(String query) {
        WebElement element = driver.findElelment(By.name("q"));
        //Enter Something to search for
        element.sendKeys(query);
        //Now submit the form. WebDriver will find the form for us from the element
        element.submit();
   }

   @Then("^ the page title should start with \"(.*)\"$")
   public void checkTitle() {
       //Google's search is rendered dynamically with JavaScript.
       //Wait for the page to load timeout after ten seconds
       new WebDriverWait(driver,'10')).untill(new ExpectedCondition<Boolean> {
       public Boolean apply(WebDriver d) {
       return d.getTitle().toLowerCase.startsWith("cheese");
       //Should see: "cheese! -Google Search"
    }

    @After()
     public void closeBrowser() {
       driver.quit();
     }
}
```


```ruby
require 'rubygems'
require 'selenium-webdriver'

Given("^I am on the Google search page$") do
  driver = Selenium::WebDriver.for :firefox
  driver.get "http:\\google.com"
end

When("^I search for "([^"]*)"$") do
  element = driver.find_element(:name => "q")
  element.send_keys "Cheese!"
  element.submit
end

Then("^the page title should start with "([^"]*)"$") do
  wait = Selenium::WebDriver::Wait.new(:timeout => 10)
  wait.until { driver.title.downcase.start_with? "cheese!" }
  puts "Page title is #{driver.title}"
    browser.close
end
```

More information on [Selenium Webdriver](http://docs.seleniumhq.org/projects/webdriver/).

# Watir Webdriver

Watir, pronounced _water_, is an open-source (BSD) family of Ruby libraries for automating web browsers. It allows you to write tests that are easy to read and maintain. It is simple and flexible.

Watir drives browsers the same way people do. It clicks links, fills in forms, presses buttons. Watir also checks results, such as whether expected text appears on the page.

Watir is a family of Ruby libraries but it supports your application no matter which technology it is developed in. While Watir supports only Internet Explorer on Windows; Watir-WebDriver solves single browser testing and support Chrome, Firefox, Internet Explorer, Opera and also running in headless mode (HTMLUnit).

Now let's jump in to a sample UI testing program using Watir:

```ruby
require "rubygems"
require "rspec"
require "watir-webdriver"

describe "google.com" do
  let(:browser) { @browser ||= Watir::Browser.new :firefox }
  before { browser.goto "http://google.com" }
  browser.text_field(:name => "q").set "watir"
  browser.button.click
  browser.div(:id => "resultStats").wait_until_present
  browser.title.should == "watir - Google Search"
  after { browser.close }
end
```

Now let us incorporate Cucumber to this simple test

```gherkin
Feature: Search In order to use Google users must be able to search for content
  Scenario: Search for a term
    Given I have entered "watir" into the query
    When I click "search"
    Then I should see some results
```

```ruby
require "watir-webdriver"
require "rspec/expectations"

Given /^I have entered "([^"]*)" into the query$/ do |term|
  @browser ||= Watir::Browser.new :firefox
  @browser.goto "google.com"
  @browser.text_field(:name => "q").set term
end

When /^I click "([^"]*)"$/ do |button_name|
 @browser.button.click
end

Then /^I should see some results$/ do
  @browser.div(:id => "resultStats").wait_until_present
  @browser.div(:id => "resultStats").should exist
  @browser.close
end
```

More information on [Watir](http://watir.com).

# Serenity BDD

Serenity BDD is an open source reporting library that helps you write better
structured, more maintainable automated acceptance criteria. Serenity also produces
rich meaningful test reports (or "living documentation") that report not only the
test results, but also which Features have been tested.

{{% text "java" %}}
A detailed tutorial on using Cucumber-JVM with Serenity can be found
[here](http://thucydides.info/docs/articles/an-introduction-to-serenity-bdd-with-cucumber.html).
{{% /text %}}

The above scenario might be written for Serenity like this:

```ruby
# Serenity only works with Java for now.
```

```java
package com.example.features.steps;

import cucumber.api.java.en.Given;
import cucumber.api.java.en.Then;
import cucumber.api.java.en.When;
import net.serenitybdd.demos.todos.pages.GoogleHomePage;
import net.serenitybdd.demos.todos.pages.SearchResultsPage;

import static org.assertj.core.api.Assertions.assertThat;

public class SearchSteps {

    GoogleHomePage googleHomePage;
    SearchResultsPage searchResultsPage;

    @Given("^I am on the Google search page$")
    public void i_am_on_the_Google_search_page() throws Throwable {
        googleHomePage.open();
    }

    @When("^I search for \"([^\"]*)\"$")
    public void i_search_for(String searchTerm) throws Throwable {
        googleHomePage.searchBy(searchTerm);
    }

    @Then("^the page title should start with \"([^\"]*)\"$")
    public void the_page_title_should_start_with(String expectedTitleStart) throws Throwable {
        assertThat(searchResultsPage.getTitle().toLowerCase())
                 .startsWith(expectedTitleStart.toLowerCase());
    }
}
```

In this example, the `WebDriver` interaction is delegated to `PageObject` subclasses.
Serenity has built-in support for `PageObject`s, which might look like this:

```ruby
# Serenity only works with Java for now.
```

```java
@DefaultUrl("http://www.google.com")
public class GoogleHomePage extends PageObject {

    public static final String FOOTER_TO_APPEAR = "#foot";

    @FindBy(name = "q")
    private WebElementFacade searchField;

    public void searchBy(String searchTerm) {
        // Enter something to search for and submit the form
        searchField.type(searchTerm)
                   .then()
                   .sendKeys(Keys.RETURN);

        // Google's search page is rendered dynamically with JavaScript.
        // Wait for the footer to appear to know that the search is complete.
        waitFor(FOOTER_TO_APPEAR);
    }
}

public class SearchResultsPage extends PageObject {}
```

More information on [Serenity](http://serenity-bdd.info).

# Capybara

<!-- TODO -->

More information on [Capybara](http://teamcapybara.github.io/capybara/).

# Tips and Tricks

## Multiple Browsers

Cucumber can run your Scenarios with different browsers, based on a configuration property loaded at runtime:


```ruby
Capybara.register_driver :selenium do |app|
  browser = (ENV['browser'] || 'firefox').to_sym
  Capybara::Selenium::Driver.new(app, :browser => browser)
end
```

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class WebDriverFactory {
    public static WebDriver createWebDriver() {
        String webdriver = System.getProperty("browser", "firefox");
        switch(webdriver) {
            case "firefox":
                return new FirefoxDriver();
            case "chrome":
                return new ChromeDriver();
            default:
                throw new RuntimeException("Unsupported webdriver: " + webdriver);
        }
    }
}
```

Then, simply define the `browser` property when you run Cucumber:

```
browser=chrome cucumber
```

```
mvn test -Dbrowser=chrome
```

If you are using Serenity, simply pass the `driver` system property (no extra coding required):

```
mvn test -Ddriver=chrome
```

## Re-using the browser window

Closing and re-opening the browser window between Scenarios will slow them down.

To reuse them, you can use the [`SharedDriver`](https://github.com/cucumber/cucumber-jvm/blob/master/examples/java-webbit-websockets-selenium/src/test/java/cucumber/examples/java/websockets/SharedDriver.java)
wrapper rather than calling `WebDriver` directly.

## Example Projects

- [java-webbit-websockets-selenium](https://github.com/cucumber/cucumber-jvm/tree/master/examples/java-webbit-websockets-selenium)
- [serenity-with-cucumber](https://github.com/serenity-bdd/serenity-articles/tree/master/introduction-to-serenity-with-cucumber/src/samples/etsy-tester)
