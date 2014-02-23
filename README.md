#Google Speech API v2:

##Host:
https://www.google.com/speech-api/v2/recognize

###Parameters
**output:** json, xml not supported.

**lang:** any valid locale (en-us, nl-be, fr-fr, etc.)

**key:** AIzaSyCnl6MRydhw_5fLXIdASxkLJzcJh5iX0M4

(Key was extracted from the Google Hotword chrome extension and is **not optional**).

**app:** optional

You can specify an optional query string called ```app```, which returns some extra transcripts for some reason.

**client:** optional

Seems to do nothing in particular.

##Data:

###File used for testing:
Flac file; 44100Hz 32bit float, exported with Audacity. Check the audio folder in this repository for some hilarious examples.

###Headers:
**Content-Type:** 

```audio/x-flac; rate=44100;```

Set the rate to be equal to the rate of the FLAC file (generally 44100Hz) but it supports different rates.

```audio/l16``` is also supported with a rate of 44100Hz for files encoded with LPCM 16-bit (signed).

**User-Agent:**

not required, but for spoofing purposes use one of Chrome’s userAgent strings.

###Response:

When Google is 100% confident in it's translation, it will return the following object:

```JSON
{
   "result":[
      {
         "alternative":[
            {
               "transcript":"good morning Google how are you feeling today"
            }
         ],
         "final":true
      }
   ],
   "result_index":0
}
```

When it's doubtful, it adds a confidence parameter for you. It also seems to add multiple transcripts for some reason.

```JSON
{
  "result":[
    {
      "alternative":[
        {
          "transcript":"this is a test",
          "confidence":0.97321892
        },
        {
          "transcript":"this is a test for"
        }
      ],
      "final":true
    }
  ],
  "result_index":0
}
```

An example of the response object can also be found here:

[https://gist.github.com/gillesdemey/68f46db997064390e1bf/0afe58f46faf49aa3d440899844cb0d886479a8a](https://gist.github.com/gillesdemey/68f46db997064390e1bf/0afe58f46faf49aa3d440899844cb0d886479a8a)

Along with the diff between supplying the optional ```app``` query string parameter and the regular response.

[https://gist.github.com/gillesdemey/68f46db997064390e1bf/revisions](https://gist.github.com/gillesdemey/68f46db997064390e1bf/revisions)

##CURL request:

```bash
curl -X POST \
--data-binary @audio/good-morning-google.flac \
--user-agent 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.117 Safari/537.36' \
--header 'Content-Type: audio/x-flac; rate=44100;' \
'https://www.google.com/speech-api/v2/recognize?output=json&lang=en-us&key=AIzaSyCnl6MRydhw_5fLXIdASxkLJzcJh5iX0M4'
```
