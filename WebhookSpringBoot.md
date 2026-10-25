

# Java Spring Boot Webhook Example

Minimal [Spring Boot](http://projects.spring.io/spring-boot/) sample app.

## Requirements

For building and running the application you need:

- [JDK 21](https://www.oracle.com/java/technologies/javase/jdk17-readme-downloads.html)
- [Maven 3](https://maven.apache.org)

## WebHook Basics

Webhooks are used to communicate the occurrence of an event in one system to another system and they often share data about the event.

Webhooks and APIs enable communication between systems in different ways.
Webhooks notify a system that an action (event) has occurred. They are a way to send data about an event to another system in real-time.
APIs expose a system's functionality via a set of endpoints, which can be used to perform actions or retrieve data.
Webhook are most common in SaaS and PaaS platforms like GitHub, Shopify, Stripe, Twilio because they support different types of events based on the activities that happen within them.

Webhook.site generates free, unique URLs and e-mail addresses and lets you see everything that’s sent there instantly.
GitHub Developer Settings can be updated to add WebHook Producer and providing webhook site URL will ensure events trigger will initiate a POST call.

```shell
Example: https://webhook.site/61af10f8-ee2d-4bc1-b4fc-79497cbb4c37
```

## Testing the Webhook in Non Prod environments

Use a HTTP API client like Postman or Insomnia and simply make direct HTTP POST requests to your localhost development endpoint, essentially “faking” a real webhook request.
1. Generate a real webhook request. Example, configure GitHub to setup WebHook. Refer; https://docs.github.com/en/webhooks/using-webhooks/creating-webhooks
2. Receive that webhook request in WebHook.site.
3. Replicate that body and headers with your HTTP Client. Reference payload available in WebHookControllerTest.
4. Common methods for webhook signature verification include HMAC with SHA-256 and digital signatures using EC or RSA keys. 





