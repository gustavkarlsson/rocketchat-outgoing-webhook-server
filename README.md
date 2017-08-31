[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/gustavkarlsson/rocketchat-gson-models/blob/master/LICENSE)
[![](https://jitpack.io/v/gustavkarlsson/rocketchat-outgoing-webhook-server.svg)](https://jitpack.io/#gustavkarlsson/rocketchat-outgoing-webhook-server)

# rocketchat-outgoing-webhook-server
A tiny framework for creating [Rocket.Chat](https://rocket.chat) outgoing webhook servers in Java.

## Download
Point your favorite build tool to [jitpack](https://jitpack.io/#gustavkarlsson/rocketchat-outgoing-webhook-server)

## Usage
This framework is built on the very simple [Spark](http://sparkjava.com/) web framework.

To create your a server, just extend `RocketChatMessageRoute` and implement the `handle` method.
Then fire up spark with your route and you're good to go!

## Example app
Here is an example of how to create a simple echo service

```java
import se.gustavkarlsson.rocketchat.models.from_rocket_chat.FromRocketChatMessage;
import se.gustavkarlsson.rocketchat.models.to_rocket_chat.ToRocketChatMessage;
import se.gustavkarlsson.rocketchat.spark.routes.RocketChatMessageRoute;
import spark.Request;
import spark.Response;
import spark.Spark;

public class App {
    public static void main(String[] args) {
        Spark.port(8080);
        Spark.post("/", "application/json", new MyRocketChatRoute());
    }

    private static class MyRocketChatRoute extends RocketChatMessageRoute {
        @Override
        protected ToRocketChatMessage handle(Request request, Response response, FromRocketChatMessage fromRocketChatMessage) throws Exception {
            String text = fromRocketChatMessage.getText();
            ToRocketChatMessage responseMessage = new ToRocketChatMessage();
            responseMessage.setText(text);
            return responseMessage;
        }
    }
}

```
