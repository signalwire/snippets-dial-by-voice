# Snippets Dial By Voice
This snippet will show you how to implement a dial phone number by voice application.
## About Dial By Voice
This application will prompt the caller for a phone number, and connect to the phone number recognized using ASR.
## Getting Started
You will need a machine with Python installed, the SignalWire SDK, a provisioned SignalWire phone number, and optionaly Docker if you decide to run it in a container.

For this demo we will be using Python, but more languages may become available.

- [x] Python
- [x] SignalWire SDK
- [x] SignalWire Phone Number
- [x] Docker (Optional)
----
## Running Dial By Voice - How It Works
## Methods and Endpoints

```
Endpoint: /dial-prompt
Methods: GET OR POST
Handles incoming LaML requests for provisioned phone number, and prompts user to say a number to call, and asks them if the wish to dial it. Speech Input: (Yes or No)  If the number to dial can not be understood it will prompt again. 
```

```
Endpoint: /dial-verify
Methods: GET OR POST
Dials the number, after a successful Yes. is detected and verified from the user.  If the user does not say Yes. they get a goodbye message.
```

## Setup Your Enviroment File
This code does not require an enviroment file.

## Build and Run on Docker
Lets get started!
1. Use our pre-built image from Docker Hub 
```
For Python:
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

## Build and Run Natively
For Python
```
1. Replace environment variables
2. From command line run, python3 app.py
```

----
# More Documentation
You can find more documentation on LaML, Relay, and all Signalwire APIs at:
- [SignalWire Python SDK](https://github.com/signalwire/signalwire-python)
- [SignalWire API Docs](https://docs.signalwire.com)
- [SignalWire Github](https://gituhb.com/signalwire)
- [Docker - Getting Started](https://docs.docker.com/get-started/)
- [Python - Gettings Started](https://docs.python.org/3/using/index.html)

# Support
If you have any issues or want to engage further about this Signal, please [open an issue on this repo](../../issues) or join our fantastic [Slack community](https://signalwire.community) and chat with others in the SignalWire community!

If you need assistance or support with your SignalWire services please file a support ticket from your Dashboard. 

