# IBM-Watson-Speech-to-Text

Implementing IBM Watson Speech to Text involves several steps, including setting up an IBM Cloud account, creating a Speech to Text service, and integrating the service into your application. 

**Step 1: Set up an IBM Cloud account**

If you don't have an IBM Cloud account, sign up for a free account at https://cloud.ibm.com/registration.

**Step 2: Install the IBM Watson SDK**

Open your terminal or command prompt and install the IBM Watson SDK for Python:

```bash
pip install ibm-watson
```

**Step 3: Create a Speech to Text service**

Once you have an IBM Cloud account, log in, and create a new Speech to Text service instance:

- Go to https://cloud.ibm.com/catalog/services/speech-to-text and click on "Create" to create a new Speech to Text service.
- Choose a plan (Lite plan should be sufficient for testing and small-scale usage).
- Give your service a name and click on "Create."

**Step 4: Obtain the API key and URL**

After creating the Speech to Text service instance, you need to obtain the API key and URL, which will be used to authenticate your requests:

- Go to the "Manage" section of your Speech to Text service instance.
- Click on "Show Credentials" to reveal your API key and URL.

**Step 5: Write the Python code**

Now, let's create a Python script to use the Watson Speech to Text service:

```python
from ibm_watson import SpeechToTextV1
from ibm_watson.websocket import RecognizeCallback, AudioSource
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Replace 'YOUR_API_KEY' and 'YOUR_URL' with the actual API key and URL obtained in Step 4
authenticator = IAMAuthenticator('YOUR_API_KEY')
speech_to_text = SpeechToTextV1(authenticator=authenticator)
speech_to_text.set_service_url('YOUR_URL')

# The audio file you want to transcribe
audio_file = 'path/to/your/audio.wav'

# Open the audio file and set the RecognizeCallback
class MyRecognizeCallback(RecognizeCallback):
    def __init__(self):
        RecognizeCallback.__init__(self)

    def on_transcription(self, transcript):
        print(transcript)

    def on_connected(self):
        print('Connection was successful')

    def on_error(self, error):
        print('Error received: {}'.format(error))

    def on_inactivity_timeout(self, error):
        print('Inactivity timeout: {}'.format(error))

# Open the audio file and start the transcription
with open(audio_file, 'rb') as audio_file:
    audio_source = AudioSource(audio_file)
    callback = MyRecognizeCallback()
    speech_to_text.recognize_using_websocket(audio=audio_source, content_type='audio/wav', recognize_callback=callback)
```

Replace `'path/to/your/audio.wav'` with the path to your audio file that you want to transcribe. Ensure the audio file is in WAV format, as the example above uses the `'audio/wav'` content type.

**Step 6: Run the Python script**

Save the Python code to a file (e.g., `speech_to_text_example.py`) and run it using the command:

```bash
python speech_to_text_example.py
```

The script will transcribe the audio and print the results to the console.

That's it! You've now implemented IBM Watson Speech to Text using Python. 
