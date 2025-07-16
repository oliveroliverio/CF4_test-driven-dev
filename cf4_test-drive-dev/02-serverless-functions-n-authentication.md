

## Testing in the Development Process


# 4.2: Intro to Serverless Functions & Authentication


- Learning Goals
- Introduction
- Cloud Computing Platforms
- Categories of Cloud Services
- Serverless Development
- Serverless Functions
- Using Serverless for Authorization
- Creating an OAuth Consumer
- Setting Up AWS Lambda
- Summary
- Resources
- Task
- Forum


#### Learning Goals


- Evaluate the merit and usefulness of serverless development
- Connect a React app with a protected API
- Prepare an OAuth client for authorization and authentication
- Obtain AWS credentials for future use


#### Introduction

Hello and welcome back! This Exercise will focus primarily on serverless architecture. The previous Exercise was all about testing—what it is, why it’s important, and the different types of testing available for software developers. You even explored some of the most common automated testing methods (unit testing, integration testing, user acceptance testing, and end-to-end testing) before diving straight into the “big two” development mindsets: test-driven and behavior-driven development. Using your newly acquired knowledge, you began thinking up test scenarios for your Achievement 4 project, which will be a Meet app for scheduling and attending events in your city. As a final step, you set up and deployed your new React app to GitHub Pages, preparing it for all the new advanced syntax you’ll be adding to it throughout this Achievement.

The first Exercise of this Achievement took care of your introduction to software testing. Now it’s time to get you up to speed on the next large concept: serverless architecture. “Serverless,” here, doesn’t refer to the complete lack of a server but rather the absence of a physical machine for your server. But how does that work? And what can it do for your app (and you as a developer)? You’ll find answers to these questions in this Exercise. After an introduction to cloud computing and serverless providers, you’ll learn how to set up an authorization server for your Achievement project using the Google Calendar API and AWS Lambda. Get ready to go serverless!


#### Cloud Computing Platforms

No doubt, you’ve heard of the “cloud” before. And no, this isn’t referring to the clouds in the sky (though they probably seem equally as nebulous), but rather, the unseen and all-encompassing digital space to which you can save and retrieve files and data on the internet. A “cloud computer” is nothing more than a computer separate from your own that you can access via the internet and use as your own personal storehouse. This computer, however, can be used for more than simple data storage—it can even be used as a server for an app or site! “This sounds pretty great,” you might think, “but how can I get one for myself?”

Fortunately, many companies offer cloud-based servers for rent (and for all variety of price ranges, from free to shock-inducing). Apps hosted on a cloud-based server are referred to as being “in the cloud.”

![Graph showing public cloud adoption for Enterprises in 2022](images-md/misc/flexera-cloud-adoption_1752706968089.png)


###### Figure 1. Source: State of the Cloud Report 2022

Cloud computing has become a bit of a buzzword in recent years, and with good reason, as it’s opened many doors for developing and deploying apps in new (and more convenient) ways. No longer do personal developers and small (or even large!) companies need to set up and maintain their own servers to get their hot new apps up and running—they can simply rent out space in the cloud from which they can deploy their products, saving them time and money. For this reason, more and more developers are switching to cloud-based service providers, and the top providers continue to see increased usage with each passing year. You’re likely already familiar with some of the most basic services offered by cloud platforms—for instance, file storage services like Dropbox or Google Drive—but cloud platforms also provide a wide range of other services. Cloud-based database services (like the one you used for your myFlix API), networking services, and app services all make it easier to start building your app without worrying about how or where your users will be able to access it. This allows you, the developer, to focus on what you do best: writing code.


#### Categories of Cloud Services

Cloud-based service providers are typically categorized according to the level of completion of the package they provide—in other words, how much of the service’s, well, service you need (and how much you can handle yourself). You’ll come across four main types of cloud service: IaaS, PaaS, SaaS, and FaaS. If all those double a’s leave you at an imPaaS, never fear! Each one will be explained in more detail shortly (with a focus on the one you’ll be using for your Achievement project!):


##### Infrastructure as a Service (IaaS)

The simplest type of cloud-based service provides—as you might guess—an infrastructure for your site or app. Here “infrastructure” refers to anything from virtual servers to storage or networking services. IaaS tends to be the most flexible of the types of services as you, the developer, still have control over most of your app—the service simply hosts your backend. Some common IaaS providers include Rackspace, Digital Ocean, and AWS EC2.


##### Platform as a Service (PaaS)

One step up from IaaS is PaaS (platform as a service). As well as providing an infrastructure for your app, PaaS providers host operating systems (Windows, Linux, MacOS, etc.), development tools, and business-analytics tools that you can use to manage your site or app further. Development tools could be—but are not limited to—IDEs (integrated development environments) to build, test, debug, and also deployment tools. Business-analytics tools could be something like QuickSight or Google Analytics—anything that helps you track traffic and make insights about your user base. As a developer, your PaaS provider would host and manage the server for your app. However, you’d still be responsible for developing and deploying it (and would, thus, be able to make your own decisions on its architecture, technologies, etc.). An example of a PaaS provider you should know very well by now is Heroku, but some others include Microsoft Azure and Engine Yard.


##### Software as a Service (SaaS)

The most comprehensive cloud-service providers are known as SaaS (software as a service). Not only do SaaS providers host your infrastructure and provide development and analytics tools, but they also offer their own cloud-based software apps. The easiest way to think of SaaS is simply as pieces of software that you’re given permission (a license) to use for your purposes, whether for personal use on your own computer or as part of a company. In the past, to use a piece of software, you had to drive down to your local office-supply store and pick up a copy on CD-ROM (or even floppy disk!). Now, however, software is available at the click of a button over the internet, and you can even pay to use it on a subscription model via the cloud. Do you use any Adobe products? These are all offered through their cloud-based service. What about Google Drive? That’s SaaS, too. And how about Slack? All of these are examples of software made available by a cloud service provider.

![Visual representation comparing the different levels of cloud service, IaaS, PaaS, and SaaS, as pizza services](images-md/misc/cf-cloud-pizza-comp_1752706968398.jpg)


###### Figure 2.


##### Serverless, or “Function as a Service” (FaaS)

As a developer, SaaS is often not what you need—you’re already developing your own app, so it’s unlikely you need to use someone else’s app. However, PaaS may not be what you need either, as it can be quite costly, depending on the type of app you’re building. In cases like these, FaaS can come to the rescue. FaaS stands for “function as a service,” but you may also see it referred to as “serverless.” It’s similar to PaaS in that you don’t have to manage your own server, but it comes with a few advantages that can make it a more attractive option; for instance, you only have to pay for the resources you use (as opposed to PaaS, where you pay even if your app isn’t used). FaaS also comes with autoscale by default (as opposed to having to paying for it as an extra), and it’s generally easier to deploy your apps on than PaaS.

All of these reasons make FaaS (or serverless) an excellent choice for the Meet app you’ll be working on for your Achievement project, which is why you’ll be spending the rest of this Exercise learning all about it!


#### Serverless Development

Before diving straight into how serverless development, or FaaS, works (and how you can start using it for your Achievement project!), let’s discuss why you may or may not want to use it for a project. As a web developer, there will be times when you’ll need to decide how to host your app, whether that app is a personal project or the app your company wants to launch. This makes it essential that you be able to weigh the pros and cons of each hosting option. No two apps are the same, and some will lend themselves to one type of hosting over another. While serverless development boasts a host of advantages, it may not always be the best choice, so it’s important to keep the following benefits and limitations in mind.


##### Benefits of FaaS

As mentioned earlier, serverless development has various advantages over typical PaaS providers. Some of these include:


- Fast and Easy Deployment: The whole point of using a service to host your infrastructure is to save time and effort. You no longer have to worry about how to get your app up and running to share with the world—you only need to code. Built-in services make it easy to develop and deploy your app, so you don’t need to implement it from scratch.
- Scalability: FaaS providers perform automatic scaling based on the number of requests received. During times of high traffic, more servers will be allocated to your app, and the number of servers is reduced in times of low traffic. With traditional servers where the machine power (RAM, CPU, etc.) is fixed, the server could crash if the number of requests exceeds its limit.


Cost Efficiency: With serverless, you only pay when the code inside your app is executed, so you’re only charged for what you use. This usually leads to considerable savings over traditional servers, where you pay a flat fee to host your app even when your code isn’t running (i.e., “idle” time). There also aren’t any costs for setting up development, staging, and production machines, and you don’t need your own dedicated engineer to maintain your server like you would if you handled everything in-house. All of these factors can lead to money saved over the more traditional methods of hosting.


##### Disadvantages of FaaS

Despite the advantages just listed, serverless development isn’t the best choice for every app. Here are a few things to keep in mind when deciding on a hosting provider:


- Local Testing and Debugging: Before releasing your app, you may want to test it locally (on your computer) to avoid making changes to—and potentially breaking—the live production app. This is difficult, though, with serverless hosting, as you may not be able to simulate the same environment as in the cloud. You also can’t use standard debugging tools such as breakpoints, which can make fixing issues in your app a chore. The only real option is to look at the online logs to figure out a problem, though some providers are working on a better solution. When choosing a provider, you must look for one that has introduced effective local testing and debugging tools.
- Max Execution Time Limit: A timeout exists when executing serverless functions. This timeout varies based on the provider and can last anywhere from one to 15 minutes. If your function execution time exceeds this limit, the provider will terminate the execution of your function to prevent it from running infinitely. While this can save you money, as serverless functions are charged based on their running time, it can also be limiting if you need to perform tasks that require a lot of processing power and time to finish.
- Cold Start: When a function is invoked for the first time (or for the first time in a while), it will be in what’s called a “cold” state. The cloud provider then needs some time to initialize the code (to “warm” up the function). This can also happen when you need to scale up to handle more requests, as the code will need to be initialized again in other servers.
- Vendor Lock-In: Switching providers (either for cost-saving purposes or better performance) can be tricky, as everything needs to be migrated and code potentially changed to ensure its compatibility with the new provider. You also need to fully trust your provider and their security controls—a precaution you don’t need to take when hosting in-house.


##### When—and When Not—to Go Serverless

Choosing the right stack is a critical step when starting any project. Though it’s always possible to fine-tune and optimize things later, the more solid and stable your initial architecture is, the easier it will be for you to maintain it in the future. Let’s go over a few key factors to keep in mind when deciding on a tech stack—and more specifically, whether you should go for a serverless or traditional server:


- Project Requirements: This should usually be the most crucial factor in your decision—what is it you’re building? One of the main benefits of serverless is the many built-in services that come with it that save you from having to build those services yourself. Before deciding on a provider, ensure that its services satisfy the project requirements of your app or site. For example, if you need a mail server, the built-in mail service provided by AWS might be a good choice.
- Complexity: More complex apps can be challenging to build using serverless architecture. When building large apps with many dependencies, coordinating between the many functions can get (increasingly) complicated. Serverless tends to work better for smaller apps.
- Resources: How long will it take you to release your app? Getting to market as fast as possible is your highest priority if you’re a small startup. With traditional servers, you need to spend more time and people power configuring and deploying your app—something you can save time on by going serverless.
- Backend Programming Languages: Each provider supports different programming languages, some more than others. If you’re using JavaScript and Node.js, you’re in luck, as almost all cloud providers will support your app. However, if your team chooses PHP or Ruby, your choices will be considerably more limited.
- Security: Serverless providers protect the machine and keep the system updated; however, this requires a certain degree of trust on your end, as you’re essentially allowing them access to all of your data. If your data is highly sensitive or you need complete control over it, serverless may not be for you. For this reason, most government organizations don’t use cloud-based hosting services.
- Availability: When a traditional server crashes, you have to get it back up and running yourself. Otherwise, users won’t be able to access your app. With serverless, however, your uptime is guaranteed by your cloud provider. On the other hand, there’s still the serverless “cold start” issue to contend with. As the server needs to initialize your functions if they haven’t been used in a while, this can lead to longer response times, which isn’t good for user experience. If instant response time is essential for your app, serverless may not be the best choice.

This list covers most of the things you should keep in mind when deciding how to host your app or site, though you may find yourself running into different requirements and necessary specifications as you work on more projects in the future. For now, though, you’ll be focusing on serverless—not only because it’s vital that you understand serverless architecture but also because it’s quite suited to the Meet app you’ll be building throughout this Achievement. Working with serverless technologies has become important for web developers in the field, with this importance likely to keep rising. As such, you must become familiar with these technologies.


##### Serverless Platforms

Now that you have a basic understanding of FaaS hosting and the different types available, let’s take a quick look at some of the major providers. As mentioned earlier, the provider you choose will depend on your project’s needs, from the programming language used to the complexity and scope of your code. The three leading serverless providers are AWS Lambda, Microsoft Azure Functions, and Google Cloud Functions; while all three are pretty similar to each other, there are some differences between them that you’ll want to be aware of:


###### Figure 3.


#### Serverless Functions

A serverless function is no different than a normal function—it’s simply a piece of code that does something. How it differs, then, is through the architecture itself. Rather than having a server with multiple modules and functions inside each module, in serverless architecture, each function is deployed individually in the cloud. An app can have one or multiple serverless functions, with each one acting as a service that developers can call to perform an action.

![Serverless architecture](images-md/diagrams/snipcart-sless-architecture_1752706968683.png)


###### Figure 4. App architecture when using serverless functions. Source: Snipchart

The great thing about writing serverless functions is that you don’t need to care about (or even think about!) the underlying infrastructure behind your functions—all of that will be taken care of by your cloud provider.

Say, for example, that you want an email sent to your address whenever a user fills out the contact form on your website. To send an email, you’d need a server, as such an action would be impossible via the client-side. However, setting up an entire server just to send an email seems silly. This is where serverless functions can step in to save the day.

![The path of a request from a website user to a serverless function to an email provider](images-md/misc/cf-sless-function-email_1752706968927.jpg)


###### Figure 5.

Let’s look at an example serverless function for this scenario using AWS Lambda.


> Terminology: Lambda Functions
> As discussed, there are a wide variety of cloud-service providers, each allowing you to deploy your serverless functions on their platform with varying requirements and limitations. For your project, you’ll be writing serverless functions with AWS Lambda, a serverless computing service provided by Amazon Web Services. Throughout this Achievement, you’ll see reference to Lambda functions, as they’re the particular flavor of serverless functions you’ll learn to write. AWS Lambda is a relatively popular choice among developers for hosting serverless functions. Once you know how to create serverless functions and host them on Lambda, it’s a fairly simple task to switch to a different provider in your future projects, if necessary.


```js
const aws = require('aws-sdk');
const nodemailer = require('nodemailer');
const ses = new aws.SES();


exports.handler = (event) => {
 // obtain and parse request body data
 const requestData = JSON.parse(event.body);


 // prepare the email to send
 const mailOptions = {
   from: requestData.sender, // sender email address
   to: 'contact@example.com', // fixed receiver email address
   subject: requestData.emailSubject, // subject line
   text: requestData.emailTextBody, // plain text body  };


 // create a nodemailer transporter object, uses Amazon's Simple Email Service (SES)
 const transporter = nodemailer.createTransport({
   SES: ses
 });


 // send the email
 transporter.sendMail(mailOptions)
   .then(result => {
     console.log(result);
     return {
       statusCode: 200,
       body: "Email sent successfully",
     };
   })
   .catch(error => {
     console.error(error);
     return {
       statusCode: 500,
       body: "Failed to send email",
     };
   })


};
```

The first two lines are importing packages, just as you’ve done in earlier Achievements of this course. First, you import the relevant AWS Software Development Kit (SDK) since you’ll need access to the AWS Simple Email Service (SES) component to create your serverless Lambda function. Next, you import the Nodemailer package, a Node.js package for sending emails. Last but not least, you create and store an instance of the AWS SES.

With the packages you need imported, you then create and export a Lambda function handler that accepts an event as a parameter, which is equivalent to the many functions you created for your API endpoints in Achievement 2. Next, you need to define the content of the mail object that needs to be transported, which is stored in `mailOptions` and outlined in the inline code comments (as shown in the earlier code). You may be wondering how it works exactly, so here’s a quick rundown of the process:


- Whenever a request is made from your website, the request is usually made with an HTTP request method. Let's assume an HTTP POST request was made. The data that’s sent from the client along with this request is in the request body;
- When this HTTP POST request is made from the client, an event is activated to the Lambda function;
- The Lambda function is then called to handle the requested event; this event is passed to the Lambda function via its event parameter;
- Now, within the Lambda function, you can access the request body via the event parameter. The data itself is located in event.body, however, it needs to be parsed first.


> Note
> For the current email example, the request body—the information/data that comes along with the request from the client—is the sender’s email address as well as the email’s subject and body text.

Next, you create a mail transporter—using the `createTransport` method from Nodemailer—that accepts the configured AWS SES object. Finally, now that a transporter object exists, you can send an email with the subject and body to the desired address!

This is just one example of a serverless function, but there are countless other use cases, such as creating a new skill for Alexa, calling an audio-to-text transcription service, and much more. Let’s have a look at another. Here’s a simple example of a serverless function (`myFunction`) that responds with a 200 statusCode and a stringified JSON message in the object body:


```js
module.exports.myFunction = (event, context, callback) => {
   const response = {
       statusCode: 200,
       body: JSON.stringify({
           message: 'myFunction executed successfully',
       }),
   };
   console.log(context.memoryLimitInMB);
   callback(null, response);
};
```

`myFunction` accepts three arguments: event, context, and callback. Let’s take a closer look at each of these:

event: You’re familiar with event handling from working on your API endpoints in Achievement 2. However, when writing AWS Lambda functions, the Lambda function “handler” is the method that processes events. So, when `myFunction` is invoked, Lambda runs the handler method, which also occurred in the email-sending example just provided. You’ll learn more about the handler method as you create other functions in the next Exercise.

context:  When Lambda runs `myFunction`, a context object is passed to the handler. This object provides methods and properties that can be used to access information about `myFunction`, the invocation of `myFunction`, and the environment in which `myFunction` was executed. In the code just shown, `console.log(‘context.memoryLimitInMB’)` will log the amount of memory allocated to the created `myFunction` in the console. The developer can use this information to set memory limits for each function.

callback: The third argument, callback, is called in non-async handlers to send a response. In the example case here, when `myFunction` is invoked, the object stored in the response variable is returned, which is `statusCode: 200` and `message: 'myFunction executed successfully'`.

In summary, every time the serverless function `myFunction` is invoked, Lambda runs the handler method and returns a response. The returned response is the object stored in the response variable, which has two properties: the 200 statusCode and the body. To make sure the value returned by “body” is a JSON string, it’s wrapped in the `JSON.stringify()` method that converts an object to a string.

![Screenshot of successful execution of serverless function](images-md/misc/sless-myfunction_1752706969160.png)


###### Figure 6.

Great! So, now that you’ve looked at two serverless Lambda functions, let’s consider a few more use cases to understand how these functions are used in real-world scenarios.

1. Vending machine payments: Consider an airport vending machine and the logic behind the payment process. The client makes a selection by buying a drink. To verify the purchase, the vending machine makes a call to the payment gateway (e.g., Google Pay, Paypal, Stripe, etc.), which then makes a REST API call to the AWS API Gateway, prompting a Lambda function. AWS Lambda handles the business logic behind the transaction made by the client. Currently, users also can conduct payment transactions through a mobile device, with a push notification to their phone submitting the information to Google Pay, Samsung Pay, or Apple Pay. The advantage of serverless implementation in this vending machine situation is that all the communication is fast, and the company only pays for an actual request.

2. Chatbots: Chatbots imitate conversations with users in real-time, and serverless can be helpful here in several ways. Firstly, chatbot apps can have vast and sudden traffic due to how they're used. Serverless is suitable for these situations due to its ability to scale. Secondly, chatbots are helpful when client requests are very predictable. If a user inserts a question into a chatbot, the received question is processed using some complex algorithm to understand what the user is asking. The chatbot then decides the answer appropriate to the user’s question. This fundamental functionality means that there would be a lot of similar requests from various clients to be managed and responses that need to be automated. To do this, the backbone of a chatbot is an API, where messages are sent back and forth over the API. By configuring serverless to handle this service, developers do not need to spend much time going through the process of scaling and maintaining the hosting environment. The serverless framework will handle these tasks. Companies like Slack, Spotify, Lyft, MasterCard, and The Wall Street Journal, to name a few, are part of the growing number of enterprises that are using chatbots and serverless for these features.

3. Live streaming services: This is when audio or video data is shared in real-time to users via the internet. When the broadcaster conveys a live stream via a smartphone to viewers, the broadcaster sends an encoded and compressed version of the content to the Content Delivery Network (CDN) servers. The live stream viewer’s smartphone decodes the video and reconstructs the compressed data so that it can be viewed. The computing resources needed to process a large amount of multimedia processing data are enormous, and serverless is more efficient at handling live data streams because it provides scalability and load flexibility. Serverless architecture also brings forth the ability to collect audio and video streams from numerous sources, which are constructed and presented to viewers in one view. Companies such as Spotify, BBC, and SoundCloud, among others, make use of serverless frameworks.

So, with these examples in mind, developers use serverless functions such as Lambda to implement services within their apps. Let’s now look at your Meet app to see if there are any spots where serverless functions could help you out.


> Serverless Infrastructure Summary
> You may have noticed that serverless functions only handle the business logic layer of your app. To demonstrate this, let’s compare the serverless functions for your Meet app with your myFlix app’s infrastructure:
> 
> myFlix app
> 
> 
> Frontend: Written with JavaScript/React; hosted on Netlify.
> Backend (Server Logic): Written with Node/Express; hosted on Heroku.
> Backend (Database): MongoDB for users and movies; stored in MongoDB Cloud.
> 
> 
> Meet app
> 
> 
> Frontend: Written with JavaScript/React; hosted on Vercel.
> Backend (Server Logic): Written with Node/Express as Lambda functions (FaaS); hosted on AWS (requests come from frontend to Lambda function to data).
> Backend (Database): Google Calendar API.


##### Your Meet App’s Infrastructure: APIs and Authorization

Similar to the Pokédex app you built in Achievement 1, your Meet app will use an external API. This API will be provided by [Google Calendar](https://developers.google.com/calendar) and will be used for retrieving data about upcoming events so that you can render these details in your React app. To retrieve the event data, the client-side app needs to be authorized—this is where your serverless backend comes into play. Let’s break this down a little more, starting with the API.

APIs can be classified into two groups: public APIs and protected APIs. Anyone can access public APIs at any time without limitations. On the other hand, protected APIs can only be called by authenticated apps—in other words, apps that have a valid token issued by the API provider.

Nowadays, most APIs are protected, so a token is required for access. This is because API providers usually want to track the number of requests made by a particular app. Protecting an API also helps prevent attacks (a single app sending thousands—or even millions—of requests in a short period) and allows the provider of that API to have both paid and free versions if they so choose.

Back in [Exercise 2.9: Authentication and Authorization](https://careerfoundry.com/en/course/full-stack-immersion/exercise/security-authentication), you created a protected API by implementing both basic HTTP authentication and JWT/token-based authentication into your myFlix API—the latter only allowing some privileges to authenticated users. The Google Calendar API you will use in your Meet app is also one of these protected APIs. To access it, you’ll need a valid OAuth2 token.


> OAuth 1.0  vs. OAuth 2.0
> OAuth 2.0 is a more organized version of OAuth 1.0, with more advanced security.

If you need a refresher on OAuth, visit the section on [Open Authorization](https://careerfoundry.com/en/course/full-stack-immersion/exercise/security-authentication#open-authorization-oauth) in Exercise 2.9. Furthermore, take a look at the following diagram to see how these tokens work:

![The path a request takes from the user through the React app. The request gets an access token from the authorization server, then sends that token to the Google Calendar API to receive a list of events](images-md/misc/react-request-path-cf_1752706969363.png)


###### Figure 7.

In Scenario 1, the user (you, in this case) doesn’t have a valid token. When the app calls the API to get the list of upcoming events, the Google Calendar API returns an error (“Unauthorized”) rather than the expected list. You can try replicating this scenario right now in your browser if you want to! The following URL makes a request for events to the “calendars” endpoint of the Google API, along with an invalid token. Copy-paste the URL into the web search bar of your browser:


```js
https://www.googleapis.com/calendar/v3/calendars/ZnVsbHN0YWNrd2ViZGV2QGNhcmVlcmZvdW5kcnkuY29t/events
```

Did you get the following error response in the browser, telling you “Permission Denied”?

[![Error message from Google Calendar API](images-md/auth/permission-denied-auth_1752706969529.png)


###### Figure 8.

Now, take a look at Scenario 2 in Figure 7. Here, you have an OAuth authorization server that takes care of everything for you—the authorization server could be Twitter, Google, Facebook, or any other platform that provides OAuth services. It’s Google in this example. So, when a user provides their credentials to log in to their Google account, the authorization server will:


- Authenticate the user (ensuring they are who they say they are);
- Authorize the user to access the events via the React app.

When a user logs in via their Google account and grants consent to the app, a token is then granted to the app. The app can then use this token (on behalf of the user) to access protected resources, such as calendar events. So, when the user tries to view the list of events in the React app (i.e., when they send a request via the app to the API), the API will see and recognize the user’s token and is subsequently happy to provide the requested list of events.

Pay close attention to the roles in Scenario 2:


- The Protected Resource: In this case, the list of events the user wants to fetch from Google Calendar API.
- The Authentication Server: You’ll use Google OAuth for your app. This server is responsible for handing your app the access token that will be used to send requests to the Google Calendar API. Therefore, your app's users will have to log in to their Google accounts to use your app (i.e., to view events displayed in the app’s UI that have been fetched from the Google Calendar API).
- The Client App (React app in Figure 7): In this case, it’s your Meet App, which you’ll build using React.


> Note: Authentication Terms
> You may see interchangeable terms within the context of OAuth, such as "Authentication Server,” “OAuth Provider,” “OAuth Authentication Server,” “OAuth Platform,” and so on. All these terms refer to the same thing: the place where the OAuth provider can get you an access token, which you can then use to access protected content/resources.

In the next Exercise, you’ll replicate Scenario 2 for your Achievement 4 project. As for this Exercise’s task, you’ll focus on preparation work, such as configuring your Google Calendar’s OAuth screen. With your valid token in hand, if you call the Google Calendar API, you should receive a result like this:

![Result of calling Google Calendar API with a valid token](images-md/auth/valid-token-auth_1752706969719.png)


###### Figure 9.

Before moving on, there’s one more topic to discuss: scopes. Scopes are a way to control how authorized you want your app to be when accessing the protected resource. Scopes can be broadly categorized into three groups, based on how sensitive the data is that will be accessed via the scope:


- Non-sensitive scopes: These scopes only allow access to public data that you would normally be able to view on the internet (such as when you subscribe to someone’s public calendar)
- Sensitive scopes: These scopes grant you some personal/private data.
- Restricted scopes: These scopes are usually found in APIs such as Gmail, Google Drive, and so on. These resources may deal with highly sensitive information, so you must be extremely careful when dealing with restricted scopes. It takes a lot of effort to verify and publish apps that use such scopes due to such security and privacy needs.

![Scopes section of Google Calendar OAuth consent screen](images-md/ui/scopes-screen_1752706969954.png)


###### Figure 10.

Keep in mind that all scopes require user consent, even non-sensitive scopes, despite them being public.

For the Meet app, users only need to access public calendar events, which is a non-sensitive scope. Technically, this means all public calendar events, including the user’s own public events. As such, this scope also consents for the authorization server to access the user’s public calendar events. If the user grants consent, this defines the scope of their access.

You’ve now looked at how OAuth works for protected APIs, such as the one your Meet app will use. It’s time to consider how serverless technology can be used for this authorization process.


#### Using Serverless for Authorization

The logic for obtaining the access token from the authorization server can be placed in one of the following:


1. Your own backend server: For example, you could build a simple API that handles fetching the access token from Google OAuth.
1. The React Meet app itself: This is possible because Google provides a client-side JavaScript library that can fetch the access token from the Google OAuth authorization server.
1. A serverless function can be used to place the logic. This is how you’ll implement the logic for your app.

Serverless functions can be used in the aforementioned Scenario 2 for providing access tokens. Similar to the email example provided earlier, it doesn’t make sense to build and maintain an entire server just to provide access tokens (the first approach that was just listed). The second option is viable, but it’s best to practice using serverless functions at this stage. As such, you’ll be going with the third option. The cloud-service provider you’ll use to create your serverless functions is AWS Lambda.

So, how will this all fit together in your Meet app? To use the Google Calendar API with your app, you need three things:


- A consumer key (client_id);
- A consumer secret (client_secret);
- An access token.

The first two (consumer key and consumer secret) are used to identify the consumer that wants to use the OAuth (in this case, the serverless function that you’ll host on AWS Lambda). Think of them like a username (key) and password (secret). However, instead of authenticating a human, they will be used to authenticate the “OAuth consumer.”


> An “OAuth consumer” could be a server app (e.g., an API server hosted on Heroku), a client app (e.g., a desktop app, browser web app, mobile app), or a serverless function (as in your case). These options correspond, respectively, to the three options shared at the start of this section.

The consumer uses the key and secret to start the process of requesting the access token from the authorization server, which then lets the user know about this request (a consent screen appears). When the user grants consent (in this case by logging into their Google account and accepting to give consent) for the app to view calendar events, the app then fetches and displays them.

As a reminder of how this works, the authorization server sends back an authorization code that’s then used to get the access token. Your app can use this token to identify the user that has logged into their Google account to start using the app. This token is then sent along with every request the user makes via the app. When the API sees this token, it knows the user is who they say they are, and it sends the requested resources (e.g., the list of events if the request is to get Google Calendar's events through the API) back to the app. The authorization server’s role in the overall process is what your serverless architecture will interact with to gain authorization tokens (to make requests).

Check out this detailed diagram of Scenario 2 (shown originally in Figure 7), which walks through all the steps that you’ve just read. The steps are ordered chronologically from left to right:

![Detailed view of Oath process](images-md/misc/scenario-two-detailed_1752706970232.png)


###### Figure 11.


> Quick recap
> The process you’ve just been learning about is relatively complex, so here’s another quick recap. In your app, only authorized users can have access to view events data from the Google Calendar API. For a user to have such access, they must possess a “token.” This token is generated by the authorization server. The authorization server is where the serverless functions used for delivering authorization tokens to users will be hosted (i.e., AWS Lambda). The token generated by the authorization server will give the user the needed access to view event data from the Google Calendar API.
> 
> Real-World Example: CodePen
> 
> 
> 
> Figure 12. Source: Codepen
> 
> CodePen is a social development environment for frontend developers that uses serverless architecture. In order for users to be authorized to have access to show off created work, build test cases to learn and debug, and find inspiration, they are given the option to sign up with a social media account that uses open authorization (2.0, as mentioned earlier).
> 
> As you already learned, OAuth is an authentication and authorization framework that allows third-party apps (in this case, the social media platforms Twitter, GitHub, or Facebook) to authenticate clients without the need for clients to share credentials directly with the server. If users decide to sign up with a social media account using the OAuth framework, an access token or authorization code will be generated for each user. In this case, the user would be authorized to access the services provided by CodePen.
> 
> Just like CodePen, you’ll create a serverless function to generate an authorization token using the OAuth framework, which will determine that your app user is authorized to see the event data from the Google API.

You’ve now covered how your app will use serverless (i.e., to authorize users), but you’ll need to work through a few steps to get your environment set up before you can start writing your own Lambda functions. As such, for the remainder of the Exercise, you’ll be completing the following steps to prepare your serverless setup:


- Creating an OAuth consumer to use Google Calendar API.
- Setting up AWS Lambda with an authentication server.

Let’s go!


> Heads Up!
> The upcoming steps are quite specific, as you’ll walk through exactly what you need to do with the relevant tools to get set up for writing serverless functions in the next Exercise. So, feel free to take a tea break before diving in!
> 
> The instructions are based on using tools that may change or update their interfaces over time. As such, there may be instances where what you see in the Exercise doesn’t match exactly what you see on your screen. If that’s the case, you can always double-check with the tool’s official documentation to help you with any steps you’re unclear about or see if other users have encountered the same problem on your go-to online forums, such as Stack Exchange. If independent research still doesn’t resolve any roadblocks, please don’t hesitate to contact your tutor.


#### Creating an OAuth Consumer

The OAuth2 protocol allows every registered app to create its own OAuth consumer, which contains both a consumer key and a consumer secret. You’ll need to create your own OAuth consumer to use the Google Calendar API. In this Exercise, you’ll be connecting your brand-new app to the Google Calendar API so that you can get data about events to display in your app. You’ll use a Google calendar provided by CareerFoundry that will be populated with events.


> Tip!
> You’ll need a Google account to complete this Achievement. If you don’t have one, create a Google account now before moving on.


##### Step 1: Create a Project

First, you’ll need to create a project using [Google’s API console](https://console.developers.google.com/). Click the dropdown arrow Select a project, then select NEW PROJECT.

![Creating a new project in Google’s API console](images-md/misc/google-new-project_1752706970561.png)


###### Figure 13.

Name your new project “Meet App”s and leave Location as No organization. Once done, click CREATE.

![Location input option when creating a project](images-md/misc/proj-name-location_1752706970721.png)


###### Figure 14.

Make sure you’ve selected the project you’ve just created. The project name should be displayed in the top panel of the page next to "Google Cloud.” For example, if you created a project named “Demo Project,” and you have this project open or selected, you should see the following:

![Screenshot of project name’s location. “Demo Project” name showed at top of page](images-md/misc/proj-name-displayed_1752706970935.png)


###### Figure 15.

If this wasn’t your first project, it might not automatically be selected after you create it, so you’ll need to select it manually using the dropdown arrow next to the project name. Make sure you’re in the right project, as you don’t want to modify older projects by mistake.

You should now see an API & Services dashboard (Figure 16). If it doesn’t load automatically, in your notification about your newly created project, click Select Project.

![API & Services dashboard screen](images-md/misc/api-services-dash_1752706971169.png)


###### Figure 16.


##### Step 2: Enable Google Calendar API

Next, click +ENABLE APIS AND SERVICES.

![Screenshot showing +Enable APIs and Services link](images-md/misc/google-enable-apis_1752706971409.png)


###### Figure 17.


> If you don't see this button, then you’re likely not in the "Enabled APIs & Services" page. You can navigate to this page from the menu on the left-hand side.

This is where you need to find and enable the Google Calendar API. Search for “Google Calendar API” and choose it from the results, then enable it on the next screen, where you’ll see something like this:

![Google Calendar API enable popup](images-md/misc/enable-popup_1752706971571.png)


###### Figure 18.

Once enabled, you’ll be routed to the Google Calendar API dashboard (to the "API/Service Details" page).


##### Step 3: Set Up Your Credentials

Next, you need to set up your credentials. To do so, click on CREATE CREDENTIALS in the upper-right corner.

![Screenshot showing Create Credentials button](images-md/misc/create-creds_1752706971722.png)


###### Figure 19.


##### Step 4: Credential Questions

From here, you’ll be asked several questions about the API being used and data. Your answers should be “Google Calendar API” and “User Data” (Figure 20). Then, click NEXT (not DONE, be careful).

![Credential Type screen, with several questions to be answered](images-md/misc/google-create-creds-type_1752706971980.png)


###### Figure 20.


> Note
> If you mistakenly clicked on DONE instead of NEXT, you can get back on track by clicking “Enabled APIs & Services” in the menu on the left. You’ll be able to find the CREATE CREDENTIALS button (in the upper-right corner) and can then resume the process of setting up your credentials. This advice applies to all upcoming steps for creating the OAuth consumer.


##### Step 5: OAuth Consent Screen

You’ll now be directed to the OAuth Consent Screen section. This section contains information about your app, which will be displayed to users as a consent screen.This information will tell your users more about—and how to contact—the developer (see Figure 21).

![Screenshot of OAuth Consent Screen](images-md/auth/oauth-consent-screen_1752706972182.png)


###### Figure 21.

In the OAuth Consent Screen section, enter the following information:


- App name: Meet App
- User support email: Your Gmail address
- App logo: You don’t need to add a logo, but you can if you wish. The logo will be shown on the Google Authentication page. You may be required to verify your app using your logo.
- Developer contact information: Again, use your Gmail address.

Now, click on the SAVE AND CONTINUE button to proceed. The next screen, Scopes, is where you can specify what user data you’d like to access (or change) once the user has authorized your app (in other words, when you successfully get the authentication token).

Click the ADD OR REMOVE SCOPES button to view the list of scopes, then enable the “.../auth/calendar.events.public.readonly” scope (it might not be on the first page). You can also manually add the scope by entering “[https://www.googleapis.com/auth/calendar.events.public.readonly”](https://www.googleapis.com/auth/calendar.events.public.readonly%E2%80%9D) in the input field that’s located in the “Manually add scopes” section, then click ADD TO TABLE. Next, hit the  UPDATE button.

![Screenshot of scopes list, including the “Manually add scopes” area](images-md/misc/manually-add-scopes_1752706972355.png)


###### Figure 22.

As you learned earlier in this Exercise, the scope is what the user needs to consent to so that the app can access calendar events.

Next, click SAVE AND CONTINUE to move to the next screen.


##### Step 6: OAuth Client ID

You should now be on the OAuth Client ID section of your credentials set up. Enter the required information as follows:


- Application type: Web application
- Name: Meet App
- Authorized JavaScript origins: Your newly hosted app’s domain from the task in Exercise 4.1: Test-Driven Development & Test Scenarios (https://your-vercel-generated-url.vercel.app). Don't forget to replace "your-vercel-generated-url" with the name of the generated Vercel app from the previous task.
- Authorized redirect URIs: Your app’s URI (https://your-vercel-generated-url.vercel.app/). Again, double-check that you’ve replaced "your-vercel-generated-url" with the name of the generated Vercel app from the previous task.

![OAuth Client ID section](images-md/auth/oauth-client_1752706972656.png)


###### Figure 23.

Once finished, click CREATE (not the DONE button) at the bottom of the screen. You’ll then receive information about how to download your credentials. Click the DOWNLOAD link, and then click DONE. Remember that this downloaded file will contain more than just the “Client ID” and the “Client Secret”—you’ll need this extra data later.

![Download credentials information screen](images-md/auth/oauth-download-creds_1752706972925.png)


###### Figure 24.


> Accessing Your Credentials
> If you don’t download your credentials, you can still access your “Client ID” and “Client Secret” on the Credentials page. There, you can download your credentials by clicking the down arrow on the right-hand side (Figure 25).
> 
> 
> 
> Figure 25.


##### Step 7: Add Test Users

You’ll also need to add the test users that will be allowed to use your app. Under the APIs & Services sidebar, click OAuth consent screen, then click +ADD USERS in the Test users section at the bottom of this section. Enter at least one Gmail account address. This can be the Gmail account address that you want to use for testing in production mode or the Gmail account of any other test users of your Meet app. Make sure to also add your tutor and mentor’s Gmail addresses as test users, as they’ll be reviewing your tasks and will need to have access to the app.

![Consent screen with test users section](images-md/auth/oauth-add-test-users_1752706973120.png)


###### Figure 26.

Once done, click SAVE, then check to ensure the user has been added to the list. This list appears as a table in the Test users section. As you can see in the example (Figure 27), the test email “[curriculum@careerfoundry.com](mailto:curriculum@careerfoundry.com)” is now listed in the table:

![Test user information table](images-md/auth/oauth-test-users-table_1752706973613.png)


###### Figure 27.

Congratulations! You’re now set up to use the Google Calendar API. This means you have a key and secret you can use to identify your Meet app. But what about the authorization code used to identify users on your app? Remember, the authorization code is generated after users are granted access to the API by way of the authorization server, so this will come in just a bit.

The last step is to set up your authorization server, which is what will receive your consumer key (`client_id`), consumer secret (`client_secret`), and, in response, will return your authorization code. For your Meet app, you’ll use AWS Lambda as your serverless provider to do this. Let’s take a look!


> Tip!
> The authorization code that’s returned can only be used to get your access token once. Once used, it can’t be used again.


#### Setting Up AWS Lambda

As a developer, you have two different options for setting up your functions in Lambda, mostly depending on whether you need to install your own libraries. These include:


- Using the Serverless Toolkit: If you need to install any libraries for your functions to work, you should choose this option. The Serverless Toolkit is a handy and widely adopted toolkit for building serverless apps. If you need to install any libraries for your functions to work, you should choose this option. With the Serverless Toolkit, you can create a function, test it locally, and deploy it using the command line.
- Using AWS Lambda’s inline editor: If you don’t need to install any libraries and prefer to work directly in the AWS console, you can choose this option. While more basic than using the Serverless Toolkit, it’s convenient for viewing console logs, testing functions using test events, editing environment variables, and deploying functions via the Amazon API Gateway. s

You’ll be using the Serverless Toolkit for your project throughout this Achievement. As this toolkit isn’t limited to AWS Lambda, you’ll be able to take the knowledge you learn here and use it to deploy your serverless functions to any of the other cloud-based providers in the future.

Let’s walk through creating an authorization server using the Serverless Toolkit. You can follow along with one of our instructors in the following video or use the steps provided afterward.


##### Step 1: Install Serverless

First, install serverless globally using npm via the following command in your terminal:


```js
npm install -g serverless # -g flag stands for globally
```


##### Step 2: Install AWS CLI

Install AWS CLI on macOS

You can install the AWS CLI using one of the following methods:

Method 1: Homebrew (Recommended)

If you have Homebrew installed, this is the easiest way to install the AWS CLI.
Open your terminal. Run the following command to install the AWS CLI:
`brew install awscli`

Once installed, verify the installation:

`aws --version`

This should return the installed version of the AWS CLI.

Method 2: Using the AWS Installation Script

If you don’t use Homebrew, you can install the AWS CLI using the installation script provided by AWS.

Run the following command to download and install the AWS CLI v2:

`curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"`

Install the downloaded package:

`sudo installer -pkg AWSCLIV2.pkg -target /`

After installation, verify the installation:
`aws --version`

This should output the version of AWS CLI installed.

Install AWS CLI on Windows

Method 1: Using the AWS CLI MSI Installer (Recommended)

Download the AWS CLI Installer: Go to the [AWS CLI v2 download page](https://aws.amazon.com/cli/) and download the MSI installer for Windows.

[](images-md/misc/figure28-2_1752706974116.png)


###### Figure 28. (Click to Zoom.)

Run the installer: After downloading the `.msi` file, double-click it to run the installer. Follow the on-screen instructions to complete the installation.

Verify the installation: After the installation is complete, open Command Prompt and run:

`aws --version`

If the installation was successful, this command will display the installed AWS CLI version.

Method 2: Using Windows Package Manager (optional)

If you have the Windows Package Manager (`winget`) installed, you can use the following command to install AWS CLI:

`winget install awscli`

After the installation is complete, open Command Prompt and run:

`aws --version`

If the installation was successful, this command will display the installed AWS CLI version.


##### Step 3: Create a Serverless Service

Next, you’ll create a directory for your auth-server. Still in your terminal—and within the root directory of your Meet app—type the following:


```js
# Create a new directory named auth-server
mkdir auth-server
# Jump into the newly created directory
cd auth-server
# Then create the following files:
touch serverless.yml
touch handler.js
touch .gitignore

If you’re using **Windows PowerShell**, type: `new-item` instead of `touch`


# Finally, create a package.json.
npm init -y
```

Once you’ve completed this step, check the structure of your new “auth-server” directory, keeping in mind that this directory should be inside the “meet” root directory. It should look like this:


```js
auth-server
├── .gitignore
├── handler.js
├── package.json
└── serverless.yml
```


##### Step 4: Configuring Your AWS Credentials

With your auth-server boilerplate created, all you need to do now is add your AWS credentials. Without this step, the toolkit won’t be able to interact with AWS on your behalf, which completely negates the point! To provide these credentials, you’ll first need to obtain them from the [AWS Management Console](https://aws.amazon.com/console/).

Log in to the AWS account you created in Exercise 4.1’s task, then click on your username in the upper right-hand corner. This should open a dropdown menu. Select Security credentials from the menu:

![An AWS dropdown menu with the security credentials option](images-md/misc/aws-sec-creds_1752706973929.png)


###### Figure 29.

From there, select Access keys (0). The first thing you’ll want to do here is to create an access key. This access key will allow you to configure your Serverless Toolkit, authorizing it to deploy functions directly to AWS. Click on the large blue Create Access Key button:

[](images-md/misc/figure30-2_1752706974337.png)


###### Figure 30. (Click to Zoom)

This access key will contain your new access key ID and secret access key. Make sure you click on Download.csv file and store it in a safe place. Careful, if you don’t download and store it now, you won't be able to retrieve your secret access key again later!

Next, head back to your terminal, where you can configure your new AWS credentials for Serverless with the following command 
`:aws configure`
This will prompt you to enter the following details:
AWS Access Key ID: Get this from your downloaded file.
AWS Secret Access Key: Also available from your downloaded file.
Default region name: E.g., `us-east-1`. (Or you can leave it empty)
Default output format: You can choose `json`, `text`, or `table`. `json` is the most commonly used. (Or you can leave it empty)
Verify configuration: You can check if the configuration is correct by listing your configured settings with:

`aws configure list`


> Reminder
> Make sure you replace Access Key Id and Secret Access Key with your own credentials!

Well done! Once you’ve set up your credentials for the first time, you won’t need to do so again.


#### Summary

You’ve covered quite a lot in this Exercise, from an overview of cloud computing and some of the different cloud providers (and their services) to some real-world use cases of serverless development in an app infrastructure context. Each of these topics will prove important in your future career as a web developer.

You also learned how to set up your environment to prepare for writing serverless functions, which you’ll explore in the next Exercise. In particular, you learned how to create a Google OAuth consumer and set up AWS Lambda so that you’re ready to code your authorization server. Don’t worry if you don’t feel completely comfortable with serverless architecture yet—you’ve still got the rest of this Achievement to play around with serverless functions!

Before that, it’s time to apply what you’ve learned in this Exercise to your project. It’s time to get your app’s infrastructure set up for serverless development!


#### Resources

If you’re curious to read more about the topics covered in this Exercise, then we recommend taking a look at the following resources. Note that this reading is optional and not required to complete the course.


- Google API Console
- Google Search Console
- A Serverless Function Example: Why and How to Get Started
- AWS: Serverless Documentation
- What is Serverless?
- Serverless Framework Documentation
- OAuth Scopes for Google APIs
- OAuth API Verification FAQs

Take the quiz to test your knowledge on this Exercise.


#### Task


- Direction
- Submission History

In this task, you’ll set up the development tools and infrastructure needed to successfully write and run serverless functions (which you’ll cover in the next Exercise). You’ll also do some of your own research to learn more about how serverless technology is useful for real-world products.

Directions


1. Create a new document (e.g., Word or Google document).
1. Do some research online to find 1 example of serverless functions being used in a real-world product, and:


Explain how this product uses serverless technology
Explain why serverless technology is useful in the context of this app.
1. In that same document, explain how you’ll use serverless functions (a few sentences).
1. Bonus: Using Figures 4, 5, and 7 as a guideline, draw an architectural diagram of your Meet app, based on what you understand about serverless architecture and the requirements of your project from your project brief. Add this diagram to your README in GitHub.
1. Create an OAuth consumer for your Meet app (follow the instructions in the Exercise).
1. Take a screenshot of your Google OAuth credentials as evidence that you successfully completed step 5.
1. Set up AWS Lambda by obtaining an access key.
1. Integrate your access key into your app via the terminal.
1. Take a screenshot of your terminal as evidence that you successfully configured AWS Lambda.
1. Add the screenshot evidence of your OAuth consumer and AWS configuration to your document from step 1.
1. Save the document with your research findings, screenshots, and explanation of serverless functions as a PDF file and upload it here for your tutor to review.

