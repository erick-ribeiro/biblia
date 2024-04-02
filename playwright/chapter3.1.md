Welcome to chapter 3.

In this chapter, we are going to be introduced to a robust test structure.

With this structure, we'll see methods such as beforeEach and afterAll that will make our tests more powerful and clean.

We'll learn how to apply this new structure to the tests that we've been working on.

We'll be introduced to a new concept called the Page Object Model and how to use it.

And we will learn about new locators and actions. I hope you have fun.

Let's start by opening the example.spec.ts file.

This is the same file that we just saw and I removed the comments and extra lines so it could fit into our screen.

We can see here that it has a lot of lines of code.

Imagine if we have thousands of files - how hard it would be to maintain and to change all those files.

We could even see here that some lines are repetitive in all of them and some are repetitive in some of them.

There are some ways to make our lives easier by simplifying the way we write tests.

We are going to learn how to change this file and turn it into a more clean, organized and maintainable file.

Let's go ahead and open the template.spec.ts file.

This file is a template that I created. It's not functional, but it will give us an idea of what we can have inside a test and later we are going to explore it more.

As usual, we start the file with imports, and I want to introduce you to the AAA pattern.

```
import { test, expect } from '@playwright/test';
```

This is a pattern that suggests us to structure our code in sections.

The first section is the Arrange section. The second is Act, and the third is Assert.

With this suggestion, you will start your file by having all the Arranges at the beginning.

And this can be in a loop - so you can have an Arrange section with Arrange, Act, and Assert within it.

For example, here we see a const password. This would be representing the Arrange area.

```
const password = process.env.PASSWORD;
```

**Ideally, all the variables should come at the very beginning of a file or a method. **

The beforeAll method, which is present in the test object, is the method that will be executed by default before all the tests within a file.

```
test.beforeAll(async ({ playwright }) => {
    test.skip(
      !!process.env.PROD,
      'Test intentionally skipped in production due to data dependency.'
    );
    // start a server
    // create a db connection
    // reuse a sign in state
});
```

You can see here that we have the object playwright that can be used inside this method.

You can have some conditions here - for example, to skip the test if the environment is production.

Other common scenarios that you can have is to start a server, to create a db connection, or even to reuse a sign in state.

For this method, if we do have an Arrange, Act and Assert, we will use it within this method.

Next, we can see the beforeEach.

```
test.beforeEach(async ({ page }, testInfo) => {
    console.log(`Running ${testInfo.title}`);
    // open a URL
    // clean up the DB
    // create a page object
    // dismiss a modal
    // load params
});
```

This is going to be executed by default before each method.

These methods are not mandatory; they are optional and you can decide if it makes sense to have them in your file or not.

Here you can see that we can pass the element page and testInfo.

For testInfo, you have many options that you can use - for example, the title.

Common actions that we have inside this method are to open a URL before each test, clean up DB, create a page object, dismiss a modal, or even load parameters.

This is just an example. It will vary depending on each project.

Similar to the beforeAll and beforeEach, we do have the afterAll and afterEach.

```
test.afterAll(async ({ page }, testInfo) => {
    console.log('Test file completed.');
    // close a DB connection
});

test.afterEach( async ({ page }, testInfo) => {
    console.log(`Finished ${testInfo.title} with status ${testInfo.status}`);

    if (testInfo.status !== testInfo.expectedStatus)
        console.log(`Did not run as expected, ended up at ${page.url()}`);
    // clean up all the data we created for this test through API calls
});
```

They will follow the same standards and you can have - for example, a console.log here - "Test file completed".

Another example is to close a DB connection.

In afterEach, you have access to testInfo, and you can log each test result within this file and you can also have some conditions here.

If something happens, you can use console.log.

We could also clean up data after each test if necessary.

Personally, I consider these methods "Arrange" methods, and that's why I always keep them at the beginning of the file.

Another advantage of having all these four methods at the beginning is that you don't need to scroll up and down within a file to guess if you have those methods and where they are.

You will always know that if they exist, they will be on the top of the file, followed by the describe and the test scenarios within it.

Followed by these methods, we have the test.describe.

```
test.describe.skip('Test Case', () => {
    test('Test Scenario One', async ({ page }) => {
        await test.step('Step One', async () => {
            // ...
        });

        await test.step('Step Two', async () => {
            // ...
        });

        // ...
    });
```


This is similar to a test case name and the test will be similar to a test scenario.

You can see here that we have an option, which is skip. In this case, the test will skip all the tests within this test case.

You could also use only here, and it would only run the tests that are inside this describe.

The describe is optional as well, and you can have multiple describes within one test file.

Personally, I avoid those situations, because I like splitting the file in case we have multiple describes.

But of course, this depends on each project.

There are situations where we do need two describes within one file, so be mindful about the use of it.

Inside the describe, we have the test scenarios.

In this first example, we can see here that we have steps.

These steps could be used - for example - as Arrange, Act, and Assert, if needed.

If not, you could just create different steps for each scenario that you want.

These are also optional.

As we did for describe, we could also use skip and only for each test scenario. You could just do test.only or test.skip.

And again, within each test scenario, we would follow the pattern AAA.

To sum up, you can have a test.describe representing the test case.

You can skip the entire test case or you can run only the entire test case, or you can just have nothing here.

Inside of describe, you have test scenarios.

Inside a test scenario, you can have steps and commands after them, or you can have just a simple test scenario without steps.

You could use .only and .skip to run one specific test scenario.

All the way back to the top, we do have our variables at the beginning, beforeAll, beforeEach, afterAll, and afterEach.

Let's see how we can implement that using our example.spec.ts file.

This is the end of the first video of chapter 3.

We still have one more video to complete the chapter.

I wish you good luck with the quiz below and a reminder to finish the exercises at the end of the whole chapter.

I'll see you in the next video.


## Resources
- [Introduction to Playwright - GitHub repository - Branch 3](https://github.com/raptatinha/tau-introduction-to-playwright/tree/chapter-3)
- [Chapter 3 - Extra Exercises](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/exercises/chapter3.MD)
- [Chapter 3 - Extra Resources](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/extra-resources/chapter3.MD)
