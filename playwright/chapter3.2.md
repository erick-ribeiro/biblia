For this practice, we are going to create a new file.

Let's come here to tests > "New File…", and we can call it example2.spec.ts.

Let's close this out so we have more room, and I want to split the screen so we can see the example.spec.ts file.

We know that we need the import, so let's go ahead and copy it over.

```
import { test, expect, type Page } from '@playwright/test';
```

In this test, we are going to use the AAA pattern to help us organize the test.

We can see that the playwright URL is being used multiple times in this file, so we can go ahead and create a const URL and paste it over to reuse that in the future.

```
import { test, expect, type Page } from '@playwright/test';

const URL = 'https://playwright.dev/';
```

Also, this command - await page.goto('https://playwright.dev/'); - is the first step of each test.

So, because it is the first step, we can create a method beforeEach and use that for this command.

```
import { test, expect, type Page } from '@playwright/test';

const URL = 'https://playwright.dev/';

test.beforeEach(async ({page}) => {
    await page.goto(URL);
    homePage = new HomePage(page);
});
```

We can paste the command here and replace the URL with the new variable we just created.

We can also copy the tests into this file and let's fix the indentation here real quick.

```
import { test, expect, type Page } from '@playwright/test';

const URL = 'https://playwright.dev/';

test.beforeEach(async ({page}) => {
    await page.goto(URL);
});

test('has title', async ({ page }) => {
  await expect(page).toHaveTitle(/Playwright/);
});

test('get started link', async ({ page }) => {
  await page.getByRole('link', { name: 'Get started' }).click();
  await expect(page).toHaveURL(/.*intro/);
});

test('check Java page', async ({ page }) => {
  await page.getByRole('link', { name: 'Get started' }).click();
  await page.getByRole('button', { name: 'Node.js' }).hover();
  await page.getByText('Java', { exact: true }).click();
  // await page.getByRole('navigation', { name: 'Main' }).getByText('Java').click(); // in case the locator above doesn't work, you can use this line. Remove the line above and use this one instead.
  await expect(page).toHaveURL('https://playwright.dev/java/docs/intro');
  await expect(page.getByText('Installing Playwright', { exact: true })).not.toBeVisible();
  const javaDescription = `Playwright is distributed as a set of Maven modules. The easiest way to use it is to add one dependency to your project's pom.xml as described below. If you're not familiar with Maven please refer to its documentation.`;
  await expect(page.getByText(javaDescription)).toBeVisible();
});
```

We can now totally remove the first line because they are going to be executed by default in the beforeEach.

Another thing we see is this command being duplicated - await page.getByRole('link', { name: 'Get started' }).click();

So, we can create a function to store this command. Let's create an async function for that.

```
import { test, expect, type Page } from '@playwright/test';

const URL = 'https://playwright.dev/';

test.beforeEach(async ({page}) => {
    await page.goto(URL);
});

async function clickGetStarted(page: Page) {
  await page.getByRole('link', { name: 'Get started' }).click();
}

test('has title', async ({ page }) => {
  await expect(page).toHaveTitle(/Playwright/);
});

test('get started link', async ({ page }) => {
  await clickGetStarted(page);
  await expect(page).toHaveURL(/.*intro/);
});

test('check Java page', async ({ page }) => {
  await clickGetStarted(page);
  await page.getByRole('button', { name: 'Node.js' }).hover();
  await page.getByText('Java', { exact: true }).click();
  await expect(page).toHaveURL('https://playwright.dev/java/docs/intro');
  await expect(page.getByText('Installing Playwright', { exact: true })).not.toBeVisible();
  const javaDescription = `Playwright is distributed as a set of Maven modules. The easiest way to use it is to add one dependency to your project's pom.xml as described below. If you're not familiar with Maven please refer to its documentation.`;
  await expect(page.getByText(javaDescription)).toBeVisible();
});
```

We can call it clickGetStarted and what we need here is going to be the page and the type is Page.

With this, we can replace this line in each test.

We also learned that it's interesting to have the tests under a describe, so we'll go ahead and do a test.describe here and the name is going to be 'Playwright website', and this is going to be an arrow function too.

Let's move the tests to inside the describe.

```
import { test, expect, type Page } from '@playwright/test';

const URL = 'https://playwright.dev/';

test.beforeEach(async ({page}) => {
    await page.goto(URL);
});

async function clickGetStarted(page: Page) {
  await page.getByRole('link', { name: 'Get started' }).click();
}

test.describe('Playwright website', () => {
    test('has title', async ({ page }) => {
      await expect(page).toHaveTitle(/Playwright/);
    });

    test('get started link', async ({ page }) => {
      await clickGetStarted(page);
      await expect(page).toHaveURL(/.*intro/);
    });

    test('check Java page', async ({ page }) => {
      await clickGetStarted(page);
      await page.getByRole('button', { name: 'Node.js' }).hover();
      await page.getByText('Java', { exact: true }).click();
      await expect(page).toHaveURL('https://playwright.dev/java/docs/intro');
      await expect(page.getByText('Installing Playwright', { exact: true })).not.toBeVisible();
      const javaDescription = `Playwright is distributed as a set of Maven modules. The easiest way to use it is to add one dependency to your project's pom.xml as described below. If you're not familiar with Maven please refer to its documentation.`;
      await expect(page.getByText(javaDescription)).toBeVisible();
    });
});
```

## Page Object Model
We can see that this addition doesn't change the code a lot, so I want to introduce you to a new concept called the Page Object Model, or POM.

POM is a representation of a page or a group of elements in a page.

If we open the playwright.dev homepage, we can have the top header mapping as a page object model, for example.

We can have all the content mapped as another page object model, and the footer as another one.

This way we can have all the actions and assertions mapped inside the Page Object Model and be able to reuse it whenever we need it.

Let's create the page object model for the homepage in this project so we can update this file.

Let's save this. Go to the Explorer, create a new folder called pages, and under that a new file called home-page.ts.

We can start by importing the type Locator and the type Page - we will need that.

```
import { type Locator, type Page } from '@playwright/test';
```

And after that, we can create a class HomePage, and we will make it visible.

```
import { type Locator, type Page } from '@playwright/test';

export class HomePage {
}

export default HomePage;
```

Right inside of the class, we'll have the variables, the constructor, and the methods or functions.

```
import { type Locator, type Page } from '@playwright/test';

export class HomePage {
    //variables
    readonly page: Page;

    //constructor
    constructor(page: Page) {
        this.page = page;
    }

    //methods
}

export default HomePage;
```

Variables are usually readonly and we can start by having page:Page.

The constructor will be constructor (page: Page) and we'll set this.page = page.

We can save this file and check in our example test file to see which locator we want to implement first.

On the homepage we do have the "Get started" link, so we can copy this command and paste it

here so we can remember, and let's create a variable for it.

It's going to be readonly and we'll call it getStartedButton and it's going to be a Locator.

In our constructor, we'll allocate this element to our new variable.

```
import { type Locator, type Page } from '@playwright/test';

export class HomePage {
    //variables
    readonly page: Page;
    readonly getStartedButton: Locator;

    //constructor
    constructor(page: Page) {
        this.page = page;
        this.getStartedButton = page.getByRole('link', { name: 'Get started' });
    }

    //methods
}

export default HomePage;
```

Now, we have access to the getStartedButton and we want to create a method for that.

```
import { type Locator, type Page } from '@playwright/test';

export class HomePage {
    //variables
    readonly page: Page;
    readonly getStartedButton: Locator;

    //constructor
    constructor(page: Page) {
        this.page = page;
        this.getStartedButton = page.getByRole('link', { name: 'Get started' });
    }

    //methods
    async clickGetStarted() {
        await this.getStartedButton.click();
    }
}

export default HomePage;
```

Now, we have a method to click "Get started" mapped in our Page Object Model.

## Using the Page Object Model
So, we can go back to our test file and we can do four steps here to be able to use that.

We'll add import { HomePage } from '../pages/home-page';.

We'll create a new variable for that - let homePage: HomePage;.

Inside our beforeEach, we are going to create a new instance of that variable where homePage = new HomePage(page) so the constructors is executed.

And in our clickGetStarted function, here we can replace the getByRole command with homePage.clickGetStarted().

```
import { test, expect, type Page } from '@playwright/test';
import { HomePage } from '../pages/home-page';

const URL = 'https://playwright.dev/';
let homePage: HomePage;

test.beforeEach(async ({page}) => {
    await page.goto(URL);
    homePage = new HomePage(page);
});

async function clickGetStarted(page: Page) {
    await homePage.clickGetStarted();
}
```

This way, instead of using this command, we will be using the Page Object Model we just created.

Let's save these, so we have access to them.

One more thing for us to do is to map the assertion on the homepage.

So let's copy this - await expect(page).toHaveTitle(/Playwright/);.

Go to the page object and we'll do the variables first.

We'll create readonly title and it's going to be a RegExp.

In our constructor, we will do this.title = /Playwright/.

And we can create a method async assertPageTitle(), and we can check that we do have the expect here.

We do need to import the expect as well, so we'll add it here. Now we are good.

```
import { type Locator, type Page, expect } from '@playwright/test';

export class HomePage {
    readonly page: Page;
    readonly getStartedButton: Locator;
    readonly pageTitle: RegExp;

    constructor(page: Page) {
        this.page = page;
        this.getStartedButton = page.getByRole('link', { name: 'Get started' });
        this.pageTitle = /Playwright/;
    }

    async clickGetStarted() {
        await this.getStartedButton.click();
    }

    async assertPageTitle() {
        await expect(this.page).toHaveTitle(this.pageTitle);
    }
}

export default HomePage;
```

Going back to the test file, instead of having await expect(page).toHaveTitle(/Playwright/);, we'll do homePage.assertPageTitle().

```
test.describe('Playwright website', () => {
    test('has title', async () => {
        await homePage.assertPageTitle();
    });
```

We can see that we don't need the page here anymore, and we don't need it here in clickGetStarted either, but I'll leave it here for now.

With that, we finished the homepage assertion and mapping on the Page Object Model.

I have cleaned up the Page Object Model file that we just created together, and this is how it looks at the end.

I have also created a new Page Object Model file to map all the elements of the top menu on the Playwright website, and this is how it looks.

You can see that we have way more elements here and a few more functions or methods, and I've updated the example2.spec.ts with the import, the variables, and all the commands that we need.

Note that on the third test 'check Java page', I'm using steps to represent the act and the assert inside the AAA method.

```
    test('check Java page', async ({ page }) => {
        await test.step('Act', async () => {
            await clickGetStarted(page);
            await topMenuPage.hoverNode();
            await topMenuPage.clickJava();
        });


        await test.step('Assert', async () => {
            await topMenuPage.assertPageUrl(pageUrl);
            await topMenuPage.assertNodeDescriptionNotVisible();
            await topMenuPage.assertJavaDescriptionVisible();
        });
    });
```

Again, this is not mandatory.

We could easily remove them and just have the list of commands here.

I invite you to take a look at those changes and explore the new learnings.

It's also important to mention that Playwright has a whole list of locators to help you find the elements and also actions to help you interact with the elements.

So for example, as we saw, a click of a button, typing into a text field, or maybe checking a checkbox.

Because Playwright is in constant development, make sure to check the documentation every so often.

I knew we were going to make it. Congratulations for finishing Chapter 3. I hope you had fun.

Good luck with the quiz below, and a friendly reminder to take a look at the extra exercises.

I'll see you in the next chapter. Happy testing.


## Resources
-[Introduction to Playwright - GitHub repository - Branch 3]()
-[Chapter 3 - Extra Exercises]()
-[Chapter 3 - Extra Resources]()