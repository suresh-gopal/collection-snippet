## Spring AI Integration Example Application
This is an example of Spring Boot Application integrated with Spring AI - OpenAI.

### Adding OpenAI token
OpenAI API key is needed which is obtained from https://platform.openai.com/ and is defined in `application.yaml`.

### Running the application
To run the app first built it using maven:

```
mvn clean install
```
And then run `SpringAIDemoApplication` as regular Spring Boot app.

### Accessing REST endpoints
When the application starts, it can be interacted with using REST endpoints defined in `OpenAIController`. the app starts at `localhost:8080`.
Swagger is enabled and can be accessed http://localhost:8080/swagger-ui/index.html

#### Simple REST Template Text prompts
To ask questions we can call `/v1/openai/resttemplate/chat` providing your prompt, such as:

```
http://localhost:8080/v1/openai/resttemplate/chat?Ask A Question=Hello
```

#### Spring AI Integration prompts
Refer https://spring.io/projects/spring-ai The Chat Completion type can also be utilized by using ChatClient prompt 
method.Please note that 429 org.springframework.ai.retry.NonTransientAiException error indicating insufficient quota 
might be displayed if the Open API profile is a free trial. 

```
http://localhost:8080/v1/openai/springai/chat?message=Hello
```
