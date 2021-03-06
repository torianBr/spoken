# Text-to-Speech and Speech-to-Text for Artificial Intelligence and Chatbot Apps

> `npm i spoken` # https://www.npmjs.com/package/spoken

Spoken is a **free SDK** for voice controlled apps.
Improve your user's exerience with easy to use _Human Interface_.

 - Zero GUI app.
 - Hands-free app interface.
 - Help blind and low-vision users gain independence.

### App Ideas

You can imagine many new possible apps using this SDK.

 - Writing documents with your voice.
 - Driver safe hands-free mode.
 - Record meeting notes.
 - IoT Voice Control Commands.
 - Voice controlled games.

![Voice Controlled Apps](https://i.imgur.com/tXJmwrN.gif)


### Example Usage Pattern

You can prompt the user with a synthetic voice.
And then you can speak the answer and capture the voice transcript.
With a few lines of code, you can have a hands-free voice app.

```javascript
spoken.say('Should I turn the hallway light on?').then( speech => {
    spoken.listen().then( transcript =>
        console.log("Answer: " + transcript) )
} )
```

### Code Playground

> **Playground URL:** https://stephenlb.github.io/spoken/

You can test the code right now in your Chrome browser.
Click the playground link above to try it out.

[![Text-to-Speech and Speech-to-Text](https://i.imgur.com/75tQtoZ.png)](https://stephenlb.github.io/spoken/)

### Turn Speech into Text and Text into Speech

Compatible on Android / iOS / Linux / Windows / MacOS.

**Spoken** is a Google Chrome Voice App SDK.
This SDK allows you to easily call Voice APIs to turn Text to Speech and Speech to Text.
The library has no dependencies.
This SDK is only for Google Chrome apps on Mobile and Web.

Compatible with Electron (desktop), Ionic, PhoneGap/Cordova, React, Angular, etc.
This works on Mobile Chrome.

Currently there is support for Google Chrome.
And if you are looking to deploy as a mobile app, this **should work** 👌.

### Text-to-Speech

Synthetic voices are pretty good these days.
You can still tell they are robot voices.

Turn text into real-world spoken voice.
The voice is synthetic.
You can pick from a few different voices too.

```javascript
// Hello World
spoken.say('Hello World.');

// Say a quick message as Guy Fieri
spoken.say( 'Looks like your on a trip to flavor town.', 'Daniel' );

// Speak with Damayanti's voice
spoken.say( 'I am princess of the Vidarbha Kingdom.', 'Damayanti' );
```

#### Promises and Async/Await

This SDK Supports modern async programming.

```javascript
// Promise
spoken.say('Hello World.').then( v => console.log('Done talking.') );

// Async/Await
await spoken.say('Hello World.');
console.log('Done talking.');
```

### Text-to-Speech Voice Library

```javascript
// List of voices supported on platform
console.log( await spoken.voices() );

// List of voices with promise callback
spoken.voices().then( voices => console.log(voices) );
```

Get list of **English** voices.

```javascript
// List English Speaking Voices
(await spoken.voices()).filter( v => v.lang.indexOf('en') == 0 );
```

Sample the list of English voices.

```javascript
// Speak with each English voice to sample your favorites
(await spoken.voices())
    .filter( voice => voice.lang.indexOf('en') == 0 )
    .map( voice => voice.name )
    .forEach( voice =>
        spoken.say( 'Welcome to flavor town.', voice ).then(
            speech => console.log(speech.voice.name)
        )
    );
```

### Speech-to-Text

> Must use HTTPS to access microphone.

You need an HTTPS (TLS) File Server. To start a local secure file server:

```shell
python <(curl -L https://goo.gl/LiW3lp)
```

Then open your browser and point it to your file in
the directory you ran the python HTTPS server.

```shell
open https://0.0.0.0:4443/index.html
```

> This is a Simple Python HTTPS Secure Server
> https://gist.github.com/stephenlb/2e19d98039469b9d0134

We posted an answer on
[StackOverflow WebRTC HTTPS](http://stackoverflow.com/a/41969170/524733).
This will get you started testing on your laptop.

Turn your spoken words into text using the `listen()` method.
You will want to also take advantage of the real-time speech
detection using the `spoken.listen.on.partial()` event.
This allows you to display the current transcription before
your utterance is finished.

The following will allow you to capture the final transcription
which can be used to send over to a chatbot API.

```javascript
// Async/Await
console.log(await spoken.listen());

// or Promise style
spoken.listen()
    .then( transcript => console.log(transcript)     )
    .catch(     error => console.warn(error.message) )
```

There can only be one concurrent listener.
You can catch for this.

```javascript
// We can only have one concurrent listener.
// So you can catch if there is an error.
spoken.listen()
    .then( transcript => console.log(transcript)     )
    .catch(     error => console.warn(error.message) )
```

Capture live "real-time" transcription as you speak.

```javascript
spoken.listen.on.partial( ts => console.log(ts) );
spoken.listen()
    .then(     ts => console.log("Partial: " + ts) )
    .catch( error => console.warn(error.message)   )
```

Additional voice transcription events.

```javascript
spoken.listen.on.start( voice => { console.log('Started Listening') } );
spoken.listen.on.end(   voice => { console.log('Ended Listening')   } );
spoken.listen.on.error( voice => { console.log('Error Listening')   } );
```

Stop listening.

```javascript
spoken.listen.stop();
```

### Continuous Listening Example

Writing a document with your voice or chatting with your friend hands-free
is possible when you enable `continuous` listening mode.

```javascript
spoken.listen.on.end(continueCapture);
spoken.listen.on.error(continueCapture);

startCapture();

function startCapture() { 
    spoken.listen({ continuous : true }).then( transcript =>
        console.log("Captured transcript: " + transcript)
    ).catch( e => true );
}

async function continueCapture() {
    await spoken.delay(300);
    if (spoken.recognition.continuous) startCapture();
}

function stopCapture() {
    spoken.recognition.continuous = false;
    spoken.listen.stop();
}
```


