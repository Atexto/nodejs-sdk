# Atexto nodejs-sdk

### Install Atexto SDK

Do `npm i @atexto/atexto-sdk` on you nodejs aplication folder.

### Create a client

To generate your token key/secret go to https://devs.atext.io, create an account and then a token.

On your application add the following code to create a client. This client will allow you to create, list and get the detail for your transcriptions.

```js
const sdk = require('@atexto/atexto-sdk')
const client = new sdk.Client({
  key: KEY,
  secret: SECRET
})
```

### Create a transcription

Upload the file to the server, then run the following code to create a transcription job in Atexto.

```js
const transcript = await client.createTranscript({
  "audioSrcUrl": "AUDIO_URL",
  "tags": ["demo", "completed"],
  "speakerCount": 2,
  "languageCode": "en",
  "options": {
    "formatText": false,
    "truncatedWords": false
  }
})
```

This will return a `transcript` object with this data:

```json
{
    "uuid": "16a54748-da46-4080-b4a1-fa18a6b223cf",
    "status": "started",
    "audioSrcUrl": "AUDIO_URL",
    "language": "en",
    "tags": [
        "demo",
        "completed"
    ],
    "speakerCount": 2,
    "options": {
        "formatText": false,
        "truncatedWords": false
    },
    "createdAt": "2018-12-08T01:25:49.031Z"
}
```

All available options and parameters to create a transcription job are [here](https://docs.atext.io/article/155-post-transcripts)  in our API docs.

### Get a transcription details

Review a job status and get the transcription once completed, with the following code and the job UUID:

```js
const transcript = await client.getTranscript(UUID)
```

This will return a `transcript` object with the following data:

```json
{
    "uuid": "16a54748-da46-4080-b4a1-fa18a6b223cf",
    "status": "processing",
    "audioSrcUrl": "AUDIO_URL",
    "language": "en",
    "tags": [
        "demo",
        "completed"
    ],
    "speakerCount": 2,
    "options": {
        "formatText": false,
        "truncatedWords": false
    },
    "createdAt": "2018-12-08T01:25:49.031Z"
}
```

### List a transcriptions

To list all jobs generated with your account, use the following code:

```js
const transcripts = await client.listTranscripts()
```

This will return a `transcripts` array with the following data:

```json
{
    "total": 2,
    "data": [
        {
            "uuid": "UUID",
            "status": "processing",
            "audioSrcUrl": "AUDIO_URL",
            "language": "en",
            "tags": [
                "sample",
                "project-1"
            ],
            "createdAt": "2018-12-04T15:42:32.082Z"
        },
        {
            "uuid": "UUID",
            "status": "ready",
            "audioSrcUrl": "AUDIO_URL",
            "language": "en",
            "tags": [
                "sample",
                "second"
            ],
            "createdAt": "2018-12-04T15:47:11.576Z"
        },
    ]
}
```

For paginated requests use:

```js
const transcripts = await client.listTranscripts({start: 20})
```

This will skip the most recent 20 jobs.

### Filter a transcriptions

Filter jobs by status or tags, using the following code:

```js
const transcripts = await client.listTranscripts({status: 'ready'})
```

This will return a `transcripts` array with transcriptions in ready only.