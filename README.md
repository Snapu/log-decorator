# Typescript Decorator for Efficient and Effective Logging

(See [Implementation](https://github.com/Snapu/workout-companion/blob/master/src/services/logger.ts))

In the recent years Web Apps became more and more populer and with the influence of Domain Driven Design and the [Micro Frontends](https://micro-frontends.org/) movement more and more functionality is shifted towards the frontend. Leading to an increased need of cloud logging in the frontend. E.g. in my own [private project](https://github.com/Snapu/workout-companion) I log to the Google Cloud from my frontend and my backend is very little to none existent.

Here, I will not talk about how to technically log to Google Cloud or any other logging platform but how to *enforce* efficient and effective logging with a little typescript decorator.

So when is a logging **efficient** and **effective**?

My opinion is: logging should be **minimal** as possible and **maximal** as needed, **structured** and follow a certain **standard**. Then, it is very comfortable to analyse the logs.

I think with a little typescript decorator you can enforce this very well:

```
class Car {

    @Log('parking car', { logArgs: true /* default is false for security reasons */ })
    public async park(direction: Direction) {
        // code for parking the car
    }
}
```

Calling `car.park(Direction.BACKWARDS)` would produce the following json log:

```
{
    "severity": "INFO",
    "jsonPayload": {
        "userId": "fca13c5a-0a44-4ed7-8fca-38e5eff04fcf",
        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.101 Safari/537.36",
        "language": "de-DE",
        "version": "1.0.0",
        "message": "parking car",
        "method": "Car.park",
        "args": "BACKWARDS",
        "duration" "12345"
    }
}
```

As you see it adds a lot of context to the log:
* a persistent random `userId` to distiguish logs from different users,
* browser details and which version of the app,
* the actual log message,
* the method where the log was initiated,
* and how long the method took to resolve. Isn't that cool?

In case of an error, it would also add the `name`, `message` and `stack` of the error and automatically change the severity to `WARN`.

So why do I think it enforces efficient and effective logging?

Firstly, every log has a certain structure with very useful context data. But more importantly, it makes you think about how to structure your entire code and enforces to apply the Separation of Concerns principle (SoC). Just think about the following: Don't you normaly just log whenever a **step** started, successfully finished or failed? Why not then implement each step as a seperate method and apply the decorator for automatic success and error logging? This process of thinking makes you to decide which step is worthful to seperate and properly log (good case **and** bad case) leading to a more balanced and complete logging.

What do you think? Could this lead to a better logging and cleaner code as well? Do you experience the same problems with logging?

*typos will be fixed soon and eventually the implementation currently living in [my app](https://github.com/Snapu/workout-companion/blob/master/src/services/logger.ts) will be extracted to a seperate library*
