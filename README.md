### tinyShazam

# by Brandon Alba and Brandon Huynh

# Project Overview
The purpose of this project is to create an song recognition program similar to Shazam. The user hears a song that they are unable to recognize, they can activate the CC3200 Launchpad to listen and identify this unknown song. The Lanuchpad will record for a duration of 5 seconds, and display the name and artist of the song on the OLED screen.

# Key Features
- FRAM 

    
# Discussion 
- The first large challenge encounter was having to store and send the data to AWS for processing using an HTTP POST. 

- The function to identify the song utilized the ShazamAPI developed by Marin-m. Essentially, the imported library used audio fingerprinting to generates a spectrogram of the sound, and maps out the frequency peaks from it. These frequency peaks are then sent to the Shazam servers, which compares the strongest peaks in a database with the peaks both that we sent to determine the identity of the song. 

- Our intitial plan was to create a lamba function that once triggered by an HTTP POST by our launchpad, would receieve and deciper the song data, and return the name and artist of the song. In the end, we decided to use the AWS LightSail server and created our own http website to handle the http posts and gets. 
