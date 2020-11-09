---
title: Chat
layout: docs
menu: docs
weight: 4
---

# Twitch Chat (WebSocket)

Twitch offers an IRC interface to our chat functionality. This allows you to, for instance:

* Develop bots for your channel.
* Connect to a channel’s chat with an IRC client instead of using the Web interface.

While our IRC server generally follows RFC1459, there are several cases where it behaves slightly differently than other IRC servers; as described below, there are many Twitch-specific IRC capabilities. The differences are necessary to accommodate:

* The massive scale at which our chat servers operate, and
* Complex Twitch-specific features that we provide (to viewers, broadcasters, and developers).

Rate-Limiting

* The library follows the official rate-limits and has a queue for a maximum of 200 messages.
* After that old messages will be removed from the queue in favor of more recent messages.

## Methods

* [JoinChannel](./join-channel)
* [LeaveChannel](./leave-channel)
* [SendMessage](./send-message)
* [SendPrivateMessage](./send-private-message)

## Use as part of Twitch4J

To use Twitch Chat and events from chat, you need to specify withEnableChat when building the Twitch4J Instance, as shown below:
{{< codeblocks >}}
{{< code Java >}}

```java
// chat credential
OAuth2Credential credential = new OAuth2Credential("twitch", "oAuthTokenHere");

// twitch client
TwitchClient twitchClient = TwitchClientBuilder.builder()
            ...
            .withEnableChat(true)
            .withChatAccount(oAuth2CredentialHere)
            ...
            .build();
```

{{</ code >}}
{{< code Groovy >}}

```groovy
// chat credential
def credential = new OAuth2Credential("twitch", "oAuthTokenHere")

// twitch client
def twitchClient = TwitchClientBuilder.builder()
            ...
            .withEnableChat(true)
            .withChatAccount(oAuth2CredentialHere)
            ...
            .build()
```

{{</ code >}}
{{< code Kotlin >}}

```kotlin
// chat credential
val credential = OAuth2Credential("twitch", "oAuthTokenHere")

// twitch client
val twitchClient = TwitchClientBuilder.builder().apply {
                ...
                withEnableChat(true)
                withChatAccount(oAuth2CredentialHere)
                ...
            }.build()
```

{{</ code >}}
{{</ codeblocks >}}
The first value of new OAuth2Credential is the identity provider, and in the case of twitch4j always `twitch`.
You can pass in your oauth token as 2nd value, if you don't have one you can generate one [here](https://twitchtokengenerator.com/).

## Use Standalone

Initialize the Chat Feature as Standalone Module, also requires setting up the EventManager and the CredentialManger yourself:

{{< codeblocks >}}
{{< code Java >}}

```java
// event manager
EventManager eventManager = new EventManager();

// credential manager
CredentialManager credentialManager = CredentialManagerBuilder.builder().build();
credentialManager.registerIdentityProvider(new TwitchIdentityProvider("jzkbprff40iqj646a697cyrvl0zt2m6", "**SECRET**", ""));

// twitch4j - chat
TwitchChat client = TwitchChatBuilder.builder()
        .withEventManager(eventManager)
        .withCredentialManager(credentialManager)
        .withChatAccount(oAuth2CredentialHere)
        .build();
```

{{</ code >}}
{{< code Groovy >}}

```groovy
// event manager
def eventManager = new EventManager();

// credential manager
def credentialManager = CredentialManagerBuilder.builder().build()
credentialManager.registerIdentityProvider(new TwitchIdentityProvider("jzkbprff40iqj646a697cyrvl0zt2m6", "**SECRET**", ""))

// twitch4j - chat
def client = TwitchChatBuilder.builder()
        .withEventManager(eventManager)
        .withCredentialManager(credentialManager)
        .withChatAccount(oAuth2CredentialHere)
        .build()
```

{{</ code >}}
{{< code Kotlin >}}

```kotlin
// event manager
val eventManager = EventManager();

// credential manager
val credentialManager = CredentialManagerBuilder.builder().build().also {
    it.registerIdentityProvider(TwitchIdentityProvider("jzkbprff40iqj646a697cyrvl0zt2m6", "**SECRET**", ""))
}

// twitch4j - chat
val client = TwitchChatBuilder.builder().apply {
            withEventManager(eventManager)
            withCredentialManager(credentialManager)
            withChatAccount(oAuth2CredentialHere)
        }.build()
```

{{</ code >}}
{{</ codeblocks >}}

The UserId is required, since it will be used to get the oauth credentials
