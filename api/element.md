## element() global API

The newly added `element()` global object adds to Nightwatch 2 the functionality present in the [Selenium WebElement](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html) class.

It supports all the usual ways of locating elements in Nightwatch, as well as the Selenium locators built using `By()`, which is also available in Nightwatch as a global named `by()`.

### Syntax:

**Using regular CSS (or Xpath) selectors:**

<div class="sample-test" style="max-width:600px"><pre data-language="javascript" style="padding-top: 10px" class="default-theme language-javascript"><code class="default-theme language-javascript">const addButtonEl = element('button[type="submit"]');
</code></pre></div>

**Using Nightwatch selector objects:**

<div class="sample-test" style="max-width:600px"><pre data-language="javascript" style="padding-top: 10px" class="default-theme language-javascript"><code class="default-theme language-javascript">const addButtonEl = element({
  selector: 'button[type="button"]',
  index: 0
});
</code></pre></div>

**Using Selenium locators:**

<div class="sample-test" style="max-width:600px"><pre data-language="javascript" style="padding-top: 10px" class="default-theme language-javascript"><code class="default-theme language-javascript">const locator = by.css('button[type="button"]');
const addButtonEl = element(locator);
</code></pre></div>

**Using Selenium WebElements:**

<div class="sample-test" style="max-width:600px"><pre data-language="javascript" style="padding-top: 10px" class="default-theme language-javascript"><code class="default-theme language-javascript">// webElement is an instance of WebElement class from Selenium
const addButtonEl = element(webElement);
</code></pre></div>

### API Commands
All the existing methods from a regular [WebElement](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html) instance are available. If a method is called, it will be added to the Nightwatch queue accordingly.

**Available element commands:**
- [.clear()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#clear)
- [.click()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#click)
- [.findElement()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#findElement)
- [.findElements()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#findElements)
- [.getAttribute()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getAttribute)
- [.getCssValue()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getCssValue)
- [.getDriver()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getDriver)
- [.getId()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getId)
- [.getRect()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getRect)
- [.getTagName()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getTagName)
- [.getText()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#getText)
- [.isDisplayed()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#isDisplayed)
- [.isEnabled()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#isEnabled)
- [.isSelected()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#isSelected)
- [.sendKeys()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#sendKeys)
- [.submit()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#submit)
- [.takeScreenshot()](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_WebElement.html#takeScreenshot)

### Working Example

The example below navigates to the AngularJS homepage and adds a new to-do item in the sample ToDo App available there.

<div class="sample-test" style="max-width:800px"><pre data-language="javascript" style="padding-top: 10px" class="default-theme language-javascript"><code class="default-theme language-javascript">describe('angularjs homepage todo list', function() {

  // using the new element() global utility in Nightwatch 2 to init elements
  // before tests and use them later
  const todoElement = element('[ng-model="todoList.todoText"]');
  const addButtonEl = element('[value="add"]');

  it('should add a todo using global element()', function() {
    // adding a new task to the list
    browser
      .navigateTo('https://angularjs.org')
      .sendKeys(todoElement, 'what is nightwatch?')
      .click(addButtonEl);

    // verifying if there are 3 tasks in the list
    expect.elements('[ng-repeat="todo in todoList.todos"]').count.to.equal(3);

    // verifying if the third task if the one we have just added
    const lastElementTask = element({
      selector: '[ng-repeat="todo in todoList.todos"]',
      index: 2
    });

    expect(lastElementTask).text.to.equal('what is nightwatch?');

    // find our task in the list and mark it as done
    lastElementTask.findElement('input', function(inputResult) {
      if (inputResult.error) {
        throw inputResult.error;
      }

      const inputElement = element(inputResult.value);
      browser.click(inputElement);
    });

    // verify if there are 2 tasks which are marked as done in the list
    expect.elements('*[module=todoApp] li .done-true').count.to.equal(2);
  });
});
</code></pre></div>