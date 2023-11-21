This is to help generate the summary of youtube videos.
### Features: 
1. Generates summary on a simple click within 1-2 minutes.
2. Timestamps are present with summary which directs to that part on clicking the timestamp.

The basic strategy it uses is using ML summarizing techniques on the transcript of the video.

The project is divided in two separate entities: 
1. client which is a chrome extension
2. server which process the request and sends it back to the client as a HTTP response.

The above structure is taken owing to implementation of Restful services.


### Server:
It is a simple flask app, which has a API /api/summarize?youtube_video='url' which can be used to get the summary of a
desired youtube video by simply making a GET XML HTTP request.

### Client:
It is a chrome extension which will render the summary of the youtube video below the youtube video player by making
use of the above API. Just click on summarize button and it will show the summary of the youtube video.

## Implementation Details:
### Server
The server side is implemented using flask as a restful service.
The summarization is done by first generating the transcript of the video
for which if the video has already transcript then it is used with the help of a python
library *youtube-transcript-api* ,otherwise first the audio is taken and speech to text transformation is done.
Again useful python libraries for used for this.

After this, the summary can be generated using the transformers, there are two ways to do this
1. Extractive summarization
2. Abstractive summarization

The length of the transcript short by applying *extractive summarization* with *bert model* and then the *T5 model* is used .

Here, currently sumy with LSA summarizer is used.

The summary is then given back as a HTTP response after one gives a GET HTTP request on */api/summarize?youtube_video="a valid url"*.

To server the request over HTTPS (as the youtube is a https website and generating a HTTP request to a http website will give a mixed content error), the app needs be to run on https rather than http, for this there can be two solutions -

1. A easy option is to use pyngrok (a wrapper library for ngrok), which allows to expose local host on a public url.
2. Another hard option is to deploy it on a host which will give the required https url and certificates.

