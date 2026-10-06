# Rickroll Line

Call **+46766868793** and you'll hear "Never Gonna Give You Up".

> Always dial with the **+46** country code. Without it the call doesn't go through.

## How it works

```
Caller → 46elks phone number → plays rickroll.mp3 (hosted on GitHub Pages)
```

There's no server and no code. 46elks plays the MP3 as soon as someone calls.

## How I built it

### 1. Get a phone number

I registered a Swedish mobile number on [46elks](https://46elks.com), a Swedish telephony API. The number has to support `voice`.

### 2. Host the song

The number needs a public URL for the audio file, so:

1. Created this repo and committed `rickroll.mp3`
2. Turned on GitHub Pages (branch `main`, folder `/`)

The song is now served at:

```
https://walter-ship-it.github.io/rickroll-line/rickroll.mp3
```

### 3. Point the number at the song

Your API username and password are on the 46elks dashboard.

Get the number's ID (it starts with `n`):

```bash
curl -u API_USERNAME:API_PASSWORD https://api.46elks.com/a1/numbers
```

Set what happens when someone calls:

```bash
curl -X POST https://api.46elks.com/a1/numbers/NUMBER_ID \
  -u API_USERNAME:API_PASSWORD \
  --data-urlencode 'voice_start={"play":"https://walter-ship-it.github.io/rickroll-line/rickroll.mp3","skippable":false}'
```

- `voice_start` is the action 46elks runs when a call comes in.
- `play` streams the MP3 to the caller.
- `skippable: false` stops the caller from skipping the song with their keypad. The call hangs up when the song ends.

## Troubleshooting

- **The call doesn't ring or you hear nothing:** you didn't dial with +46. When I tested it this way, the 46elks call log was empty because the call never reached them.
- **Still nothing:** run the first `curl` command and check that `capabilities` for your number includes `voice`.

## Notes

- The number expires on 2026-10-24 unless it's renewed.
- Built at a hackathon with [Claude Code](https://claude.com/claude-code).
