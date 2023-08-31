# Harmony Link - Events API

This document describes the Events API of Harmony Link, which needs to be used if you want to use Harmony Link to extend
your Software with AI capabilites. The Events API has been designed with the goal to have Harmony Link perform actions
with as little effort as possible, so developers can focus on their product, and not bother with the specifics of AI
system integration.

We plan on releasing multiple SDKs for different Programming languages and use cases (e.g. Unreal / Unity Plugin) over
time, to reduce the integration efforts even more. In case you need inspiration, feel free to also check out the
[list of available Harmony Link plugins](Plugins.md).

Also, if you have a custom implementation for Harmony Link which could be beneficial for others, you're also happily invited
to share it with the Harmony.AI community-

## Events API - Basics

### Endpoints
Harmony Link currently offers both a Websocket and a HTTP Endpoint for Event processing.

#### WebSocket
WebSocket Support two-way-communication between Harmony Link and a client with one handler per connection. The recommended
way to use the protocol is to process one character per WebSocket connection. However, it is theoretically also possible to
handle multiple characters in a single connection. Yet this requires significant process management and Event ID handling
overhead on the Plugin side. 

#### HTTP
The HTTP Endpoint is considered legacy, since it's slightly less performant than the WebSocket Communication. Increased 
connection latency between client and Harmony Link further increases this downside. However, there might be use cases where
WebSocket communication is not possible due to network or library limitations. In this cases HTTP is usually a possible
alternative.

Using the HTTP Endpoint requires an async handshake between Plugin and Harmony Link, where Harmony Link assigns a
`Harmony-Session-Id` to the client, which needs to be included in any subsequent request as a header, so Harmony link can
match individual sessions. Also, it is required for the plugin to manage it's own HTTP Handler / Server, and each 
request must provide the Header Value `Harmony-Result-Port`, so Harmony Link knows on which Port the Plugin is currently
listening.

### Events
Harmony Link's Events API uses a single Data type: `HarmonyLinkEvent`

It consists of the following JSON-like data structure:

````json
{
  "event_id": "",   // string
  "event_type": "", // string
  "status": "",     // string
  "payload": []     // bytearray
}
````
This structure might appear simple, but it allows Harmony Link to basically handle all types of requests.

#### event_id (mandatory)
`event_id` is an arbitrary string which is used to identify an event. It is especially useful if you have a multiple 
character/entity scenario, or parallel processing per entity, where you need to keep track of results for individual
processing tasks. It needs to be at least 1 char long.

#### event_type (mandatory)
`event_type` defines the type of event, which will affect how Harmony Link will treat the received event.
Not all events may be implemented to be received; some are just handled internally, or sent to the client by Harmony Link.
The full list of Events can be seen in the expandable section below:

<details>
<summary>List of Harmony Link Event types</summary>

#### Current Version
	// Backend Event Types
	INIT_CHARACTER // Basic character information; triggers setup on Harmony Link side
	AI_STATUS      // State update used for stance, mood, face expression etc.
	AI_UTTERANCE   // Something the AI says and/or does (can contain both speech and action)
	AI_SPEECH      // Something the AI says
	AI_ACTION      // Something the AI does
	USER_UTTERANCE // Something the User says and/or does (can contain both speech and action)

	// Countenance Event Types
	AI_COUNTENANCE_UPDATE // Request Countenance update based on conversation context

	// STT Event Types
	STT_INPUT_AUDIO  // Takes binary audio data and transcribes it using the transcribe backend
	STT_START_LISTEN // Start recording on default microphone 
	STT_STOP_LISTEN  // Stop recording on default microphone

	// TTS Event Types
	TTS_PLAYBACK_DONE // Inform Harmony Link that Playback is done (cleans audio file from cache)

#### Deprecated Event types
- None

</details>

#### status (mandatory)
`status` is used to control the lifecycle of events. There's 4 possible event status types:
````json
NEW
PENDING
SUCCESS
ERROR
````
New Events always need to be sent with status `NEW`. Harmony Link may be returning a copy of the Event in an intermediate
state with status `PENDING`, if config value `ConfirmEvents` is set to true.

Once processing is done or failed, Harmony Link will send a Result Event with status of either `SUCCESS` or `ERROR`.

#### payload (optional)
`payload` is an arbitrary data field, which may be empty or consisting binary or JSON data, depending on the event type.


### Example Events
In the following, we provide a few common example events and expected Results from Harmony Link, to help with Implementation
efforts.

Plugin -> Harmony Link:

<details>
<summary>INIT_CHARACTER</summary>

Request
````json
{
  "event_id": "init_char0",
  "event_type": "INIT_CHARACTER",
  "status": "NEW",
  "payload": {
    "character_id": "char0"
  }
}
````
Result (Async)
````json
{
  "event_id": "init_char0",
  "event_type": "INIT_CHARACTER",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>STT_START_LISTEN</summary>

Request
````json
{
  "event_id": "start_listen",
  "event_type": "STT_START_LISTEN",
  "status": "NEW",
  "payload": null
}
````
Result (Async)
````json
{
  "event_id": "start_listen",
  "event_type": "STT_START_LISTEN",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>STT_STOP_LISTEN</summary>

Request
````json
{
  "event_id": "stop_listen",
  "event_type": "STT_STOP_LISTEN",
  "status": "NEW",
  "payload": null
}
````
Result (Async)
````json
{
  "event_id": "stop_listen",
  "event_type": "STT_STOP_LISTEN",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>STT_INPUT_AUDIO</summary>

Request
````json
{
  "event_id": "audio_data_1234",
  "event_type": "STT_INPUT_AUDIO",
  "status": "NEW",
  "payload": {
    "AudioBytes": [] // bytearray
  }
}
````
Result (Async)
````json
{
  "event_id": "audio_data_1234",
  "event_type": "STT_INPUT_AUDIO",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>USER_UTTERANCE (Human Speech as text)</summary>

Request
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "NEW",
  "payload": {
    "type": "UTTERANCE_VERBAL",
    "content": "I really like being outside touching grass"
  }
}
````
Result (Async)
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>USER_UTTERANCE (Combined Human Speech + Nonverbal Interaction)</summary>

Request
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "NEW",
  "payload": {
    "type": "UTTERANCE_COMBINED",
    "content": "*smiles* Hi, how are you?"
  }
}
````
Result (Async)
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>USER_UTTERANCE (Nonverbal Interaction)</summary>

Request
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "NEW",
  "payload": {
    "type": "UTTERANCE_NONVERBAL",
    "content": "hugs you lovingly and caresses your back"
  }
}
````
Result (Async)
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>USER_UTTERANCE (Delayed Nonverbal Interaction - Will be processed / sent to LLM together with next Audio Transcript)</summary>

Request
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "NEW",
  "payload": {
    "type": "UTTERANCE_NONVERBAL_DELAYED",
    "content": "hugs you lovingly and caresses your back"
  }
}
````
Result (Async)
````json
{
  "event_id": "utterance_1234",
  "event_type": "USER_UTTERANCE",
  "status": "DONE",
  "payload": null
}
````
</details>

<details>
<summary>TTS_PLAYBACK_DONE (Tells Harmony Link to delete cached file)</summary>

Request
````json
{
  "event_id": "playback_123",
  "event_type": "TTS_PLAYBACK_DONE",
  "status": "NEW",
  "payload": "C:\\harmony-tmp\\tts\\audio-123.wav"
}
````
Result (Async)
````json
{
  "event_id": "playback_123",
  "event_type": "TTS_PLAYBACK_DONE",
  "status": "DONE",
  "payload": null
}
````
</details>

Harmony Link -> Plugin:

<details>
<summary>AI_ACTION (Nonverbal Action or Interaction)</summary>

Event by Harmony Link
````json
{
  "event_id": "char0_action_20230901-182301.234",
  "event_type": "AI_ACTION",
  "status": "DONE",
  "payload": {
    "type": "UTTERANCE_NONVERBAL",
    "content": "sits down at the table"
  }
}
````

</details>

<details>
<summary>AI_SPEECH (Configured for Output File)</summary>

Event by Harmony Link
````json
{
  "event_id": "char0_speech_20230901-182301.234",
  "event_type": "AI_SPEECH",
  "status": "DONE",
  "payload": {
    "type": "UTTERANCE_VERBAL",
    "content": "I really liked that movie, it's amazing!",
    "audio_file": "C:\\harmony-tmp\\tts\\audio-123.wav",
    "audio_type": "wav"
  }
}
````
</details>

<details>
<summary>AI_SPEECH (Configured for Binary Data)</summary>

Event by Harmony Link
````json
{
  "event_id": "char0_speech_20230901-182301.234",
  "event_type": "AI_SPEECH",
  "status": "DONE",
  "payload": {
    "type": "UTTERANCE_VERBAL",
    "content": "I really liked that movie, it's amazing!",
    "audio": "BYTESTRING", // Base64 encoded
    "audio_type": "wav"
  }
}
````
</details>

<details>
<summary>AI_COUNTENANCE_UPDATE</summary>

Event by Harmony Link
````json
{
  "event_id": "char0_emotions_20230901-182301.234",
  "event_type": "AI_COUNTENANCE_UPDATE",
  "status": "DONE",
  "payload": {
    "emotional_state": "happy",
    "facial_expression": "happy_smile"
  }
}
````
</details>

---
&copy; 2023 Harmony AI Solutions & Contributors