---
title: "Speech Recognition with Python: Comparing 9 most prominent alternatives"
excerpt: "Which Speech Recognition suits your transcriber: Google Speech-to-Text, Amazon Transcribe, Microsoft Azure Speech, Watson, Nuance, CMU Sphinx, Kaldi, DeepSpeech, Facebook wav2letter?"
image: "https://1.bp.blogspot.com/-lTzrc8o_iCw/Xkie0L8_ueI/AAAAAAAATEU/Cplxbh-4RwgHc54eT6tkFxaMbtdgB_tegCKgBGAsYHg/s1600/speech-recognition-with-python-snake.png"
image_thumbnail: "https://1.bp.blogspot.com/-lTzrc8o_iCw/Xkie0L8_ueI/AAAAAAAATEU/Cplxbh-4RwgHc54eT6tkFxaMbtdgB_tegCKgBGAsYHg/s320/speech-recognition-with-python-snake.png"
tags:
  - "machine-learning"
  - "speech-recognition"
  - "python"
canonical_url: "https://www.slanglabs.in/blog/automatic-speech-recognition-in-python-programs"
code: true
---

![](https://1.bp.blogspot.com/-lTzrc8o_iCw/Xkie0L8_ueI/AAAAAAAATEU/Cplxbh-4RwgHc54eT6tkFxaMbtdgB_tegCKgBGAsYHg/s1600/speech-recognition-with-python-snake.png){: width="100%" class="framedimg"} 

Speech recognition technologies have been evolving rapidly for the last couple of years, and are transitioning from the realm of science to engineering.
With the growing popularity of voice assistants like Alexa, Siri and Google Assistant, several apps (e.g., [YouTube](https://medium.com/slanglabs/voice-in-apps-youtube-25bcc288ac4c){:target="_blank"}, [Gana](https://medium.com/slanglabs/voice-in-apps-gaana-1f6e2d8b026b){:target="_blank"}, [Paytm Travel](https://medium.com/slanglabs/voice-in-apps-paytm-travel-5bee6aea76dc){:target="_blank"}, [My Jio](https://medium.com/slanglabs/voice-in-apps-my-jio-5dc8f2e298d){:target="_blank"}) are beginning to have functionalities controlled by voice. At Slang Labs, we are building a platform for programmers to easily [augment existing apps with voice experiences](https://medium.com/slanglabs/what-is-voice-augmented-experience-1003a28b6e5){:target="_blank"}.

Automated Speech Recognition (ASR) is the necessary first step in processing voice. In ASR, an audio file or speech spoken to a microphone is processed and converted to text, therefore it is also known as Speech-to-Text (STT). Then this text is fed to a Natural Language Processing/Understanding (NLP/NLU) to understand and extract key information (such as intentions, sentiments), and then appropriate action is taken. There are also stand-alone applications of ASR, e.g. transcribing dictation, or producing real-time subtitles for videos.

We are interested in ASR and NLU in general, and their efficacy in the voice-to-action loop in apps in particular. Our Android and Web SDKs provide simple APIs suitable from the perspective of app programmers, while Slang platform handles the burden of the complexity of stitching together ASR, NLU and Text-to-Speech (TTS). But, naturally, we are curious about the state of art in ASR, NLU and TTS even though we do not expose these parts of our tech stack as separate SaaS offerings. This exploration of existing ASR solutions is the result of that curiosity.

## Service vs. Software

There are two possibilities: make calls to Speech-to-Text SaaS on the cloud or host one of the ASR software package in your application.

**Service** is the easiest way to start. You sigh-up for a SaaS, get key/credentials, and you are all set to use it in your code, either through HTTP endpoints or libraries in the programming languages of your choice. However, for reasonable large usage, it typically cost more money.

**Software** packages offer you full control as you are hosting it, and also the possibility of creating smaller models tailored for your application, and deploying it on-device/edge without needing network connectivity. But it requires expertise and upfront efforts to train and deploy the models.

![](https://3.bp.blogspot.com/-C9CX13JCM4U/Xkifra78mKI/AAAAAAAATEs/b8w9Ixz4PRcnnX2rSh6zNpZeiKRoaQq6gCKgBGAsYHg/s1600/asr-service-vs-software.png){: class="framedimg"}

It is a reversible choice. For example, you can start with a cloud service, and if needed, move to your own deployment of a software package; and vice versa. You can design your code to limit the blast-radius of such reversal, as well as in case if you migrate to another SaaS or software package.

## Batch vs. Streaming

You need to determine whether your application requires batch ASR or streaming ASR.

**Batch:** If you have audio recordings that need to transcribe it offline, then batch processing will suffice as well more economical. In batch API, an audio file is passed as a parameter, and speech-to-text transcribing is done in one shot.

**Streaming:** If you need to process speech in realtime (e.g. in voice-controlled applications, video subtitles), you will need streaming API. In the case of streaming API, it is repeatedly invoked with available chunks of the audio buffer. It may send interim results, but the final result is available at the end.

All services and software packages have batch APIs, but some lack streaming APIs at the moment. So if you have a streaming application, that eliminates some of the choices.

## Choice of Python

Most speech services provide libraries in popular programming languages. In the worst case, you can always use HTTP endpoints. Same is true for speech packages, these come with bindings in various programming languages. In the worst case, you can create bindings yourself. So there is no constraint of using Python.

> I am choosing Python for this article because most speech cloud services and ASR software packages have Python libraries. Also, you can run code snippets of the article using its companion [Colab notebook](https://colab.research.google.com/github/scgupta/yearn2learn/blob/master/speech/asr/python_speech_recognition_notebook.ipynb){:target="_blank"} in the browser, without requiring anything to be installed on your computer.

One common use case is to collect audio from microphone and pass on the buffer (batch or streaming) to the speech recognition API. Invariably, in such transcribers, the microphone is accessed though PyAudio, which is implemented over PortAudio. But since the microphone is not accessible on Colab, we simplify it. We will use a complete audio file to examine batch API. And for streaming API, we will break an audio file into chunks and simulate stream.

## How to best use the rest of the article

Following services and software packages are covered.

**Services:**

- Google Speech-to-Text
- Microsoft Azure Speech
- IBM Watson Speech to Test
- Amazon Transcribe
- Nuance

**Software:**

- CMU Sphinx
- Mozilla DeepSpeech
- Kaldi
- Facebook wav2letter

Code samples are not provided for Amazon Transcribe, Nuance, Kaldi, and Facebook wav2letter due to some peculiarity or limitation (listed in their respective sections). Instead, links to code samples and resources are given.

The next section has the common utility functions and test cases. The last section covers Python `SpeechRecognition` package that provides an abstraction over batch API of several could services and software packages.

If you want to have an overview of all services and software packages, then please open the Colab, and execute the code as you read this post. If you are interested only in a specific service or package, directly jump to that section. But in either case, do play with the code in Colab to explore it better.

Let's plunge into the code.

---

## Common Setup

Download some audio files needed for testing Speech Recognition services and software packages:

~~~ bash
$ curl -LO https://github.com/mozilla/DeepSpeech/releases/download/v0.6.0/audio-0.6.0.tar.gz

$ tar -xvzf audio-0.6.0.tar.gz

$ ls -l ./audio/
~~~

It has three audio files. Define test cases with needed metadata:

~~~ python
TESTCASES = [
  {
    'filename': 'audio/2830-3980-0043.wav',
    'text': 'experience proves this',
    'encoding': 'LINEAR16',
    'lang': 'en-US'
  },
  {
    'filename': 'audio/4507-16021-0012.wav',
    'text': 'why should one halt on the way',
    'encoding': 'LINEAR16',
    'lang': 'en-US'
  },
  {
    'filename': 'audio/8455-210777-0068.wav',
    'text': 'your power is sufficient i said',
    'encoding': 'LINEAR16',
    'lang': 'en-US'
  }
]
~~~

Also, write some utility functions. The `read_wav_file()` takes the path to the audio file, and returns the buffer bytes and sample rate:

~~~ python
def read_wav_file(filename) -> Tuple[bytes, int]:
    with wave.open(filename, 'rb') as w:
        rate = w.getframerate()
        frames = w.getnframes()
        buffer = w.readframes(frames)

    return buffer, rate
~~~

The `simulate_stream()` is useful for simulating steam to try streaming APIs. Usually, there will be an audio source like a microphone. At regular intervals, the microphone will generate a speech chunk, which has to be passed to the streaming API. The `simulate_stream()` function helps to avoid all that complexity and to focus on the APIs. It takes an audio buffer and batch size, and *generates* chunks of that size. Notice the `yield buf` statement in the following:

~~~ python
def simulate_stream(buffer: bytes, batch_size: int = 4096):
    buffer_len = len(buffer)
    offset = 0
    while offset < buffer_len:
        end_offset = offset + batch_size
        buf = buffer[offset:end_offset]
        yield buf
        offset = end_offset
~~~

---

## Google Speech-to-Text Cloud Service

![](https://1.bp.blogspot.com/-yGaN4SLPvlg/XkilwaO9gEI/AAAAAAAATFA/o8FGrVJjiDoeGEqf2N7Oi2bYcCTWYuO6QCKgBGAsYHg/s1600/Google-Cloud-Speech.png){: width="25%" class="framedimg alignleft"}

Google has [speech-to-text](https://cloud.google.com/speech-to-text/docs){:target="_blank" rel="nofollow"} as one of the Google Cloud services. It has [libraries](https://cloud.google.com/speech-to-text/docs/reference/libraries){:target="_blank" rel="nofollow"} in C#, Go, Java, JavaScript, PHP, Python, and Ruby. It supports both batch and stream modes. Please open the [Colab](http://bit.ly/colab-speech-recognition-python){:target="_blank"}, and execute the code as you read this post. And also play with the code.

### Setup

You will need your [Google Cloud Credentials](https://developers.google.com/accounts/docs/application-default-credentials){:target="_blank" rel="nofollow"}. You will need to setup `GOOGLE_APPLICATION_CREDENTIALS` environment variable pointing to the cred file:

~~~ bash
$ export GOOGLE_APPLICATION_CREDENTIALS='/path/to/google/cloud/cred/file/gc-creds.json'
$ ls -l $GOOGLE_APPLICATION_CREDENTIALS
~~~

### Batch API

Using batch speech-to-text-API is straight forward. You need to create a `SpeechClient`, create a `config` with audio metadata and call `recognize()` method of the speech client.

~~~ python
from google.cloud import speech_v1
from google.cloud.speech_v1 import enums

def google_batch_stt(filename: str, lang: str, encoding: str) -> str:
    buffer, rate = read_wav_file(filename)
    client = speech_v1.SpeechClient()

    config = {
        'language_code': lang,
        'sample_rate_hertz': rate,
        'encoding': enums.RecognitionConfig.AudioEncoding[encoding]
    }

    audio = {
        'content': buffer
    }

    response = client.recognize(config, audio)
    # For bigger audio file, the previous line can be replaced with following:
    # operation = client.long_running_recognize(config, audio)
    # response = operation.result()

    for result in response.results:
        # First alternative is the most probable result
        alternative = result.alternatives[0]
        return alternative.transcript

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('google-cloud-batch-stt: "{}"'.format(
        google_batch_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

When you run this, you will see the text of each of audio test files in the output:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
google-cloud-batch-stt: "experience proves this"

audio file="audio/4507-16021-0012.wav"    expected text="why should one halt on the way"
google-cloud-batch-stt: "why should one halt on the way"

audio file="audio/8455-210777-0068.wav"    expected text="your power is sufficient i said"
google-cloud-batch-stt: "your power is sufficient I said"
~~~

### Streaming API

Google's streaming API is also quite simple. For processing audio stream, you can repeatedly call the streaming API with the available chunk of audio, and it will return you interim resuls:

~~~ python
from google.cloud import speech
from google.cloud.speech import enums
from google.cloud.speech import types

def response_stream_processor(responses):
    print('interim results: ')

    transcript = ''
    num_chars_printed = 0
    for response in responses:
        if not response.results:
            continue

        result = response.results[0]
        if not result.alternatives:
            continue

        transcript = result.alternatives[0].transcript
        print('{0}final: {1}'.format(
            '' if result.is_final else 'not ',
            transcript
        ))

    return transcript

def google_streaming_stt(filename: str, lang: str, encoding: str) -> str:
    buffer, rate = read_wav_file(filename)

    client = speech.SpeechClient()

    config = types.RecognitionConfig(
        encoding=enums.RecognitionConfig.AudioEncoding[encoding],
        sample_rate_hertz=rate,
        language_code=lang
    )

    streaming_config = types.StreamingRecognitionConfig(
        config=config,
        interim_results=True
    )

    audio_generator = simulate_stream(buffer)  # buffer chunk generator
    requests = (types.StreamingRecognizeRequest(audio_content=chunk) for chunk in audio_generator)
    responses = client.streaming_recognize(streaming_config, requests)
    # Now, put the transcription responses to use.
    return response_stream_processor(responses)

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('google-cloud-streaming-stt: "{}"'.format(
        google_streaming_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

In the output, you can see that the results improves as more audio is fed:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
interim results: 
not final: next
not final: iSpy
not final: Aspira
not final: Xperia
not final: Experian
not final: experience
not final: experience proved
not final: experience proves
not final: experience proves the
not final: experience proves that
not final: experience
final: experience proves this
google-cloud-streaming-stt: "experience proves this"
~~~

---

## Microsoft Azure Speech Cloud Service

![](https://1.bp.blogspot.com/-O3wB1GGbMro/XkimgPKvsfI/AAAAAAAATFI/j3cMevYvJ2YzIbbeLZd6CCfHOF0EVLcuACKgBGAsYHg/s1600/Microsoft-Azure-Speech-to-Text.png){: width="25%" class="framedimg alignleft"}

Microsoft Azure [Speech Services](https://azure.microsoft.com/en-in/services/cognitive-services/speech-services/){:target="_blank" rel="nofollow"} have [Speech to Text](https://azure.microsoft.com/en-in/services/cognitive-services/speech-to-text/){:target="_blank" rel="nofollow"} service.

### Setup

Install Azure speech package:

~~~ bash
$ pip3 install azure-cognitiveservices-speech
~~~

You can enable Speech service and find credentials for your account at [Microsoft Azure portal](https://portal.azure.com/){:target="_blank" rel="nofollow"}. You can open a free account [here](https://azure.microsoft.com/en-in/free/ai/){:target="_blank" rel="nofollow"}. Service credentials:

~~~ python
AZURE_SPEECH_KEY = 'YOUR AZURE SPEECH KEY'
AZURE_SERVICE_REGION = 'YOUR AZURE SERVICE REGION'
~~~

### Batch API

Azure's batch API is simple too. It takes a config and audio input, and returns the text:

~~~ python
import azure.cognitiveservices.speech as speechsdk

def azure_batch_stt(filename: str, lang: str, encoding: str) -> str:
    speech_config = speechsdk.SpeechConfig(
        subscription=AZURE_SPEECH_KEY,
        region=AZURE_SERVICE_REGION
    )
    audio_input = speechsdk.AudioConfig(filename=filename)
    speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config,
        audio_config=audio_input
    )
    result = speech_recognizer.recognize_once()

    return result.text if result.reason == speechsdk.ResultReason.RecognizedSpeech else None

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('azure-batch-stt: "{}"'.format(
        azure_batch_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

The output looks as following:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
azure-batch-stt: "Experience proves this."

audio file="audio/4507-16021-0012.wav"    expected text="why should one halt on the way"
azure-batch-stt: "Whi should one halt on the way."

audio file="audio/8455-210777-0068.wav"    expected text="your power is sufficient i said"
azure-batch-stt: "Your power is sufficient I said."
~~~

### Streaming API

Azure has several kinds of streaming API. By creating different types of audio sources, one can either push the audio chunks, or give a call back to Azure to pull the audio chunk. It fires several types of speech recognition events to hookup callbacks. Here is how you can wire a push audio stream with the audio stream generator:

~~~ python
import time
import azure.cognitiveservices.speech as speechsdk

def azure_streaming_stt(filename: str, lang: str, encoding: str) -> str:
    speech_config = speechsdk.SpeechConfig(
        subscription=AZURE_SPEECH_KEY,
        region=AZURE_SERVICE_REGION
    )
    stream = speechsdk.audio.PushAudioInputStream()
    audio_config = speechsdk.audio.AudioConfig(stream=stream)
    speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config,
        audio_config=audio_config
    )

    # Connect callbacks to the events fired by the speech recognizer
    speech_recognizer.recognizing.connect(lambda evt: print('interim text: "{}"'.format(evt.result.text)))
    speech_recognizer.recognized.connect(lambda evt:  print('azure-streaming-stt: "{}"'.format(evt.result.text)))

    # start continuous speech recognition
    speech_recognizer.start_continuous_recognition()

    # push buffer chunks to stream
    buffer, rate = read_wav_file(filename)
    audio_generator = simulate_stream(buffer)
    for chunk in audio_generator:
      stream.write(chunk)
      time.sleep(0.1)  # to give callback a chance against this fast loop

    # stop continuous speech recognition
    stream.close()
    time.sleep(0.5)  # give chance to VAD to kick in
    speech_recognizer.stop_continuous_recognition()
    time.sleep(0.5)  # Let all callback run

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    azure_streaming_stt(t['filename'], t['lang'], t['encoding'])
~~~

The output for the first test case looks like:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
interim text: "experience"
interim text: "experienced"
interim text: "experience"
interim text: "experience proves"
interim text: "experience proves this"
azure-streaming-stt: "Experience proves this."
~~~

---

## IBM Watson Speech to Text Cloud Service

![](https://4.bp.blogspot.com/-QMZoIEzqhDs/XkimgPDd8HI/AAAAAAAATFI/aLGvVgJLapAT1BRZbNMx1HDCRgH15VBbgCKgBGAsYHg/s1600/IBM-Watson.png){: width="25%" class="framedimg alignleft"}

IBM [Watson Speech to Text](https://www.ibm.com/in-en/cloud/watson-speech-to-text){:target="_blank" rel="nofollow"} is ASR service with .NET, Go, JavaScript, [Python](https://cloud.ibm.com/apidocs/speech-to-text/speech-to-text?code=python){:target="_blank" rel="nofollow"}, Ruby, Swift and Unity API libraries, as well as REST endpoints. It has rich documentation.

### Setup

You will need to [sign up/in](https://cloud.ibm.com/docs/services/text-to-speech?topic=text-to-speech-gettingStarted), and get API key credential and service URL, and fill it below.

~~~ python
WATSON_API_KEY = 'YOUR WATSON API KEY'
WATSON_STT_URL = 'YOUR WATSON SERVICE URL'
~~~

### Batch API

The batch API is predictably simple:

~~~ python
import os

from ibm_watson import SpeechToTextV1
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

def watson_batch_stt(filename: str, lang: str, encoding: str) -> str:
    authenticator = IAMAuthenticator(WATSON_API_KEY)
    speech_to_text = SpeechToTextV1(authenticator=authenticator)
    speech_to_text.set_service_url(WATSON_STT_URL)

    with open(filename, 'rb') as audio_file:
        response = speech_to_text.recognize(
            audio=audio_file,
            content_type='audio/{}'.format(os.path.splitext(filename)[1][1:]),
            model=lang + '_BroadbandModel',
            max_alternatives=3,
        ).get_result()

    return response['results'][0]['alternatives'][0]['transcript']

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('watson-batch-stt: "{}"'.format(
        watson_batch_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

Here is the output:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
watson-batch-stt: "experience proves this "

audio file="audio/4507-16021-0012.wav"    expected text="why should one halt on the way"
watson-batch-stt: "why should one hold on the way "

audio file="audio/8455-210777-0068.wav"    expected text="your power is sufficient i said"
watson-batch-stt: "your power is sufficient I set "
~~~

### Streaming API

Watson's streaming API works over WebSocket, and takes a little bit of work to set it all up. It has the following steps:

- Create a `RecognizeCallback` object for receiving speech recognition notifications and results.
- Create a buffer queue. Audio chunks produced by the microphone (or stream simulator) should be written to this queue, and Watson reads and consumes the chunks.
- Start a thread in which speech recognition (along with WebSocket communication) executes.
- Start microphone or speech simulator, to start producing audio chunks
- Upon completion, join the speech recognition thread (i.e. wait till it completes).

~~~ python
import json
import logging
import os
from queue import Queue
from threading import Thread
import time

from ibm_watson import SpeechToTextV1
from ibm_watson.websocket import RecognizeCallback, AudioSource
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Watson websocket prints justs too many debug logs, so disable it
logging.disable(logging.CRITICAL)

# Chunk and buffer size
CHUNK_SIZE = 4096
BUFFER_MAX_ELEMENT = 10

# A callback class to process various streaming STT events
class MyRecognizeCallback(RecognizeCallback):
    def __init__(self):
        RecognizeCallback.__init__(self)
        self.transcript = None

    def on_transcription(self, transcript):
        # print('transcript: {}'.format(transcript))
        pass

    def on_connected(self):
        # print('Connection was successful')
        pass

    def on_error(self, error):
        # print('Error received: {}'.format(error))
        pass

    def on_inactivity_timeout(self, error):
        # print('Inactivity timeout: {}'.format(error))
        pass

    def on_listening(self):
        # print('Service is listening')
        pass

    def on_hypothesis(self, hypothesis):
        # print('hypothesis: {}'.format(hypothesis))
        pass

    def on_data(self, data):
        self.transcript = data['results'][0]['alternatives'][0]['transcript']
        print('{0}final: {1}'.format(
            '' if data['results'][0]['final'] else 'not ',
            self.transcript
        ))

    def on_close(self):
        # print("Connection closed")
        pass

def watson_streaming_stt(filename: str, lang: str, encoding: str) -> str:
    authenticator = IAMAuthenticator(WATSON_API_KEY)
    speech_to_text = SpeechToTextV1(authenticator=authenticator)
    speech_to_text.set_service_url(WATSON_STT_URL)

    # Make watson audio source fed by a buffer queue
    buffer_queue = Queue(maxsize=BUFFER_MAX_ELEMENT)
    audio_source = AudioSource(buffer_queue, True, True)

    # Callback object
    mycallback = MyRecognizeCallback()

    # Read the file
    buffer, rate = read_wav_file(filename)

    # Start Speech-to-Text recognition thread
    stt_stream_thread = Thread(
        target=speech_to_text.recognize_using_websocket,
        kwargs={
            'audio': audio_source,
            'content_type': 'audio/l16; rate={}'.format(rate),
            'recognize_callback': mycallback,
            'interim_results': True
        }
    )
    stt_stream_thread.start()

    # Simulation audio stream by breaking file into chunks and filling buffer queue
    audio_generator = simulate_stream(buffer, CHUNK_SIZE)
    for chunk in audio_generator:
        buffer_queue.put(chunk)
        time.sleep(0.5)  # give a chance to callback

    # Close the audio feed and wait for STTT thread to complete
    audio_source.completed_recording()
    stt_stream_thread.join()

    # send final result
    return mycallback.transcript

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('watson-cloud-streaming-stt: "{}"'.format(
        watson_streaming_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

Output produced:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
not final: X. 
not final: experts 
not final: experience 
not final: experienced 
not final: experience prove 
not final: experience proves 
not final: experience proves that 
not final: experience proves this 
final: experience proves this 
watson-cloud-streaming-stt: "experience proves this "
~~~

---

## Amazon Transcribe Cloud Service

![](https://1.bp.blogspot.com/-Lx_7dsyO7-s/XkimgM8xRZI/AAAAAAAATFI/85tQK1fIW5ogajM-13qGmkoSY-k3Su_owCKgBGAsYHg/s1600/amazon-social-transcribe.jpg){: width="25%" class="framedimg alignleft"}

[Amazon Transcribe](https://aws.amazon.com/transcribe/){:target="_blank" rel="nofollow"} is a [speech-to-text](https://docs.aws.amazon.com/transcribe/latest/dg/getting-started.html){:target="_blank" rel="nofollow"} AWS cloud service with [libraries](https://aws.amazon.com/transcribe/resources/){:target="_blank" rel="nofollow"} in C#, Go, Java, JavaScript, PHP, Python, and Ruby. It has a [batch](https://docs.aws.amazon.com/transcribe/latest/dg/getting-started-python.html){:target="_blank" rel="nofollow"} speech-to-text API (also available as [command line](https://docs.aws.amazon.com/cli/latest/reference/transcribe/start-transcription-job.html){:target="_blank" rel="nofollow"}), but it requires the audio file to be either in S3 bucket, or be available over HTTP. It also has a streaming API on [WebSocket](https://docs.aws.amazon.com/transcribe/latest/dg/websocket.html){:target="_blank" rel="nofollow"} and [HTTP/2](https://docs.aws.amazon.com/transcribe/latest/dg/how-streaming.html){:target="_blank" rel="nofollow"}. Here is an [example with AWS Java SDK](https://docs.aws.amazon.com/transcribe/latest/dg/getting-started-streaming.html){:target="_blank" rel="nofollow"}, but no Python bindings (of course, a Python socket library can be used, but it will require getting into [low-level event stream encoding](https://docs.aws.amazon.com/transcribe/latest/dg/event-stream.html){:target="_blank" rel="nofollow"}).

Amazon Transcribe Python APIs currently do not facilitate use cases covered in this notebook, and therefore code samples are not included here.

---

## Nuance

![](https://2.bp.blogspot.com/-otX2dmaQulI/XkimgKn98EI/AAAAAAAATFI/BjR8l10TgJUI8IJR88mfCnAUi3vyITl6gCKgBGAsYHg/s1600/Nuance-Dragon.png){: width="25%" class="framedimg alignleft"}
 
Nuance is most probably the oldest [commercial speech recognition products](https://www.nuance.com/mobile/speech-recognition-solutions.html){:target="_blank" rel="nofollow"}, even customised for various domains and industries. They do have [Python bindings](https://nuancedev.github.io/samples/http/python/){:target="_blank" rel="nofollow"} for a speech recognition service. Here is a [code sample](https://github.com/NuanceDev/ndev-python-http-cli/blob/master/ndev/asr.py){:target="_blank" rel="nofollow"} in their GitHub repo.
 
I could not figure out a way to create a developer account. I hope there is a way to get a limited period free trial credits similar to other products, and get the credentials needed to access the services.

---

## CMU Sphinx Library

![](https://3.bp.blogspot.com/-UH8R1gJuEhY/XkimgKzfTFI/AAAAAAAATFI/UF17oQqC_sAxZA49ctDcchjIt4_Wk0QqwCKgBGAsYHg/s1600/cmu-sphinx-wav.png){: width="25%" class="framedimg alignleft"}

[CMUSphinx](https://cmusphinx.github.io/){:target="_blank" rel="nofollow"} has been around for quite some time, and has been adapting to advancements in ASR technologies. [PocketSphinx](https://github.com/cmusphinx/pocketsphinx-python){:target="_blank" rel="nofollow"} is speech-to-text decoder Python package.

### Setup

First, install [swig](http://www.swig.org/){:target="_blank" rel="nofollow"}. On macOS, you can install using `brew`:

~~~ bash
$ brew install swig
$ swig -version
~~~

On Linux, you can use `apt-get`:

~~~ bash
$ apt-get install -y swig libpulse-dev
$ swig -version
~~~

And then install `pocketsphinx` using pip:

~~~ bash
$ pip3 install pocketsphinx
$ pip3 list | grep pocketsphinx
~~~

### Create a Decoder Object

Whether you use batch or streaming API, you will require a decoder object:

~~~ python
import pocketsphinx
import os

MODELDIR = os.path.join(os.path.dirname(pocketsphinx.__file__), 'model')

config = pocketsphinx.Decoder.default_config()
config.set_string('-hmm', os.path.join(MODELDIR, 'en-us'))
config.set_string('-lm', os.path.join(MODELDIR, 'en-us.lm.bin'))
config.set_string('-dict', os.path.join(MODELDIR, 'cmudict-en-us.dict'))

decoder = pocketsphinx.Decoder(config)
~~~

### Batch API

Batch API is expectedly simple, just a couple of lines of code:

~~~ python
def sphinx_batch_stt(filename: str, lang: str, encoding: str) -> str:
    buffer, rate = read_wav_file(filename)
    decoder.start_utt()
    decoder.process_raw(buffer, False, False)
    decoder.end_utt()
    hypothesis = decoder.hyp()
    return hypothesis.hypstr

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('sphinx-batch-stt: "{}"'.format(
        sphinx_batch_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

And you will see now familiar output:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
sphinx-batch-stt: "experience proves this"

audio file="audio/4507-16021-0012.wav"    expected text="why should one halt on the way"
sphinx-batch-stt: "why should one hold on the way"

audio file="audio/8455-210777-0068.wav"    expected text="your power is sufficient i said"
sphinx-batch-stt: "your paris sufficient i said"
~~~

Notice the errors in transcription. With more training data, it typically improves.


### Streaming API

Streaming APIs are also quite simple, but there is no hook to get intermediate results:

~~~ python
def sphinx_streaming_stt(filename: str, lang: str, encoding: str) -> str:
    buffer, rate = read_wav_file(filename)
    audio_generator = simulate_stream(buffer)

    decoder.start_utt()
    for chunk in audio_generator:
        decoder.process_raw(chunk, False, False)
    decoder.end_utt()

    hypothesis = decoder.hyp()
    return hypothesis.hypstr

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('sphinx-streaming-stt: "{}"'.format(
        sphinx_streaming_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

---

## Mozilla DeepSpeech

![](https://2.bp.blogspot.com/-RVIupURDNUI/XkimgGEjH-I/AAAAAAAATFI/yzty4qVV1UYHbbC9rITrRuWI6zYu27GswCKgBGAsYHg/s1600/Mozilla-DeepSpeech-logo.png){: width="25%" class="framedimg alignleft"}

Mozilla released [DeepSpeech 0.6](https://hacks.mozilla.org/2019/12/deepspeech-0-6-mozillas-speech-to-text-engine/){:target="_blank" rel="nofollow"} software package in December 2019 with [APIs](https://github.com/mozilla/DeepSpeech/releases/tag/v0.6.0){:target="_blank" rel="nofollow"} in C, Java, .NET, [Python](https://deepspeech.readthedocs.io/en/v0.6.0/Python-API.html){:target="_blank" rel="nofollow"}, and JavaScript, including support for TensorFlow Lite models for use on edge devices.

### Setup

You can install DeepSpeech with pip (make it `deepspeech-gpu==0.6.0` if you want to use GPU in Colab runtime or on your machine):

~~~ bash
$ pip install deepspeech==0.6.0
~~~

Download and unzip the models (this will take a while):

~~~ bash
$ curl -LO https://github.com/mozilla/DeepSpeech/releases/download/v0.6.0/deepspeech-0.6.0-models.tar.gz
$ tar -xvzf deepspeech-0.6.0-models.tar.gz
$ ls -l ./deepspeech-0.6.0-models/
~~~

Test that it all works. Examine the output of the last three commands, and you will see results *“experience proof less”*, *“why should one halt on the way”*, and *“your power is sufficient i said”* respectively. You are all set.

~~~ bash
$ deepspeech --model deepspeech-0.6.0-models/output_graph.pb --lm deepspeech-0.6.0-models/lm.binary --trie ./deepspeech-0.6.0-models/trie --audio ./audio/2830-3980-0043.wav

$ deepspeech --model deepspeech-0.6.0-models/output_graph.pb --lm deepspeech-0.6.0-models/lm.binary --trie ./deepspeech-0.6.0-models/trie --audio ./audio/4507-16021-0012.wav

$ deepspeech --model deepspeech-0.6.0-models/output_graph.pb --lm deepspeech-0.6.0-models/lm.binary --trie ./deepspeech-0.6.0-models/trie --audio ./audio/8455-210777-0068.wav
~~~

### Create a Model Object

The first step is to read the model files and create DeepSpeech model object.

~~~ python
import deepspeech

model_file_path = 'deepspeech-0.6.0-models/output_graph.pbmm'
beam_width = 500
model = deepspeech.Model(model_file_path, beam_width)

# Add language model for better accuracy
lm_file_path = 'deepspeech-0.6.0-models/lm.binary'
trie_file_path = 'deepspeech-0.6.0-models/trie'
lm_alpha = 0.75
lm_beta = 1.85
model.enableDecoderWithLM(lm_file_path, trie_file_path, lm_alpha, lm_beta)
~~~

### Batch API

It takes just a couple of line of code for doing batch speech-to-text:

~~~ python
import numpy as np

def deepspeech_batch_stt(filename: str, lang: str, encoding: str) -> str:
    buffer, rate = read_wav_file(filename)
    data16 = np.frombuffer(buffer, dtype=np.int16)
    return model.stt(data16)

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('deepspeech-batch-stt: "{}"'.format(
        deepspeech_batch_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

Output:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
deepspeech-batch-stt: "experience proof less"

audio file="audio/4507-16021-0012.wav"    expected text="why should one halt on the way"
deepspeech-batch-stt: "why should one halt on the way"

audio file="audio/8455-210777-0068.wav"    expected text="your power is sufficient i said"
deepspeech-batch-stt: "your power is sufficient i said"
~~~

### Streaming API

DeepSpeech streaming API requires creating a stream-context and use it repeatedly to feed chunks of audio:

~~~ python
def deepspeech_streaming_stt(filename: str, lang: str, encoding: str) -> str:
    buffer, rate = read_wav_file(filename)
    audio_generator = simulate_stream(buffer)

    # Create stream
    context = model.createStream()

    text = ''
    for chunk in audio_generator:
        data16 = np.frombuffer(chunk, dtype=np.int16)
        # feed stream of chunks
        model.feedAudioContent(context, data16)
        interim_text = model.intermediateDecode(context)
        if interim_text != text:
            text = interim_text
            print('inetrim text: {}'.format(text))

    # get final resut and close stream
    text = model.finishStream(context)
    return text

# Run tests
for t in TESTCASES:
    print('\naudio file="{0}"    expected text="{1}"'.format(t['filename'], t['text']))
    print('deepspeech-streaming-stt: "{}"'.format(
        deepspeech_streaming_stt(t['filename'], t['lang'], t['encoding'])
    ))
~~~

DeepSpeech returns interim results:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
inetrim text: i
inetrim text: e
inetrim text: experi en
inetrim text: experience pro
inetrim text: experience proof les
deepspeech-streaming-stt: "experience proof less"
~~~

Check out how to write a [Python transcriber that converts microphone input to text](/tech/python-speech-to-text-asr-transcriber-with-mozilla-deepspeech.html).

---

## Kaldi

![](https://1.bp.blogspot.com/-hCAFOfb6zZ4/XkimgFgUy6I/AAAAAAAATFI/JlQQuFsgXzshePAPp-2j6XBoZWdqpXEBwCKgBGAsYHg/s1600/Kaldi-ASR.png){: width="25%" class="framedimg alignleft"}
 
[Kaldi](https://kaldi-asr.org/doc/about.html){:target="_blank" rel="nofollow"} is a very popular software toolkit speech recognition among the research community. It is designed to experiment with different research ideas and possibilities. It has a rich collection of various possible techniques and alternatives. The learning curve is steeper compared to other alternatives discussed in the code lab.
 
[PyKaldi](https://pykaldi.github.io/){:target="_blank" rel="nofollow"} provides [Python bindings](https://pykaldi.github.io/api/kaldi.asr.html){:target="_blank" rel="nofollow"}. Please do take a look at [README](https://github.com/pykaldi/pykaldi){:target="_blank" rel="nofollow"} of their GitHub repo.
 
There is no pre-build PyPI ready to use package, and you have to build it either from source or from Conda. Neither options suit the Colab environment.

---

## Facebook wav2letter

![](https://4.bp.blogspot.com/-1FThWMRiDqQ/Xkjh5vf9CZI/AAAAAAAATFk/mWRhksvFq9AK0UKYjlgOFZ_Z37bRlbmFQCKgBGAsYHg/s1600/fb-wav2letter.png){: width="25%" class="framedimg alignleft"}
 
[Facebook wav2letter@anywhere](https://ai.facebook.com/blog/online-speech-recognition-with-wav2letteranywhere/){:target="_blank" rel="nofollow"} released  in January 2020. It boasts a fully convolutional (CNN) acoustic model instead of a recurrent neural network (RNN) that is used by other solutions. It is very promising, include for use in edge devices. It has [Python bindings](https://github.com/facebookresearch/wav2letter/wiki/Python-bindings){:target="_blank" rel="nofollow"} for its [inference framework](https://github.com/facebookresearch/wav2letter/wiki/Inference-Framework){:target="_blank" rel="nofollow"}.
 
Like Kaldi, this also does not provide a PyPI package, and needs to build and installe from [source](https://github.com/facebookresearch/wav2letter/){:target="_blank" rel="nofollow"}.

---

## SpeechRecognition Python Package

The [SpeechRecognition](https://pypi.org/project/SpeechRecognition/){:target="_blank" rel="nofollow"} package provides a nice abstraction over several solutions. We already explored using Google service and CMU Sphinxpackage. Now we will use these through SpeechRecognition package APIs. It can be installed using pip:

~~~ bash
$ pip3 install SpeechRecognition
~~~

### Batch API

SpeechRecognition has only batch API. The first step to create an audio record, either from a file or from a microphone, and the second step is to call `recognize_<speech engine name>` function. It currently has APIs for [CMU Sphinx, Google, Microsoft, IBM, Houndify, and Wit](https://github.com/Uberi/speech_recognition){:target="_blank" rel="nofollow"}. Let's checkout using one cloud service (Google) and one software package (Sphinx) through SpeechRecognition abstraction.

``` python
import speech_recognition as sr
from enum import Enum, unique

@unique
class ASREngine(Enum):
    sphinx = 0
    google = 1

def speech_to_text(filename: str, engine: ASREngine, language: str, show_all: bool = False) -> str:
    r = sr.Recognizer()

    with sr.AudioFile(filename) as source:
        audio = r.record(source)

    asr_functions = {
        ASREngine.sphinx: r.recognize_sphinx,
        ASREngine.google: r.recognize_google,
    }

    response = asr_functions[engine](audio, language=language, show_all=show_all)
    return response

# Run tests
for t in TESTCASES:
    filename = t['filename']
    text = t['text']
    lang = t['lang']

    print('\naudio file="{0}"    expected text="{1}"'.format(filename, text))
    for asr_engine in ASREngine:
        try:
            response = speech_to_text(filename, asr_engine, language=lang)
            print('{0}: "{1}"'.format(asr_engine.name, response))
        except sr.UnknownValueError:
            print('{0} could not understand audio'.format(asr_engine.name))
        except sr.RequestError as e:
            print('{0} error: {0}'.format(asr_engine.name, e))
```

Output:

~~~
audio file="audio/2830-3980-0043.wav"    expected text="experience proves this"
sphinx: "experience proves that"
google: "experience proves this"

audio file="audio/4507-16021-0012.wav"    expected text="why should one halt on the way"
sphinx: "why should one hold on the way"
google: "why should one halt on the way"

audio file="audio/8455-210777-0068.wav"    expected text="your power is sufficient i said"
sphinx: "your paris official said"
google: "your power is sufficient I said"
~~~

### API for other providers

For other speech recognition providers, you will need to create API credentials, which you have to pass to `recognize_<speech engine name>` function, you can check out [this example](https://github.com/Uberi/speech_recognition/blob/master/examples/audio_transcribe.py){:target="_blank" rel="nofollow"}.

It also has a nice abstraction for Microphone, implemented over PyAudio/PortAudio. Check out examples to capture input from the microphone in [batch](https://github.com/Uberi/speech_recognition/blob/master/examples/microphone_recognition.py){:target="_blank" rel="nofollow"} and continuously in the [background](https://github.com/Uberi/speech_recognition/blob/master/examples/background_listening.py){:target="_blank" rel="nofollow"}.

---

> Hope you enjoyed reading it. Which ASR engine did you like the most, and Why? Please share it in comments.
