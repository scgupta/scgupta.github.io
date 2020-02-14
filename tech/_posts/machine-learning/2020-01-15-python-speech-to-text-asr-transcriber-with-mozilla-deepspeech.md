---
title: How to build Python transcriber using Mozilla DeepSpeech
excerpt: "Learn to build audio transcriber for voice applications using PyAudio and DeepSpeech speech-to-text ASR API in less than 70 lines of Python code."
image: "https://3.bp.blogspot.com/-Um4CzvPw_wo/XiGZJT3QHbI/AAAAAAAASzg/oTh6tU8mPhsIu3jAOspjCFY192Mpbe2JACKgBGAsYHg/s1600/mozilla-deepspeech-py-transcriber-slang.png"
image_thumbnail: "https://3.bp.blogspot.com/-Um4CzvPw_wo/XiGZJT3QHbI/AAAAAAAASzg/oTh6tU8mPhsIu3jAOspjCFY192Mpbe2JACKgBGAsYHg/s320/mozilla-deepspeech-py-transcriber-slang.png"
tags:
  - "machine-learning"
  - "speech-recognition"
  - "deep-speech"
  - "python"
code: true
---

Voice assistants are one of the hottest tech right now. Siri, Alexa, Google Assistant, all aim to help you talk to computers and not just touch and type. Automated Speech Recognition (ASR) and Natural Language Understanding (NLU/NLP) are the key technologies enabling it. If you are just-a-programmer like me, you might be itching to get a piece of action and hack something. You are at the right place; read on.

Though these technologies are hard and the learning curve is steep, but are becoming increasingly accessible. Last month, Mozilla released [DeepSpeech 0.6](https://github.com/mozilla/DeepSpeech/releases/tag/v0.6.0){:target="_blank" rel="nofollow"} along with models for US English. It has smaller and faster models than ever before, and even has a [TensorFlow Lite](https://www.tensorflow.org/lite){:target="_blank" rel="nofollow"} model that [runs faster than real time on a single core of a Raspberry Pi 4](https://hacks.mozilla.org/2019/12/deepspeech-0-6-mozillas-speech-to-text-engine/){:target="_blank" rel="nofollow"}. There are several interesting aspects, but right now I am going to focus on its refreshingly [simple batch and stream APIs in C, .NET, Java, JavaScript, and Python](https://deepspeech.readthedocs.io/en/v0.6.0/Python-API.html){:target="_blank" rel="nofollow"} for converting speech to text. By the end of this blog post, you will build a voice transcriber. No kidding :-)


## Let’s get started

You need a computer with [Python 3.6.5+ installed](https://realpython.com/installing-python/){:target="_blank" rel="nofollow"}, good internet connection, and elementary Python programming skills. Even if you do not know Python, read along, it is not so hard.
If you don't want to install anything, you can try out DeepSpeech APIs in the browser using this [code lab](http://bit.ly/colab-mozilla-deepspeech-python){:target="_blank" rel="nofollow"}.

Let’s do needed setup:

~~~ bash
# Create virtual environment named ds06
$ python3 -m venv ./some/pyenv/dir/path/ds06

# Switch to virtual environment
$ source ./some/pyenv/dir/path/ds06/bin/activate

# Install DeepSpeech
$ pip3 install deepspeech==0.6.0

# Download and unzip en-US models, this will take a while
$ mkdir -p ./some/workspace/path/ds06
$ cd ./some/workspace/path/ds06
$ curl -LO https://github.com/mozilla/DeepSpeech/releases/download/v0.6.0/deepspeech-0.6.0-models.tar.gz
$ tar -xvzf deepspeech-0.6.0-models.tar.gz
x deepspeech-0.6.0-models/
x deepspeech-0.6.0-models/lm.binary
x deepspeech-0.6.0-models/output_graph.pbmm
x deepspeech-0.6.0-models/output_graph.pb
x deepspeech-0.6.0-models/trie
x deepspeech-0.6.0-models/output_graph.tflite

$ ls -l ./deepspeech-0.6.0-models/

# Download and unzip some audio samples to test setup
$ curl -LO https://github.com/mozilla/DeepSpeech/releases/download/v0.6.0/audio-0.6.0.tar.gz
$ tar -xvzf audio-0.6.0.tar.gz
x audio/
x audio/2830-3980-0043.wav
x audio/Attribution.txt
x audio/4507-16021-0012.wav
x audio/8455-210777-0068.wav
x audio/License.txt

$ ls -l ./audio/

# Test deepspeech
$ deepspeech --model deepspeech-0.6.0-models/output_graph.pb --lm deepspeech-0.6.0-models/lm.binary --trie ./deepspeech-0.6.0-models/trie --audio ./audio/2830-3980-0043.wav

$ deepspeech --model deepspeech-0.6.0-models/output_graph.pb --lm deepspeech-0.6.0-models/lm.binary --trie ./deepspeech-0.6.0-models/trie --audio ./audio/4507-16021-0012.wav

$ deepspeech --model deepspeech-0.6.0-models/output_graph.pb --lm deepspeech-0.6.0-models/lm.binary --trie ./deepspeech-0.6.0-models/trie --audio ./audio/8455-210777-0068.wav
~~~

Examine the output of the last three commands, and you will see results _&ldquo;experience proof less&rdquo;_, _&ldquo;why should one halt on the way&rdquo;_, and _&ldquo;your power is sufficient i said&rdquo;_ respectively. You are all set.


## DeepSpeech API

The API is quite simple. You first need to create a model object using the model files you downloaded:

~~~ python
$ python3
>>> import deepspeech
>>> model_file_path = 'deepspeech-0.6.0-models/output_graph.pbmm'
>>> beam_width = 500
>>> model = deepspeech.Model(model_file_path, beam_width)
~~~

You should add language model for better accuracy:

~~~ python
>>> lm_file_path = 'deepspeech-0.6.0-models/lm.binary'
>>> trie_file_path = 'deepspeech-0.6.0-models/trie'
>>> lm_alpha = 0.75
>>> lm_beta = 1.85
>>> model.enableDecoderWithLM(lm_file_path, trie_file_path, lm_alpha, lm_beta)
~~~

Once you have the model object, you can use either batch or streaming speech-to-text API.

### Batch API

To use the batch API, the first step is to read the audio file:

~~~ python
>>> import wave
>>> filename = 'audio/8455-210777-0068.wav'
>>> w = wave.open(filename, 'r')
>>> rate = w.getframerate()
>>> frames = w.getnframes()
>>> buffer = w.readframes(frames)
>>> print(rate)
16000
>>> print(model.sampleRate())
16000
>>> type(buffer)
<class 'bytes'>
~~~

As you can see that the speech sample rate of the wav file is 16000hz, same as the model’s sample rate. But the buffer is a byte array, whereas DeepSpeech model expects 16-bit int array. Let’s convert it:

~~~ python
>>> import numpy as np
>>> data16 = np.frombuffer(buffer, dtype=np.int16)
>>> type(data16)
<class 'numpy.ndarray'>
~~~

Run speech-to-text in batch mode to get the text:

~~~ python
>>> text = model.stt(data16)
>>> print(text)
your power is sufficient i said
~~~

### Streaming API

Now let’s accomplish the same using streaming API. It consists of 3 steps: open session, feed data, close session.

Open a streaming session:

~~~ python
>>> context = model.createStream()
~~~

Repeatedly feed chunks of speech buffer, and get interim results if desired:

~~~ python
>>> buffer_len = len(buffer)
>>> offset = 0
>>> batch_size = 16384
>>> text = ''
>>> while offset < buffer_len:
...     end_offset = offset + batch_size
...     chunk = buffer[offset:end_offset]
...     data16 = np.frombuffer(chunk, dtype=np.int16)
...     model.feedAudioContent(context, data16)
...     text = model.intermediateDecode(context)
...     print(text)
...     offset = end_offset
...
i
i
your power is
your power is suffi
your power is sufficient i said
your power is sufficient i said
~~~

Close stream and get the final result:

~~~ python
>>> text = model.finishStream(context)
>>> print(text)
your power is sufficient i said
~~~


## Transcriber

A transcriber consists of two parts: a producer that captures voice from microphone, and a consumer that converts this speech stream to text. These two execute in parallel. The audio recorder keeps producing chunks of the speech stream. The speech recognizer listens to this stream, consumes these chunks upon arrival and updates the transcribed text.

![](https://4.bp.blogspot.com/-q6QdwMp9bqQ/XiEs-A0Vk-I/AAAAAAAASw8/GXJ0ia7_KnAy5OCdbRvU28kMYaWl-6qQACKgBGAsYHg/s1600/AudioTranscriber.png){:width="100%"}

To capture audio, we will use [PortAudio](http://www.portaudio.com/){:target="_blank" rel="nofollow"}, a free, cross-platform, open-source, audio I/O library. You have to [download](http://www.portaudio.com/download.html){:target="_blank" rel="nofollow"} and install it. On macOS, you can install it using [brew](https://brew.sh/){:target="_blank" rel="nofollow"}:

~~~ bash
$ brew install portaudio
~~~

[PyAudio](https://people.csail.mit.edu/hubert/pyaudio/) is Python bindings for PortAudio, and you can install it with pip:

~~~ bash
$ pip3 install pyaudio
~~~

PyAudio has two modes: blocking, where data has to read (pulled) from the stream; and [non-blocking, where a callback function](https://people.csail.mit.edu/hubert/pyaudio/docs/#pyaudio.Stream.__init__){:target="_blank" rel="nofollow"} is passed to PyAudio for feeding (pushing) the audio data stream. The non-blocking mechanism suits transcriber. The data buffer processing code using DeepSpeech streaming API has to be wrapped in a call back:

~~~ python
text_so_far = ''
def process_audio(in_data, frame_count, time_info, status):
    global text_so_far
    data16 = np.frombuffer(in_data, dtype=np.int16)
    model.feedAudioContent(context, data16)
    text = model.intermediateDecode(context)
    if text != text_so_far:
        print('Interim text = {}'.format(text))
        text_so_far = text
    return (in_data, pyaudio.paContinue)
~~~

Now you have to create a PyAudio input stream with this callback:

~~~ python
audio = pyaudio.PyAudio()
stream = audio.open(
    format=pyaudio.paInt16,
    channels=1,
    rate=16000,
    input=True,
    frames_per_buffer=1024,
    stream_callback=process_audio
)

print('Please start speaking, when done press Ctrl-C ...')
stream.start_stream()
~~~

Finally, you need to print the final result and clean up when a user ends recording by pressing Ctrl-C:

~~~ python
try: 
    while stream.is_active():
        time.sleep(0.1)
except KeyboardInterrupt:
    # PyAudio
    stream.stop_stream()
    stream.close()
    audio.terminate()
    print('Finished recording.')
    # DeepSpeech
    text = model.finishStream(context)
    print('Final text = {}'.format(text))
~~~

That’s all it takes, just 66 lines of Python code to put it all together: [ds-transcriber.py](https://github.com/scgupta/yearn2learn/blob/master/speech/asr/deepspeech06/ds-transcriber.py){:target="_blank" rel="nofollow"}.


## Recap

In this article, you had a quick introduction to batch and stream APIs of DeepSpeech 0.6, and learned how to marry it with PyAudio to create a speech transcriber. The ASR model used here is for US English speakers, accuracy will vary for other accents. By replacing the model for other languages or accents, the same code will work for that language/accent.

Did you enjoy building it? Any feedback, improvements, suggestions? What other voice application would you like to build? Do let me know in the comments. Thanks for reading.

