# Harmony Link - General Concepts

Harmony Link is a middleware software which is focusing around building believable characters. This is done by
providing an interfaces and a processing model which allows for independent AI systems to work as one - for a single
AI character, or as many as you like (if your Hardware and Infrastructure supports it). 

Effectively, this allows these systems to act as one, multi-modal system per character configuration, which can be used
to control and "represent" the character it's set up to form.

This part of the documentation aims at giving you a top-level overview of how this works.

So Let's assume you have an Application or Game that you want to extend with AI features, like conversational AI NPCs which
should also perform some movement animation and facial expression simulation based on the conversation.
The typical flow here would probably be, that you would need the following:

1. An LLM Backend like ChatGPT, plus custom code for integrating their backend.
2. A TTS Backend like Elevenlabs, plus custom code for integrating their backend.
3. Your own conversational management handling code in your application.
4. Lots of trial-and-error iterations while integrating the above.
5. Ingame Animations for your characters.

***However, with Harmony Link you only need the following:***

1. A Harmony Link Plugin or SDK that you can use to integrate Harmony Link
2. Code for creating and handling received Harmony Link Events.
3. Ingame Animations for your characters.

Harmony Link already today supports multiple backend integrations for LLMs and TTS, and much more are going to follow soon.
Check out our [Modules documentation](Modules.md) for more details on which modules are available.

If you're curious to get started integrating AI into your software using Harmony Link now, please check out the already
existing Plugins at our [available Plugins list](Plugins.md).

Also, in case your programming language or engine is not supported yet, it's still easy to integrate Harmony Link's events
API yourself. Check our [API Reference](Events-API.md) for more details.

---
&copy; 2023-2024 Harmony AI Solutions & Contributors