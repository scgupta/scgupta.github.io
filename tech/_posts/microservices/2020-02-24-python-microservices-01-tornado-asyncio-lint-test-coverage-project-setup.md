---
title: "How to build, test and profile microservices in Python: choices, key concepts and project setup"
excerpt: "Tornado, with AsyncIO APIs, is an efficient framework for building IO-bound Python microservices. Learn key concepts. Set up lint, test, code coverage."
image: "https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s1600/py-microservice-tornado-title-image.png"
image_thumbnail: "https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s320/py-microservice-tornado-title-image.png"
tags:
  - "microservices"
  - "python"
  - "tornado"
code: true
---

*Distilled lessons from building microservices powering Slang Labs platform.*

![](https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s1600/py-microservice-tornado-title-image.png){: width="100%" class="framedimg"}

At SlangLabs, we are building a platform for *[programmers](https://slanglabs.in/developers.html){:target="_blank" rel="nofollow"}* to easily and quickly add multilingual, multimodal *[Voice Augmented eXperiences (VAX)](https://medium.com/slanglabs/what-is-voice-augmented-experience-1003a28b6e5){:target="_blank" rel="nofollow"}* to their mobile and web apps. Think of an assistant like Alexa or Siri, but *[running inside your app and tailored for your app](https://www.youtube.com/watch?v=TMhGGIAc0f0){:target="_blank" rel="nofollow"}*.

The platform consists of:

- **Console** to configure a *[buddy](https://docs.slanglabs.in/slang/digging-deeper/building-buddies){:target="_blank" rel="nofollow"}* for an app,
- **SDKs** for *[Android](https://docs.slanglabs.in/slang/getting-started/for-android-apps){:target="_blank" rel="nofollow"}* and *[Web (JavaScript)](https://docs.slanglabs.in/slang/getting-started/for-web-apps){:target="_blank" rel="nofollow"}* providing voice-to-action capabilities,
- **Microservices** that SDKs invoke to infer the *[intent](https://docs.slanglabs.in/slang/digging-deeper/building-buddies/intents){:target="_blank" rel="nofollow"}* inherent in the voice utterance of an end-user, and extract associated *[entities](https://docs.slanglabs.in/slang/digging-deeper/building-buddies/entities-and-entity-types){:target="_blank" rel="nofollow"}*, and
- **Analytics** to analyze [end-user behaviour](https://docs.slanglabs.in/slang/digging-deeper/understanding-analytics){:target="_blank" rel="nofollow"} and improve the experience.

This series of blog posts is to share the best practices and lessons we have learned while building the microservices.

---

DRAFT
