# Harmony Link - Backend Modules Overview

Harmony Link currently supports at least one backend module for each of the following tasks:

[Large Language Model (LLM)](#large-language-model-llm-module): For conversational AI.

[Text-To-Speech (TTS)](#text-to-speech-tts-module): For vocalizing written text. Integrates with LLM Module by default.

[Speech-to-Text (STT)](#speech-to-text-stt-module): For text comprehension. Integrates with LLM Module by default.

[Countenance Module](#countenance-module): For detecting and predicting moods and facial expressions in conversations. Integrates with LLM Module by default.


## Large Language Model (LLM) Module
Our LLM Backend Module - also just called backend module - Is the "brain" of each AI character. It's used for processing
conversation and nonverbal interaction, and create verbal and non-verbal responses. Every action, communication and
interaction between entities is internally translated to text by Harmony Link, and passed to the LLM backend.
The results then will be shared amongst all other modules (if linked) for further processing.

Currently, we're supporting the following LLM modules:

### Kajiwoto
https://kajiwoto.ai/

Kajiwoto is a long time development partner of Project Harmony.AI and offers an incredible amount of features for creating
realistic AI characters, including:

- Multiple AI LLM Backends, including ChatGPT, Llama 2, Pygmalion, as well as Custom Models.
- Character based Prompting and Personality definitions
- Consistend long-term and short term memory for characters
- Custom Dataset creation

If you want to use Kajiwoto for your Harmony Link characters, just follow our
[Quickstart Guide for Kajiwoto](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Module-Settings-Backend#kajiwoto).

### Character AI
https://beta.character.ai/

Character AI (or just "cai"), is a rather new website for creating AI powered characters, which however got massively
popular in a very short amount of time, due to it's simplicity and due to big names from Google behind it.
It's not as flexible and detailed as Kajiwoto when it comes to character tweaking, also NSFW content is strongly filtered,
however it offers a solid solution for creating realistic AI characters for conversation.

*ATTENTION: Character AI support is currently experimental.*

[Quickstart Guide for Character AI](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Module-Settings-Backend#character-ai).

### Kindroid
https://landing.kindroid.ai/

Kindroid is an app for creating AI companions. It's overall premise is quite similar to Kajiwoto, however their model quality
and development team is significantly larger and they have more development resources and powerful AI hardware at their hands,
allowing them to provide advanced features like video calls, image generation and animated in-app avatars, yet their
app doesn't feature as much customization features as Kajiwoto at the moment.

*ATTENTION: Kindroid AI support is currently experimental and not many features are provided by their official API currently.*

[Quickstart Guide for Character AI](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Module-Settings-Backend#kindroid-ai).

### OpenAI
https://platform.openai.com/

OpenAI has become the go-to company when it comes to generative AI technology since their release of
ChatGPT to the world in November 22 at the latest. Their AI Backends offer state-of-the-art performance
and quality, and therefore we also integrated some of their services as a backend option into Harmony Link.

[Quickstart Guide for OpenAI's ChatGPT / GPT-4](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Module-Settings-Backend#openai).

### Oobabooga's text-generation-webui
(Original Repo: https://github.com/oobabooga/text-generation-webui)

Harmony.AI Version (required to work with Harmony Link): https://github.com/harmony-ai-solutions/text-generation-webui-harmony-ai

The text-generation-webui is a de-facto open source standard for running any kind of LLM locally.
Using it for realtime conversations with characters requires a powerful machine, otherwise conversation might be very 
laggy.

To work with Harmony Link's conversation History feature, we had to do some minor adaptions to the code. Also we're using
Oobabooga as a backend solution for our Emotions API (Countenance module). 

To use it for your Harmony Link characters, just follow our
[Quickstart Guide for Textgen](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Module-Settings-Backend#text-generation-webui-only-for-advances-users).


## Text-To-Speech (TTS) Module
The TTS module can be used to vocalize Texts using AI speech. By default it integrates with the LLM module and vocalizes
the verbal part of the communication with an AI character controlled by Harmony Link.

Currently, we're supporting the following TTS modules:

### Harmony Speech V1
Harmony Speech is our own custom TTS voice engine. It was designed to deliver good results in One-Shot Voice Cloning, and
is capable of handling voice generation for hundreds of characters in parallel and faster than real time.

It is already being used by Kajiwoto to power TTS for their AI characters at this point. Therefore, all AI voices created
for Kajiwoto are by default compatible with Harmony Link as well.

By default, Harmony Link uses Harmony Speech for TTS. To properly configure it and check out voices on Kajiwoto as well, check
out our [Quickstart Guide for Harmony Speech](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Quickstart#tts-using-harmony-speech).

### Elevenlabs
Elevenlabs is a very well known Provider for realistic and reliable TTS solutions. Their Bandwidth is usually quite limited
in the free tiers, but therefore their generated voices sound extremely realistic and authentic.

If you want to use Elevenlabs for your Harmony Link characters, just follow our
[Quickstart Guide for Elevenlabs](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Quickstart#tts-using-elevenlabs).

## Speech-to-Text (STT) Module
The STT module can be used to convert spoken speech into text. By default it integrates with the LLM module and allows
AI controlled Characters to listen to user or other character's speech.

### Harmony Speech V1
Harmony Speech is our own custom TTS voice engine.
It has a STT-Engine integrated which utilizes a self-hosted Whisper Server under the hood.

By default, Harmony Link uses Harmony Speech for STT. To properly configure it and check out voices on Kajiwoto as well, check
out our [Quickstart Guide for Harmony Speech](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Quickstart#stt-using-harmony-speech).

### OpenAI

OpenAI has become the go-to company when it comes to generative AI technology since their release of
ChatGPT to the world in November 22 at the latest. Their AI Backends offer state-of-the-art performance
and quality, and therefore we also integrated some of their services as a backend option into Harmony Link.

[Quickstart Guide for OpenAI's Whisper](https://project-harmony.youtrack.cloud/articles/HARMONY-A-7/Quickstart#stt-using-openai-whisper).

## Countenance Module
The Countenance module is responsible for simulating a characters facial expressions and an overall stance in context of
what's currently happening. It is planned to directly interface with future modules for AI movement and (nonverbal) Interaction
with Human users or other AI.

Currently we only support Countenance via our own Backend; however we're constantly investigating whether there are
third-party solutions which might work as alternative and could be integrated in the future.

---
&copy; 2023-2024 Harmony AI Solutions & Contributors