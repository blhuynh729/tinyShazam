

# Project Overview
The purpose of this project is to create an song recognition program similar to Shazam. When the user hears a song that they are unable to recognize, they can activate the CC3200 Launchpad to listen and identify this unknown song. The Lanuchpad will record for a duration of 5 seconds, and display the name and artist of the song on the OLED screen.

# Key Features
- FRAM: 256 kBit FRAM chip is used to store 5s of 8bit audio 
- Adafruit Electret Microphone Amplifier to detect audio
    
# Discussion 
- Our initial plan was to create a AWS lamba function that will be triggered by a HTTP POST from the CC3200 launchpad. The POST will transmit to the lamba function a json file containing the base64 representation of the ADC song data. The audio is sampled using ADC on the CC3200, but due to 1.4V limit, we used a voltage divider to reduce the size of the audio waveform, which normally can be up to 2Vpp with a 1.25V offset. To ensure data integrity, we encode the raw audio data in base64 before transmission. 

- To reduce the number of steps in this process, we decided to use the AWS LightSail server. We chose a lightsail server primarily due to the dependencies of the ShazamAPI Python library on ffmpeg, which was difficult to satisty in a Lamda function. The Lightsail server hosts a Flask (Python) web server that can wait for POST requests and handle them as they come in. The POST Function waits for all audio chunks to come in and stitches them into a RAW binary file. From there, it uses another Python audio processing library, providing the bit depth and sample rate, to generate an MP3 file from the audio samples. This MP3 is fed into the ShazamAPI, which generates song information if the song is found.

- The function to identify the song utilized the ShazamAPI developed by Marin-m. The imported library used audio fingerprinting to generate a spectrogram of the sound, and maps out the frequency peaks from it. These frequency peaks are then sent to the Shazam servers, which compares the strongest peaks in a database with the peaks that we sent to determine the identity of the song. 



# Video Demo
Music audio in demo video is removed due to copyright

<iframe width="560" height="315" src="https://www.youtube.com/embed/8AjH50Y9S9k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


