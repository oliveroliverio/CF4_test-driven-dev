

## Testing in the Development Process


# 4.1: Test-Driven Development & Test Scenarios


- Learning Goals
- Introduction
- The Importance of Testing
- Core Testing Concepts
- Test-Driven and Behavior-Driven Development
- Translating Specs into User Stories
- Writing Test Scenarios
- npm create vite
- Summary
- Resources
- Task
- Forum


#### Learning Goals


- Write user stories based on the app’s key features.
- Translate user stories for each feature into multiple test scenarios.
- Use npm create vite to create a React app and push it to GitHub.
- Deploy a React app to Vercel.


#### Introduction

Welcome to the fourth Achievement of your Full-Stack Immersion Course! In the previous Achievement, you used a JavaScript framework known as React to complete the client-side for your myFlix application, rounding out (and giving a face to) the server-side you constructed using Node.js in Achievement 2. Together, these two “ends” (frontend and backend) formed a fully functioning app that allows users to search for movies, read information about movies (synopses, director biographies, genres, etc.), create a list of favorite movies, and update their personal details. Perhaps most importantly, you did all of this while familiarizing yourself with the concepts behind MVC architecture—an industry standard when it comes to structuring the client-side for web applications.

Pat yourself on the back, because you’ve come a long way since you first started your Intro to Frontend Development Course. You’ve gone from developing simple websites using HTML and CSS to coding entire single-page applications from scratch. In fact, it may feel like you’ve learned all there is to learn about building applications—but like so many things in development, there’s always more. Ready to take the next step?

In this Achievement, you’ll take things up another notch—not only in coding complexity but in the quality of your code. You’ll explore more advanced development concepts such as serverless functions, progressive web apps (PWA), and data visualization, while also learning how to properly test your apps (an essential step for ensuring error-free, or, at the very least, less error-prone code). In doing so, you’ll be introduced to a new development technique known as TDD, or “test-driven development,” which essentially means operating under a “test first, code later” mentality.

Excited? We certainly hope you are! After this Achievement, you’ll really be able to start strutting your stuff, and the project you’ll be building to show off your new knowledge will be a serverless progressive web application built using React and following the TDD approach. The app itself will be a “meetup” app of sorts, displaying a list of upcoming events for a city and time of the user’s choosing. It will also be available for users to use while offline (this is where the “PWA” part of your project will come into play!). In the end, you’ll be proud to share your app (and its skillful use of cutting-edge technologies) with your future employer!

To get there, though, you’ll need to take things one step at a time—starting with acquiring a healthy dose of new knowledge about test-driven development!  What exactly is TDD? Why is it so important to you as a developer? And how can you use it to ensure high-quality, bug-free codebases for your future projects?

You’re about to find out!


> Meet App: Project Brief
> You can download your Meet App project brief here. You'll be needing it throughout this entire Achievement, so make sure it’s stored somewhere convenient!
> 
> 
> Meet App: Project Brief


#### The Importance of Testing

As a consumer, you expect the tools and systems you use to work properly—in other words, they do what they’re supposed to do, they’re not missing (potentially essential) parts, and they perform their function(s) without error. It’s this expectation that forces companies to put their products through rigorous testing before launching them to the market. After all, you’d probably demand your money back if a product didn’t work as advertised, whether that product is small and simple (like a toothbrush) or large and complicated (like a car).

Washing machines, for example, have an expected average lifespan of ten, even twenty years. During this period of time, you’d expect one to effectively and efficiently wash your clothes without breaking down and potentially flooding your home. The reason you can expect this is because each individual washing machine has been tested carefully over and over again (using different wash programs, under different conditions, etc.) before hitting the market. The manufacturers wouldn’t want their product wreaking watery havoc on their consumers’ lives, after all, nor forcing them to spend copious amounts of money on repairs. This would damage the brand and lead to a decrease in washing machine sales.

While digital software won’t (usually) lead to the kind of home devaluation a faulty appliance might breed, this doesn’t mean it’s not essential to test—and test well! You’ll be exploring the various approaches to software testing in an upcoming section, but before jumping ahead, let’s take a moment to understand what exactly software testing entails and why it’s so important to conduct on a regular basis.

As you might surmise from its name, software testing involves checking a piece of software to ensure it doesn’t contain any errors (i.e., “bugs”). The ability to test your own code (and test it effectively) is an important skill that every developer should have, but one few developers enjoy taking the time to learn. After all, testing takes time away from actual coding! And why should you need to test your own code? It obviously works. You wrote it, after all. It’s perfect! If you just retched slightly from those previous sentences, know that you’re not alone. A lot of developers can get pretty high-and-mighty when it comes to their code, which is why it’s important to learn early on that no piece of code is ever perfect, especially once it’s in the hands of your users, who certainly won’t be interacting with it under perfect circumstances. This is why it’s absolutely essential that you test your code. Once you start creating test cases, you’ll dream up all sorts of edge cases (those “not-so-perfect circumstances” in which users may be interacting with your code) that could break your code. Testing your code doesn’t mean that you don’t trust yourself—it simply ensures a stable, bug-free, and enjoyable experience for your users.


> Tip!
> The term “edge case” is used above. Edge cases are situations that, while unlikely to happen, could still happen (they’re on the “edge” of what most users will experience). They usually occur as you approach the maximum or minimum value of any of your parameters—for example, say you have a label on a MovieCard that displays a movie’s name. While the label would probably fit most movie names, an unusually long name might break the UI. This is the kind of thing you likely wouldn’t uncover on your own. If you didn’t write tests for various edge cases to test your app beforehand, you might not even discover the error until it pops up in your live app!

For instance, suppose you take a break from working on your myFlix app for several months (hey, it happens to the best of us!). When you return, you jump right back into making updates and changes to your code, feeling altogether quite good about yourself—until, you try to run your app, and it crashes! How could this be? Your only choice here would be to go back through your code, checking each line and each function one-by-one in search of an inconsistency or flaw. Then, say you do find something that fixes it—only, fixing it breaks something else! This type of scenario is more common than you might think.

Imagine, for example, that your backend for myFlix returns the following JSON object for a movie:


```js
{
 Title: "The Silence of the Lambs",
 Description: "The Silence of the Lambs is a 1991 American psychological horror-thriller film directed by Jonathan ..."
}
```

However, as you haven’t been working with your myFlix database for some time, when you go to add a new entry, you write the following:


```js
{
 Title: "Super Mario Bros.",
 Summary: "Super Mario Bros. is a 1993 American fantasy adventure film based loosely on the Mario video game series by Nintendo ..."
}
```

When your React application attempts to load this data (in order to display it on a `MovieCard`), it will crash, as it expects a “Description” field (rather than a “Summary” field). You don't realize where the issue lies, so you attempt to fix your app by simply removing the “Description” field entirely from your React app. Great! The app doesn’t crash anymore. But now, your movie won’t have a description, which will break the UI for your `MovieCard`. You can already see where attempting to fix bugs can lead to new bugs, especially when there are even more factors (and people!) in play. After all, you’ll likely be working with a team of other developers in the future, who’ll all be writing their own code and fixing their own bugs for the project you’re working on. And that project? It may not have originally been written by those developers! So much of development work is working with codebases that weren’t written by you, meaning that the only viable and effective way of ensuring the final app will behave as expected is to run tests—and lots of them!

Tests are like a safety net for your app. So long as you have tests in place, you can refactor your code without worry (i.e., rewrite/optimize your code to improve its readability, remove unnecessary functions, etc.). These existing tests will immediately notify you of any broken functionalities caused by your poking and prodding.

For example, if you’d had tests in place to check for the presence of all necessary fields in your myFlix UI (“Title,” “Description,” “Cast,” and “Director”), you’d have been notified immediately upon removing the “Description” field from your `MovieCard`. You’d then know that simply removing the “Description” field wouldn’t work as a fix for your previous error (your mistaken use of “Summary” in place of “Description”). While this example might feel a bit silly for your current project (because you’d never do something as unforgivable as forget the proper name for one of your fields), imagine if the original codebase for myFlix were written by someone else and you were simply tasked with updating it. In a case like this, there would likely be many things you wouldn’t know how to write properly (and, subsequently, many naming mistakes!), making it all the more important that you have tests in place to ensure you don’t break your code.

Another way testing can make your life as a developer easier is through automation. Say, for example, you wanted to have an alert in your calendar app for upcoming events. As the developer, you’d, of course, want to test this functionality—but would that mean you’d have to wait until you have an event in your calendar? Thankfully, no! You can simply set up an automated test.

Automated testing can also serve as a way to generate live documentation for your code. By using a technique known as BDD (“behavior-driven development,” which you’ll be exploring in more depth later on in this Exercise), developers come up with different testing scenarios and use cases for the functionality of their apps, which allows them to more easily create documentation that explains the various functionalities behind their code. This documentation can then be understood by and shared with all team members (even non-devs!) from business people, to designers, to stakeholders.

You can probably see by now (if it hasn’t been emphasized enough) just how important testing is when it comes to web development. To quickly recap, remember these four main benefits testing can bring to a project:


- It helps you find bugs while developing an application.
- It helps you think of different scenarios and use cases for your application.
- It helps you create documentation for your code.
- It helps you deliver higher-quality code.


##### Backend vs. Frontend Testing

Many developers think that testing is only beneficial when it comes to backend code. There’s also the notion that frontend code is too hard to test because, for instance, frontend code is run on the user’s browser, which is “outside the developer’s control,” and users are “bound to interact with it in unpredictable ways anyhow.” These two statements simply aren't true. So long as you know the business logic and expected behaviors of a feature, you can test any code without difficulty. In fact, you’ll be testing your frontend code yourself when you get to Exercise 4.4: Unit Testing!


#### Core Testing Concepts

Much like cars, software testing can be classified into two main categories: manual and automated. Manual testing, or “traditional testing,” is most commonly applied to more traditional software development processes, for example, the waterfall process (which you first learned about in Exercise 1.1 of your Intro to Frontend Development Course). Here, QA/QC engineers sit down with a piece of software and systematically check that it meets all the quality requirements one-by-one, completely by hand. QA stands for “Quality Assurance,” while “QC” stands for “Quality Control,” and they both refer to the process of testing a piece of software before it goes to production. Typically, in this kind of testing, testers will have a checklist of tests (also called a “test suite”) to work from, each of which needs to be conducted manually (clicking buttons, filling in data, submitting forms, etc). As you might imagine, this process can be incredibly time consuming, especially as it requires each test to be conducted again every time a change is made in the product.

Nowadays, many companies have turned to a development process known as Agile for planning their workflows (you should remember this term from back in your Intro to Frontend Development Course!). As part of the Agile methodology, teams plan the development of new features and bug fixes in what are called sprints. Each sprint, or “sprint cycle,” typically lasts only two weeks, and in that time the team accomplishes any variety of tasks, from planning, to developing, to testing. In this kind of environment, where at least some testing is usually conducted each sprint, manual testing could become quite costly, and there likely wouldn’t be enough time to finish it within the time allotted, making it less attractive as a testing option.

This is where a new testing method called “automated testing” comes in. Different from manual testing, automated testing is quite quick, and typically involves the developers rather than dedicated QA/QC engineers. These types of tests run during the actual development process, making it more convenient to fix errors that pop up before they can create even larger errors down the line. While the effort required to write these tests in the first place isn’t negligible, they save your team considerable time as you only need to write each test once. Once they’re in place, you can run them again and again without the need for human interference. It also reduces the time spent on manual testing, codebase maintenance, and bug fixing, overall balancing out the upfront effort involved in writing the tests.

Most of this Achievement will be focused on automated testing as it’s the testing method you’ll almost certainly use in your future career as a web developer. That said, manual testing isn’t something that can be neglected entirely. While the bulk of your testing can be covered using automated testing, there will always be certain things that require a human touch—for instance, dragging and dropping an image, swiping something on a touchscreen device, or accurately categorizing content.


##### Types of Testing

There are a number of approaches you can take when it comes to automated testing. The four most common methods include unit testing, integration testing, acceptance testing, and end-to-end testing. Each of these will be outlined below:

Unit Testing

You might be able to figure out the kind of approach unit testing takes based on its name—it breaks down a website or app into “units,” each of which is tested independently of the others. Here, a unit refers to a single function that needs to be tested individually to ensure its functionality before combining it with other functions to form a larger whole.

You can think of unit testing as you would the testing of a bicycle. Just like a piece of software, a bicycle is made up of many parts—or “units.” These units can range from moving parts like wheels and gears to more structural sections like handlebars, pedals, and the saddle. In order for the bike to function correctly, each one of its parts, or units, must also function correctly, so they need to be tested one at a time, independently of one another, to assess their integrity and durability. If no problems are found, they can then be combined in various ways to test their ability to perform well together. This next step is called “integration testing,” which you’ll be looking at in more detail in the next section.

In web development, unit testing can be used to test the output of a function, the response to an API, the rendering of a UI element, and so on. Functions, here, rank most common for this type of testing, as they’re quite easy to test on their own. When testing a function, you call it together with a specified set of parameters to ensure it returns the expected result. Let’s try a simple function test now. The function below returns the sum of two numbers—a and b:


```js
function sum(a, b) {
   return a + b;
}
```

To test the function above, you would pass it a set of parameters, say, “1” and “2,” then check that the output is, indeed, “3.” The actual test would look like this:


```js
const result = sum(1, 2);
expect(result).toEqual(3);
```

If the output returned (`expect(result)`) is equal to the expected result (`toEqual(3)`), the test passes; otherwise, it fails.


> Test Syntax
> Throughout this Exercise, a specific syntax has been used to demonstrate how tests work and what a test script might look like. This syntax is determined by whichever test runner is being used to perform the tests. The examples so far have used assertion syntax, which is used by the test runner Jest. You’ll be learning more about test runners and test syntax later on in this Exercise and in Exercise 4.4: Unit Testing.

Integration Testing

As mentioned earlier, integration testing takes place after unit testing is finished. Once each function has been tested individually, it can be tested in combination with each of the other functions to ensure their compatibility and cohesiveness. After all, just like with the bicycle, even if each piece works fine by itself, you won’t get very far if the gears don’t fit onto the wheels, the saddle doesn’t fit onto the frame, and the pedals keep falling off!

In web development, integration testing is most commonly used to check whether the frontend and backend of an app cooperate with one another. Take the following example:


```js
function login(username, password) {
   if (!isValid(username, password) {
     showAccountPage(username);
   }
}
```

This `login();` function calls two other functions (`isValid();` and `showAccountPage();`) upon execution. The `isValid();` function sends a request to the backend that confirms the accuracy of an entered username/password pair. If the username and password are found in the database, the `showAccountPage();` function opens the account page associated with that username.

A round of unit testing will have already checked to ensure the validity of both the `isValid();` and `showAccountPage();` functions; however, the encompassing `login();` function still refuses to cooperate. How could it be that the `login();` function doesn’t work, but both of the sub-functions inside of it have been confirmed to function correctly?

This is what integration testing is all about! Only by testing each of the larger “integrated” functions would you be able to discover and fix this error, which, in this case, is simply a mistake in syntax. In its current form, the `login();` function is checking the exact opposite of what you want to check—that the entered username/password pair is invalid rather than valid. This is due to the pesky `!` before the `isValid();` function. With integration tests, you can prevent this from happening. Just like with unit testing, you write a test that passes a set of specified parameters to the function in question, then compares the result with the expected result.

Acceptance Testing

Acceptance testing differs from the methods outlined above in that it’s conducted by neither the development team nor QA/QC engineers but, rather, the business users of the product.

Let’s refer back to the bicycle example. Say that a bicycle manufacturer is working on a new line of bicycles and, in order to get feedback, they send early versions of their bike to a selection of loyal customers. They would probably ask these users to fill out some kind of survey (or invite them to a short interview) where they can comment on their experience with the bike; from this feedback, the manufacturer could glean whether or not the bikes are ready for market.

Business users don’t have to be people outside of the company—they simply need to be people who would use the project (and who aren’t familiar with the technology behind it). This could be colleagues on a different team. Take a banking company, for instance. Say this company is developing a piece of banking software. As their own accountants would likely benefit from such software, they’d be prime candidates for conducting acceptance testing. Once the product has passed its unit and integration testing, it can be sent to the accountants, who’d conduct their own series of manual tests. Depending on the results of the tests, the product could either be released or sent back to the development team for further testing and bug fixing.

All of this sounds like a really great way to get a product tested by real users before its release; but what about smaller companies in which there are no employees who might use the product in their daily work (like the accountants from the example above)? Can they still conduct acceptance testing? The answer is yes—they just need to go about it in a different way. Rather than relying on their own employees to conduct the test, they can roll out a test or “beta” version of their product to a small portion of users (typically somewhere between 5% and 10% of their user base). Based on the retention rate (or by way of a survey), they can then determine whether or not the product is ready for launch. You’ve probably seen this method of testing being employed yourself, either with new software you’ve downloaded or games you’ve played.

There remains one problem with the two methods discussed above, and that’s speed. In agile development, the models above would take too much time. As things need to be done in two-week sprints, this doesn’t allow room for sending out test copies and waiting for feedback, whether your testers are in-house or otherwise. This is another area where automated frameworks can come in handy. You’ll be learning how to do acceptance testing via a framework called Cucumber later in this Achievement.

End-to-End Testing

In web development, end-to-end testing refers to the process of testing an entire website or app, starting from opening a browser (one end) until a user finishes their task (the other end). Though you may not think it, this type of testing can be done using automation frameworks, as well. Frameworks have become so advanced nowadays that they can do almost everything—from opening a browser to navigating to a website, simulating user inputs (clicking, typing, etc.), calling an API, and more.

Take a look at the short end-to-end test coded below:


```js
test('login to account page', () => {
   const browser = launch({
       environment: "chrome"
   }); // Launch chrome browser
   // Open careerfoundry website
   browser.open("https://careerfoundry.com").then((page) => {
       page.type("#username", "walle"); // Fill in "walle" in the username input
       page.type("#password", "P@ssw0rd"); // Fill in "P@ssw0rd" in the password input
       page.click("#login"); // Click on the login button
       expect(page.url).toEqual("https://careerfoundry.com/account/") // We expect to see account page since the username and password are correct
   });
});
```

The test above:


1. Opens the browser, in this case, Chrome (const browser = launch({ environment: "chrome"});
1. Navigates to "https://careerfoundry.com" (browser.open("https://careerfoundry.com")
1. Enters a username and password into the correct fields (page.type("#username", "walle"); page.type("#password", "P@ssw0rd");)
1. Clicks the login button (page.click("#login");)

Once it’s completed these actions, it compares the actual result with the expected result, which in this case, is the page “[https://careerfoundry.com/account/](https://careerfoundry.com/account/)”. You’ll notice that the code for this is the same as in the unit test example shown above (`expect(page.url).toEqual("https://careerfoundry.com/account/")`).

End-to-end testing relies heavily on the environment (the backend, the browser, the operating system, the network connection, etc.), as well as the actions a user takes. That’s why it’s essential that you 1) take network conditions into consideration; 2) create tests for many different browsers/operating systems; and 3) cover as many different user interactions as possible.


##### Test Runners

With so many different ways to test your code, you’d imagine there would be a variety of frameworks, or “test runners,” for helping you do so—and you’d be right! Some of the most popular (and amusingly named) test runners include [Jest](https://facebook.github.io/jest/), [Puppeteer](https://pptr.dev/), [Protractor](https://www.protractortest.org/), [TestCafe](https://devexpress.github.io/testcafe/), [Selenium](https://www.seleniumhq.org/), [Cypress](https://www.cypress.io/), [Jasmine](http://jasmine.github.io/), [Mocha](https://mochajs.org/), [Chai](http://chaijs.com/), [Cucumber](https://cucumber.io/).

Don’t let the large number of tea- and puppet-related names above overwhelm you—you don’t need to learn (or even remember!) all of them. As they each work in very similar ways, becoming familiar with one is usually enough—if you need to switch to a different one in the future, most of your knowledge can be passed over. But which one should you choose? This mostly depends on the type of testing you want to conduct. Take a look at the table below that lays out which test runners are compatible with which types of testing quite nicely:


###### Figure 1.

This Achievement will focus mostly on Jest, a testing framework developed by Facebook (the creators of React). However, as Jest only covers unit and integration testing, you’ll also be taking a look at Cucumber, which is a great (and popular!) choice for conducting acceptance tests.


> Tip!
> When searching for more info on Jest or other test runners online, you may see them sometimes referred to as “frameworks” rather than test runners. Don’t worry! Similar to the libraries vs. frameworks debate, this is just another term some developers use to refer to these suites of testing tools. In the end, it doesn’t really matter what you call them—test runners, testing frameworks, testing environments—so long as you understand what they are and what they do.

Now that you know a bit more about the types of tests developers conduct on their code, you’re ready to move onto the testing process itself, which means the introduction of a very important term in the world of web development—test-driven development!


#### Test-Driven and Behavior-Driven Development

There's been an increasing buzz around the acronyms TDD and BDD (which stand for “test-driven development” and “behavior-driven development” respectively) in recent years, and for good reason—they’re two of the most popular processes when it comes to developing and testing software. Let’s take a look at each one in more detail for a better idea of how they work, why they’re so important, and how they differ from one another.


##### Test-Driven Development (TDD)

TDD, or test-driven development, is more than just a testing process—it’s an entire software development process, and one that specifically focuses on unit testing to ensure the integrity and success of a piece of software. When working under a TDD mindset, testing itself is what helps to mold and shape the final product. Once the requirements of an app are set in stone, they're converted into test cases, and the final code for the app is implemented on the basis of these test cases.

The major difference between TDD and traditional programming lies in when tests are written. In traditional programming, the code for an app or feature is written first, and only then are tests developed for QAing the code. Conversely, in TDD, developers write tests before they implement any code. While this might seem backward, it actually helps developers better focus on the code that they write, as there’s no way they’ll end up writing anything that doesn’t support a test case/requirement. This type of development process also caters more for short development cycles, making it easier to get immediate feedback and deliver high-quality code while following an Agile approach. These benefits are what have helped TDD rise to the forefront of web development in recent years, having already been adopted by dev teams at Google, Microsoft, Pillar, CleanCoders, Allstate, and other major players in the industry.


> TDD and Agile
> Want to see for yourself how much easier it is to follow a TDD mindset when working in an Agile environment? Check out this blog posts that details the importance of TDD in Agile development.

A project built following TDD processes will likely have a much higher test coverage rate than one developed using a more traditional approach (>= 80%). Test coverage rate, here, refers to how much of your code has been tested. For instance, if you had ten functions in your code, and nine of them had been tested, you could say that you had a test coverage rate of 90%. Higher rates of test coverage make for easier refactoring of code, as there’s significantly less chance that you’ll break any of the existing code when updating or adding new features.

The TDD process can generally be divided into six steps:


1. (Re-)Write a test
1. Test fails
1. Implement a fix
1. Test passes
1. Clean up code
1. Repeat

Of course, reading a list of steps is much easier than actually understanding it. To really dive into what goes on behind the scenes for each of the steps above, let’s walk through your upcoming Achievement project—the Meet app—using these steps as guidelines for exploring how you’ll follow a TDD approach to complete your final app.


##### 1. (Re-)Write a Test

![An arrow with the words “Step 1” pointing to a circle with the words “(Re)Write a test”](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/step-one-rewrite-test.jpg)


###### Figure 2.

The first step involves, as you might expect, writing a test. As mentioned above, all of your tests should be based on the requirements of the app. By developing your code in such a way as to pass all the tests you write, you’re also ensuring that it meets all the initial requirements you laid out for the app.

Suppose one of the requirements of your Meet app is a feature that enables users to click a “Show details” button to reveal more information about an event. You’d write a test that 1) clicks a button; 2) checks whether the information for the corresponding event is already expanded or not; 3) if not, it expands the information; and 4) checks the actual results against the expected results (an expanded block of information).


##### 2. Test Fails

![An arrow with the words “Step 1” pointing to a circle with the words “(Re)Write a test” and an arrow with the words “Step 2” pointing to a red diamond with the words “Test fails.” Another smaller arrow connects Steps 1 and 2 in a cyclical motion.](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/step-two-test-fails.jpg)


###### Figure 3.

Now that you’ve written a test, you’ll, of course, want to actually run it! In this second step, all the tests you've written are run on the app. You might be thinking—if I’m writing the test before writing any code, then won’t the test always fail? And you’d be right! That’s why this second step is called “Test Fails.” You actually expect it to fail at this stage of the process, as if it does somehow pass, this either means that 1) you’ve already added that feature at some point in the past, or 2) the test itself is flawed.

As you haven’t written any code for expanding information about events upon button-click in your app, the test obviously fails. Great! This means that users don’t currently have the ability to expand information within your app. As this is one of your requirements, you know this needs to be fixed, so you move on to step three.


##### 3. Implement a Fix

![Three arrows pointing to the first three steps of test-driven development: “(Re)Write a test,” “Test fails,” and “Implement.”](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/step-three-implement.jpg)


###### Figure 4.

This is where the actual development of “test-driven development” comes into play. What code do you need to write to ensure that the test you wrote passes? Write it here! At this stage, code quality isn’t important. Your main goal is simply to write code that makes the test pass.

You set to work writing however much code is necessary to ensure that whenever a user clicks on a “More details” button within your app, the information about that event will expand to reveal its full contents.


##### 4. Test Passes

![Four arrows pointing to the first four steps of test-driven development— “(Re)Write a test,” “Test fails,” “Implement,” and  “Test passes.”](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/step-four-test-passes.jpg)


###### Figure 5.

With your new code in hand, it’s time to move onto the fourth step—re-running your tests. The difference between this step and step two is that, this time, you expect your code to pass (or, at the very least, you hope it will pass). The purpose of this step is to ensure that the new code you wrote does, in fact, work, and that it doesn’t break any existing features either. If any of the tests fail, you’ll need to adjust your new code until they pass.

You run your tests again, and they pass! This means that a user can now successfully press a button to view more details and information about an event. You can also rest assured that the code you added for this button doesn’t break any of your previous code.


##### 5. Clean Up Code

![Five arrows pointing at the first five steps of test-driven development, from (Re-)write test through to “Clean up code”](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/step-five-code-cleanup.jpg)


###### Figure 6.

In step three, you implemented new code that would ensure your test would pass. Your mindset at the time was on writing effective code—not, necessarily, the more efficient or highest-quality code. That’s why, in this step, you need to revisit the code you wrote and optimize it as best you can. Always keep in mind, though, that after optimization, your tests still need to pass!

You revisit the code for your button, taking small steps to optimize what you can and ensure that everything is legible and efficient. Once finished, you run your tests a final time to make sure your optimization efforts didn’t break anything.


##### 6. Repeat (optional)

And that’s it! This last step is more of a symbol representing the cyclical nature of the process. Once you reach the end of the steps for one of your tests, repeat the steps for the next, and the next, and the next after that. You keep cycling through the steps until every one of your requirements for the app or feature has been met.


##### Behavior-Driven Development (BDD)

BDD, or behavior-driven development, is a process very similar to TDD. In fact, it also follows the six steps listed above for creating and running tests. This is because BDD is more of an extension of TDD. Rather than taking an entirely new approach, it makes small changes to the way some of the tasks in TDD are handled—mainly, the test cases. Whereas TDD focuses more on the developer who’s writing the app, BDD focuses on a wider range of stakeholders, advocating for test cases that are easy for developers and non-developers to understand.

In BDD, a test case is referred to as a scenario (or, sometimes, an acceptance criteria). A scenario is usually part of a larger user story (also known as an app requirement). Scenarios describe interactions between a user and the application, and they’re written using clear language that all stakeholders can understand, with minimal technical details. Once all the scenarios within a user story have been implemented, that user story is considered to be finished.

Scenarios are typically written in a language called “Gherkin.” Gherkin attempts to make scenarios as clear and succinct as possible while ensuring that all stakeholders can understand them. To write a scenario in Gherkin, you start with a short description, then use a special “Given-When-Then” syntax to convey what needs to happen to fulfill the requirements of the scenario.


- Given represents the context of the scenario. In what type of situation would this scenario arise?
- When represents the user interaction or behavior. What does the user need to do for this scenario to come into play? Ideally, you should narrow this down to one action per scenario.
- Then represents the expected outcomes of the scenario. What should happen when the user performs this specific action in this specific content?

Let’s walk through the same example for your Meet app in the TDD section above using Gherkin. In TDD, you’d write:


> Suppose one of the requirements of your Meet app is a feature that enables users to click a “Show details” button to reveal more information about an event. You’d write a test that 1) clicks a button; 2) checks whether the information for the corresponding event is already expanded or not; 3) if not, it expands the information; and 4) checks the actual results against the expected results (an expanded block of information).

But in BDD with Gherkin, you’d rephrase it to something like:


- Scenario: User can expand an event to see its details
- Given the list of events has been loaded
- When user clicks on “Show details” button for an event
- Then the event element will be expanded to show the event details

From the two examples above, the difference between TDD and BDD is fairly clear—TDD focuses on the actual implementation, while BDD focuses on the user behavior (just as its name behavior-driven development implies).

You’ll dive deeper into BDD and the Gherkin language later in this Achievement, when you’ll be writing BDD test scenarios for your app. For now, though, let’s back up slightly and explore how you can get those scenarios to begin with—by translating your app features into user stories!


#### Translating Specs into User Stories

In BDD, test cases are referred to as scenarios. Before you can jump into writing scenarios for your app, however, you first need to understand what it is you’re even building. The best way to do this is through user stories.

Per the “Your Project Features & Scenarios” section of this Achievement’s [Project Brief](https://images.careerfoundry.com/public/courses/fullstack-immersion/full-stack-project-briefs/A4-Project-Brief-Jun2023.pdf), your app needs to possess six key features:


- Feature 1: Filter Events By City
- Feature 2: Show/Hide Event Details
- Feature 3: Specify Number of Events
- Feature 4: Use the App When Offline
- Feature 5: Add an App Shortcut to the Home Screen
- Feature 6: Display Charts Visualizing Event Details

While a features list like this gives you a much better idea as to the app you’re going to be building, it still leaves a bit to be desired when it comes to the exact functionality required of each feature. A good way to fix this is by translating these features into user stories.

Back in your Intro to Frontend Development Course, you learned that a user story usually follows this format:


> As a [role],
> I should be able to [action]
> So that [benefit].

Try to rewrite each of the required features using the format above. Let’s consider the first one together: “Filter events by city.” How might you convert this to a user story?


> As a user,
> I should be able to filter events by city
> So that I can see a list of events taking place in that city.

The user story starts by describing the role: a normal user of the app. Then, the action: filter events by city. And finally, the benefit: to see a list of events that take place in that city. You now have more information to work with than the single line “It must be able to filter events by city” from the Project Brief. But there’s more you can do to break this down further.

User stories, by nature, can be quite large, and they often require a variety of smaller features and functionality in order to fulfill them. This is why breaking user stories down into multiple scenarios can help you pinpoint each individual function that needs to be implemented in your app. In most companies, especially larger companies, this process is handled by product managers; however, there will be times when you need to break down user stories yourself, especially when working by yourself or on a small team.

For your Achievement project, we’ve already broken down each of the features into scenarios (found in the “Your Project Features & Scenarios” section of the Project Brief).

Great! You now have all the scenarios you need to start writing your tests—or do you? Remember that, according to BDD principles, each scenario needs to be written in Gherkin’s “Given-When-Then” syntax. The scenarios above are only descriptions. Before you can move on, you’ll need to add a context, interaction, and outcome to each of the scenarios outlined above. Let’s give it a try!


> TIP!
> Keep in mind that not every feature within an app is testable. For instance, the “Add an App Shortcut to the Home Screen” feature is actually handled by the user’s OS, not by your app. For this reason, you won’t be testing this feature.


#### Writing Test Scenarios

While the job of breaking down user stories into scenarios is typically handled by a company’s product manager, the task of writing “Given-When-Then” steps can be handled by anyone. For this reason, it’s important that you’re comfortable with writing them yourself. Don’t let this intimidate you! So long as you keep in mind what “given,” “when,” and “then” mean in this context, you won’t have much trouble. Remember:


- Given represents the context of the scenario. In what type of situation would this scenario arise?
- When represents the user interaction or behavior. What does the user need to do for this scenario to come into play? Ideally, you should narrow this down to one action per scenario.
- Then represents the expected outcomes of the scenario. What should happen when the user performs this specific action in this specific content?

A good formatting tip to remember when writing your “Given-When-Then” steps is to use past tense for the “Given” clause, present tense for the “When” clause, and future tense for the “Then” clause. This can help you avoid mistakes and keep your scenarios consistent.

Also keep in mind that your test scenarios don’t need to be anywhere near perfect at this point. Writing them is a skill, and one that you’ll get better at over time. For now, feel free to get creative, think outside the box, and have a bit of fun!

Let’s start by finishing up the scenarios for the first feature of your app: “Filter events by city.”


> Scenario 1
> When user hasn’t searched for a specific city, show upcoming events from all cities.
> 
> 
> Given user hasn’t searched for any city;
> When the user opens the app;
> Then the user should see a list of upcoming events.
> 
> 
> Scenario 2
> User should see a list of suggestions when they search for a city.
> 
> 
> Given the main page is open;
> When user starts typing in the city textbox;
> Then the user should receive a list of cities (suggestions) that match what they’ve typed.
> 
> 
> Scenario 3
> User can select a city from the suggested list.
> 
> 
> Given user was typing “Berlin” in the city textbox AND the list of suggested cities is showing;
> When the user selects a city (e.g., “Berlin, Germany”) from the list;
> Then their city should be changed to that city (i.e., “Berlin, Germany”) AND the user should receive a list of upcoming events in that city.

Congratulations! You now know the basics behind writing tests and scenarios for your app. While you won’t be writing any actual tests in the Task for this Exercise, you will be writing more scenarios, which will give you plenty of practice in translating user stories into clear, delineated functions. Having a solid foundation of requirements for an app is crucial before writing any code, especially when adopting a test-driven approach to development!

Before finishing up this Exercise, let’s get a few things ready to go technology-wise so that you’ll be able to get up and writing tests and code in the coming Exercises. You’ll start by setting up your React application for your Achievement project, then learn how you can deploy it to GitHub Pages. Let’s get started!


#### npm create vite

In the previous Achievement, you built your React app from scratch, learning about the build process and everything that goes into a React app. In this Achievement, however, you’ll be able to streamline things a bit by way of a tool called Vite. By using npm create vite, you can skip a lot of the manual work at the beginning of a React project. What it does is create a “starter” project, or template, that includes all the build tools and files you may need as you begin developing your app. In essence, all the bundling, minification, and auto-prefixing you had to do manually in the previous Achievement for your myFlix app will be handled automatically by Vite with only one command—pretty convenient!

Of course, now you might be wondering why you didn’t learn about Vite in the previous Achievement. This is because Vite hides almost everything related to the build process. While this is handy and saves a lot of time, it makes it impossible for you, as a new developer, to learn how it all works. You need to understand the basics before you can start taking shortcuts!


##### What is Vite?

Vite is a build tool designed specifically for frontend development that works great with frameworks like React, Vue, Svelte, and Angular. The key advantages of Vite over other build tools like Webpack or Parcel are its ES module-based bundling and on-demand loading, both of which make it a very quick and efficient tool to work with.

Key Features of Vite


1. Instant Server Start: Vite uses native ES modules, which allows development servers to start up quickly, even with large projects. Unlike Webpack, which bundles everything upfront, Vite loads JavaScript files on an as-needed basis, decreasing the startup time.
1. Hot Module Replacement (HMR): Vite supports hot module replacement, or HMR. What does this mean for you? Well, you’ll be able to see any changes you make quickly without needing to reload, which can be very handy when working with UI components.
1. Pre-bundling and Optimized Builds: Vite optimizes project dependencies by pre-bundling them using es-build, a bundler written in Go. This reduces cold-start times and improves build performance.
1. ESM (ECMAScript Modules): Vite takes advantage of modern browser support for native ES modules. This simplifies module loading and makes it faster.
1. Fast Production Builds: Vite uses Rollup, an optimized bundler, during production, which makes for smaller, more efficient builds.
1. Plugin Ecosystem: Vite comes together with a growing ecosystem of plugins that allow it to do additional things such as handle different file types (CSS preprocessors, JSX, etc.), integrate backend systems, and more. Vite plugins are compatible with Rollup plugins thanks to their shared architecture.
1. Framework Agnostic: Though Vite is often associated with Vue, it can be used across many different frontend framekworks, making it incredibly versatile.


##### Setting Up Your Meet App

Now that you know what Vite is and how it can make your life easier when creating apps in React, let’s put what you’ve learned into practice by creating your Meet app using Vite. Start by creating a new project for your app using npm.

The npm command would be:


```js
npm create vite@5.3.0
```

This command will set up a project using the Vite tool. If this is your first time running the command, you’ll be prompted to install the packages needed to set up this and upcoming projects with Vite. Type 'y' if you get this prompt.

Once the packages for Vite are downloaded, you’ll be prompted to type the name of your project. Let’s call it “meet”:

![Naming your meet app.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure7.png)


###### Figure 7.

Then, you’ll need to select the Framework to use. From the available options, select React, followed by the variant Javascript + SWC.

![Selecting React.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure8.png)


###### Figure 8.

![Selecting JavaScript plus SWC.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure9.png)


###### Figure 9.

These steps will create the following project structure within your new “meet” folder:


```js
meet
├── index.html
├── package.json
├── public
│   ├── vite.svg
├── README.md
├── src
│   ├── App.css
│   ├── App.jsx
│   ├── assets
│   │   ├──react.svg
│   ├── index.css
│   ├── main.jsx
│
├── .gitignore
├── vite.config.js
```


> Hidden Files
> As you encountered in earlier Achievements, your .gitignore file is a hidden file and isn’t visible by default when you view your file directory in the file explorer/finder on Windows or Mac. If you want to view this file, use this guide to learn how to show hidden files on Windows and Mac]
> 
> Alternatively, you can view hidden files using VSCode by default, simply by opening the project directory in VSCode. However, in case your VSCode app is configured differently and your hidden files aren’t being shown, check out this StackOverflow thread about .gitignore and VSCode.

The “public” folder contains what Vite builds when you run it (when you run `npm run dev`). It's exactly like the “dist” folder you used in previous Achievements.

The “src” folder contains your source code.In this folder, you can see a few JavaScript files. The two of interest right now are `App.jsx` and `main.jsx`, which are both generated by Vite. `main.jsx` is the entry point of your build and `App.jsx` comes right after it in the dependency tree (`App.jsx` is imported by `main.jsx`). You’ll add your app configurations to `main.jsx`, but the main React code will be added to `App.jsx` and its child components.

In order to get your new app up and running, you first need to install the packages listed in `package.json`. Navigate to your “meet” directory within your terminal and run `npm install`. Once the packages are downloaded, you can start the app by running the command `npm run dev`:


```js
cd meet
npm install
npm run dev
```

After running these commands, you should be able to navigate to your app in the browser under the URL`http://localhost:5173/`:

![Navigating to your app.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure10.png)


###### Figure 10.


##### Deploying Your App to Vercel

You used GitHub Pages to host your portfolio site in the Intro to Frontend Development Course, and your Pokédex app in Achievement 1 of the Full-Stack Immersion Course. Then, you deployed your movie-api to Heroku or render. Finally, you deployed your myFlix-client to Netlify. For this Achievement, you’re going to be using the Vercel platform to deploy your application. But first, you need to set your project up in your Github account.

1. Create a new GitHub repository for your project.

Head over to [GitHub](https://github.com/new) and create a new repository:

![Create a new repository window on GitHub. The words “APP_NAME” appear under the “Repository name” field, and the “Public” option has been selected for the type of repository.](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/github-new-repository.png)


###### Figure 11.

Enter the name of your app under “Repository name” (for the sake of future examples, we’ll be using the name “meet”) and then click on “Create repository” (leaving the other options as they are by default).

You’ll be taken to the setup page shown in Figure 9, which will guide you through setting up your project.

![GitHub’s setup page, showing “Quick setup” for those familiar with the process.](https://images.careerfoundry.com/public/courses/fullstack-immersion/A4/4.1/github-setup-page.png)


###### Figure 12.

2. Link the new GitHub repository to your meet app project.

Back in your terminal, start by `add`ing your remote URL so you can link your local project to the GitHub repository you just created. You might want to run `git init` to initialize Git in the project folder just in case it wasn't initialized by default:


```js
git init
git remote add origin https://github.com/YOUR_USER_NAME/meet.git
```

Once again, make sure you replace “YOUR_USER_NAME” with your GitHub username and “meet” with the name of your repository if you used a different name.

Next, add your changes and commit them:


```js
git add .
git commit -m "First commit"
git branch -M main
```

To push your committed changes, use the command:

`git push -u origin main`


> Athentication via the Terminal
> You may be asked to provide your GitHub user credentials when running this command to authenticate yourself. When prompted for your password, you’ll need to share your “personal access token,” which you’ll find in your GitHub settings when logged into the browser.
> 
> There are two types of tokens, “classic” and “fine grain,” and you can use either. These tokens are commonly used to grant other GitHub users access to your project, so you’d usually only allow the permissions required for each collaboration. For this task, you’ll use the token in place of your password, so grant all possible permissions and give the farthest possible expiration date.
> 
> Follow the instructions in these links to create your personal access token:
> 
> 
> How to Authenticate on GitHub with Two-Factor Authentication (Classic Tokens)
> Creating a Personal Access Token (Classic and Fine-Grain Tokens)
> 
> 
> Once you’ve created your token, copy it, then re-run git push -u origin main in the terminal to prompt the authentication. When asked for your account password, paste your new token (not your password).

3. Create a Vercel account.

Vercel is a hosting and deployment platform optimized for frontend applications and static websites with focus on modern frameworks such as React, Next.js, Vue.js, and Angular among others. To deploy a project on Vercel and link it with a project on GitHub, you’ll need to follow these steps:

Create an account on Vercel: If you don’t have an account on Vercel yet, go to the official [Vercel website](https://vercel.com/) and sign up. Choose the free “Hobby” version:
![Creating a Vercel account.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure13.png)


###### Figure 13.

Now, use your GitHub account to facilitate the integration:
![Integrating with GitHub.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure14.png)


###### Figure 14.

Log in to Vercel: After creating an account, log in to Vercel using the GitHub option. Generally, after registration, the login is automatic. You should see the following welcome screen:
![Logging in to Vercel.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure15.png)


###### Figure 15.

Import project to Vercel: Click the “Import Project” option to link your repository and deploy a new Vercel project:
![Importing your project.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure16.png)


###### Figure 16.

Install the Vercel GitHub extension: You’ll now see a section on the screen where you can select a repository from GitHub and load it into your newly created Vercel project. The first time you do this step, it will ask you to install a GitHub extension for Vercel. Install this extension:
![Installing the GitHub extension.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure17.png)


###### Figure 17.

Select the GitHub account where your repository is located:
![Selecting the GitHub account.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure18.png)


###### Figure 18.

You’ll need to grant access to your GitHub so that Vercel can access your repositories, then finalize the installation:
![Granting access to GitHub.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure19.png)


###### Figure 19.

Import your repository: After installing the GitHub extension, you’ll be redirected to the previous section, where you can select the repository to link with the project. You’ll see a complete list of your repositories. Simply select the one you need and import it:
![Importing your repository.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure20.png)


###### Figure 20.

Project configuration: Vercel will automatically detect that your project is a React application and apply the default configuration. Generally, you don’t need to change anything, but you can review and adjust the configuration if necessary. Once finished with the configuration, click the Deploy button:
![Using automatic configuration.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure21.png)


###### Figure 21.

Deploy the application: Once you’ve connected your repository, Vercel will start building and deploying your application. This process includes:


- Installing Dependencies: Vercel will install all dependencies defined in your “package.json” file.
- Building the Application: Using the command npm run build (or yarn build if you use Yarn), Vercel will build an optimized version of your application.
- Deployment: The application will be deployed at a URL provided by Vercel.

After the application build is complete, you’ll be redirected to a new screen that indicates how your deployment went. If there are no errors, you’ll see a congratulations screen, where you can click Continue to Dashboard:
![Continue to Dashboard.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure22.png)


###### Figure 22.

Application URL and testing: Once you click on the Dashboard of your newly deployed application, you’ll be able to see more-detailed information about the number of deployments you’ve made, the commits and branches of your linked repository, as well as the current status of the application. To view the application live, click the Visit button:
![Clicking the Visit button.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure23.png)


###### Figure 23.

The generated link is the public URL of your application in production. You now have your first deployed React application in Vercel! Now, it’s time to test and verify that everything works correctly.
![Your live app on Vercel.](https://coach-courses-us.s3.amazonaws.com/public/courses/fullstack-immersion/A4/4.1/figure24.png)


###### Figure 24.

4. Updates and further re-deploys.

Every time you make a commit and push to your main branch (or any other branch configured for deployment), Vercel will automatically rebuild and redeploy your application with the changes made. This makes the continuous deployment process extremely simple.


#### Summary

In this Exercise, you learned about the four most common types of software testing—unit tests, integration tests, acceptance tests, and end-to-end tests. You also covered how to convert feature requirements into user stories, which can then be further broken down into test scenarios using BDD’s Gherkin syntax. Next you walked through the six basic steps of TDD, exploring how you’ll be able to use this approach yourself in the Meet app you’ll be building for your Achievement project. Finally, you got your new React app up and running on Vercel, which will allow you to easily share and view your app as you’re working on it.

In the next Exercise, you’ll be exploring a brand-new type of backend architecture—serverless functions—which will be key to allowing users of your Meet app to continue using your app while offline. For now, though, let’s take what you’ve learned and put it into practice by writing some more scenarios for your application!


#### Resources

If you’re curious to read more about the topics covered in this Exercise, then we recommend taking a look at the resources below. Note that this reading is optional and not required to complete the course.


- What is Test Driven Development (TDD)?
- Product Backlog Explained

Take the quiz to test your knowledge on this Exercise.


#### Task


- Direction
- Submission History

Before getting started on this task, download your [Project Brief PDF](https://images.careerfoundry.com/public/courses/fullstack-immersion/full-stack-project-briefs/A4-Project-Brief-Nov2023.pdf) for this Achievement. You’ll need it now and throughout the entire Achievement.

This task will see you doing three different things in order to get your new React app up and running:


- You’ll start by converting the rest of your app’s key features into user stories, then use the “Given-When-Then” syntax to deliver a full list of scenarios based on the scenarios for features 2-6, as found in the project brief.
- Next, you’ll use Vite to build your React project and publish it using GitHub and Vercel.
- Finally, you’ll create an Amazon Web Services (AWS) account. You’ll need this account once you begin exploring serverless functions in the next Exercise. As it can take up to 24 hours to create an account, we ask that you start the process in this task. This will ensure your account is ready for the next task. Note that this account is not the same as the Amazon account you use for shopping!


> Bonus: Spotlight on AI
> 
> Collaborate with AI to Define User Stories and Gherkin Syntax
> 
> Why not use a generative AI tool to help define the deliverables for this task? For example, you could prompt an LLM tool like ChatGPT to convert the app’s features into user stories with a prompt like: ”You’re a product manager, and you need to turn the following features into user stories: [features]. The user stories should follow this structure: As a [role], I should be able to [action], so that [benefit]....
> 
> AI can also help you with your Gherkin syntax. For example, you could share a scenario with the AI, and ask it to convert the scenario into Gherkin’s Given-When-Then format, with a prompt such as: ”I'm working on an events app. I’m working on scenarios for [feature]. Convert the following feature scenario into Gherkin syntax: [scenario]”.
> 
> 
> 
> An example of a prompt requesting that the AI converts a feature scenario into Gherkin’s Given-When-Then syntax.
> 
> This is a great example of how AI can help to speed you up when working on specific, manual tasks that occur in almost every project. Note: Due to data privacy concerns, when working on professional projects, there may be limitations to the type of information you’re permitted to feed into the AI tool.
> 
> As always, remember to check the generated syntax before mindlessly copy-pasting it for your project. As you know by now, errors and inaccuracies in AI-generated content can be subtle and hard to detect, but it’s your responsibility to ensure they don’t find their way into your codebases.
> 
> If you decide to use AI to help with these parts of your task, be sure to explain to your instructor how you did so and share any comments about how it helped (or hindered!) you when completing the task.

Directions


1. Convert features 2-6 (detailed in the Project Brief) into user stories using this format:


As a [role],
I should be able to [action]
So that [benefit]
1. Convert the scenarios for each of the features in your Project Brief into Gherkin's (“Given-When-Then”) syntax.
1. Add a README.md file to your repo. Add your user stories and scenarios for all 6 features to it, and also include some basic information about the project. You can use the information in your project brief.


If you need a refresher on README.md files, revisit Achievement 1.
1. Create and deploy your React app to Vercel following the steps in the Exercise. Check that everything worked correctly by navigating to the URL for your site once it’s been deployed.
1. Create a zip file of your project repository on your computer.
1. Submit your zip file, as well as the links to your deployed meet site and GitHub repository here. Feel free to share additional thoughts or ask questions on your submission page.
1. Follow the steps provided in this tutorial for creating an AWS account to create your own AWS account. You’ll be using it in the next Exercise.

Rubric

Refer to the categories below to see how to meet the requirements of the approved stage

![](https://cdn.careerfoundry.com/assets/rubrics/not_yet-c9fb80e521507759d546f847f8a65a00c66f2c8ec7ece4e37f98c25aa122778c.svg)


- App created with Create React App but not on GitHub Pages or contains errors; AND
- User stories and scenarios for project features are unclear and inaccurate; AND
- “Given-When-Then” syntax hasn’t been employed

![](https://cdn.careerfoundry.com/assets/rubrics/almost_there-f4bb1c077a0a826e7d4e3ecb72859fc401d362d9bd49c0658f4fd85c4a047a87.svg)


- App created with Create React App and on GitHub Pages; AND
- Project features converted into user stories, though some may be inaccurate; AND
- User stories converted into scenarios using the “Given-When-Then” syntax, though some may lack clarity and/or accuracy

![](https://cdn.careerfoundry.com/assets/rubrics/approved-7dfdcf59318cf52fcbd1333d8b71bf7a2bde35b6e0b753ac975349982495e0b4.svg)


- App created with Create React App, is on GitHub Pages, and contains no errors; AND
- Project features converted into accurate user stories; AND
- User stories converted into clear and accurate scenarios using the “Given-When-Then” syntax

.