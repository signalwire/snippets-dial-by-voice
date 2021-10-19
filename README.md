# Dial By Voice
This application using the [SignalWire Python SDK](https://developer.signalwire.com/twiml/reference/client-libraries-and-sdks#python) will prompt the caller for a phone number via speech input and connect to the phone number recognized using ASR providing an additional level of accessibility to your users. 



# Configuring the code 
This code is incredibly simple to implement! The first route `/dial-prompt` handles incoming http requests for a provisioned phone number, and prompts users to say a number to call, and asks them if they wish to dial it.  If the number to dial can not be understood, they will be prompted again to repeat the number. 

```python
# accepts web requests to `/dial-prompt`
@app.route('/dial-prompt', methods=['GET', 'POST'])
def dial_prompt():

    # Initialize VoiceResponse
    response = VoiceResponse()

    # check if user input was provided via dtmf entry
    if "SpeechResult" in request.values:

        # Read speech result
        speech_result = request.values.get("SpeechResult")

        # Validate Number Provided
        number = re.sub("[^0-9]", "", speech_result)

        # Make E164
        if len(number) == 11:
            number = "+" + number

        # Assume US Number, Make E164
        elif len(number) == 10:
            number = "+1" + number

        # We did not determine a valid phone number
        else:
            response.say("I am sorry, I did not understand you.  Please try again.", voice="man")
            response.redirect("/dial-prompt")
            return str(response)

        # Prompt user
        gather = Gather(action='/dial-verify?number_to_dial=' + number, input='speech', speechTimeout="auto", timeout="10", method='GET')

        # Append say to gather to produce TTS
        gather.say("We detected " + speech_result + " Would you like me to connect you? ")

        # Append the gather
        response.append(gather)

        # Hangup the call
        response.hangup()

    else:

        # Prompt user
        gather = Gather(action='/dial-prompt', input='speech', speechTimeout="auto",  timeout="10", method='GET')

        # Append say to gather to produce TTS
        gather.say("What number would you like to dial?")

        # Append the gather 
        response.append(gather)

        # Hangup the call
        response.hangup()

    # return response
    return str(response)

```

The second route is `/dial-verify` and it dials the number after a successful Yes is detected and verified by the user in the previous route.  If the user does not say Yes. they get a goodbye message instead. 

```python
# accept web requests to '/dial-verify'
@app.route('/dial-verify', methods=['GET', 'POST'])
def dial_verify():

    # Initialize VoiceResponse
    response = VoiceResponse()

    if "SpeechResult" in request.values:
        if request.values.get("SpeechResult") == "Yes.":

            # Read the passed in number to dial from the request
            number_to_dial = request.values.get("number_to_dial")

            # Append the gather
            response.dial(number_to_dial)
            return str(response)

    response.say("OK. Thank you for calling goodbye!")
    return str(response)
```

# Build and Run on Docker

1. Use our pre-built image from Docker Hub 
```

docker pull signalwire/snippets-dial-by-voice:python
```
(or build your own image)

1. Build your image
```
docker build -t snippets-dial-by-voice .
```
2. Run your image
```
docker run --publish 5000:5000 --env-file .env snippets-dial-by-voice
```
3. The application will run on port 5000

# Build and Run Natively

To run the application, execute export FLASK_APP=app.py then run flask run.

You may need to use an SSH tunnel for testing this code if running on your local machine. – we recommend [ngrok](https://ngrok.com/). You can learn more about how to use ngrok [here](https://developer.signalwire.com/apis/docs/how-to-test-webhooks-with-ngrok). 

# Sign Up Here

If you would like to test this example out, you can create a SignalWire account and space [here](https://m.signalwire.com/signups/new?s=1).

Please feel free to reach out to us on our Community Slack or create a Support ticket if you need guidance!
