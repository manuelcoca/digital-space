---
tags: azure, azureai, azureaispeech
---

Similarly to its **Speech to text** APIs, the Azure AI Speech service offers other REST APIs for speech synthesis:

-   The **Text to speech** API, which is the primary way to perform speech synthesis.
-   The **Batch synthesis** API, which is designed to support batch operations that convert large volumes of text to audio - for example to generate an audio-book from the source text.

## SDK implementation

1. Use a **SpeechConfig** object to encapsulate the information required to connect to your Azure AI Speech resource. Specifically, its **location** and **key**.
2. Optionally, use an **AudioConfig** to define the output device for the speech to be synthesized. By default, this is the default system speaker, but you can also specify an audio file, or by explicitly setting this value to a null value, you can process the audio stream object that is returned directly.
3. Use the **SpeechConfig** and **AudioConfig** to create a **SpeechSynthesizer** object. This object is a proxy client for the **Text to speech** API.
4. Use the methods of the **SpeechSynthesizer** object to call the underlying API functions. For example, the **SpeakTextAsync()** method uses the Azure AI Speech service to convert text to spoken audio.
5. Process the response from the Azure AI Speech service. In the case of the **SpeakTextAsync** method, the result is a **SpeechSynthesisResult** object that contains the following properties:
    1. AudioData
    2. Properties
    3. Reason
    4. ResultId

When speech has been successfully synthesized, the **Reason** property is set to the **SynthesizingAudioCompleted**enumeration and the **AudioData** property contains the audio stream (which, depending on the **AudioConfig** may have been automatically sent to a speaker or file).

## Voices

The Azure AI Speech service provides multiple voices that you can use to personalize your speech-enabled applications. There are two kinds of voice that you can use:

-   *Standard voices* - synthetic voices created from audio samples.
-   *Neural voices* - more natural sounding voices created using deep neural networks.

Voices are identified by names that indicate a locale and a person's name - for example `en-GB-George`.

To specify a voice for speech synthesis in the **SpeechConfig**, set its **SpeechSynthesisVoiceName** property to the voice you want to use:

```
speechConfig.SpeechSynthesisVoiceName = "en-GB-George";
```

## Audio format

The **SpeechConfig** object can be used to customize the audio that is returned by the API.

Different possible formats (defined in the **SpeechSynthesisOutputFormat** enum):

-   Audio file type
-   Sample-rate
-   Bit-depth

## Speech Synthesis Markup Language (SSML)

The Text to Speech API supports an XML-based syntax for describing characteristics of the speech and offers greater control over how the spoken ouput should sound:

-   Specify a speaking style, such as "excited" or "cheerful" when using a neural voice.
-   Insert pauses or silence.
-   Specify *phonemes* (phonetic pronunciations), for example to pronounce the text "SQL" as "sequel".
-   Adjust the *prosody* of the voice (affecting the pitch, timbre, and speaking rate).
-   Use common "say-as" rules, for example to specify that a given string should be expressed as a date, time, telephone number, or other form.
-   Insert recorded speech or audio, for example to include a standard recorded message or simulate background noise.

For example, consider the following SSML:

```
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
                     xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        <mstts:express-as style="cheerful">
          I say tomato
        </mstts:express-as>
    </voice>
    <voice name="en-US-GuyNeural">
        I say <phoneme alphabet="sapi" ph="t ao m ae t ow"> tomato </phoneme>.
        <break strength="weak"/>Lets call the whole thing off!
    </voice>
</speak>
```

This SSML specifies a spoken dialog between two different neural voices, like this:

-   **Ariana** (_cheerfully_): "I say tomato:
-   **Guy**: "I say tomato (pronounced *tom-ah-toe*) ... Let's call the whole thing off!"

To submit an SSML description to the Speech service, you can use the **SpeakSsmlAsync()** method, like this:

```
speechSynthesizer.SpeakSsmlAsync(ssml_string);
```

For more information about SSML, see the [Azure AI Speech SDK documentation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-synthesis-markup).
