# AlphaBetchaGotcha! — two-phone multiplayer prototype

A deployable Node.js + Socket.IO web app for Android and iPhone. Includes private 6-character room codes, shareable links, synchronized turn state, scoring, categories, challenges, final recall, reconnect tokens, and alphabet-aware speech entry without saying “next”.

## Local test

Install Node.js 20+ and run:

```bash
npm install
npm start
```

Open `http://localhost:3000` in two browser tabs. For testing on real phones, deploy to an HTTPS host first (mobile microphone permission generally requires HTTPS).

## Deploy (example: Render)

1. Create a GitHub repository containing these files, preserving `public/index.html`.
2. At Render, create a **Web Service** from the repository. Use Node runtime, build command `npm install`, start command `npm start`.
3. Once deployed, open the Render URL on both phones and test a room.
4. Add the custom domain `game.safetycityny.com` in the Render service settings. At your domain registrar/DNS provider, create the DNS record Render instructs you to create. Wait for HTTPS certificate issuance. **Do not change existing safetycityny.com website records.**

## Important prototype limitations

- Rooms are stored in server memory. Restarting the server resets active games; multiple server instances will not share state. Use a shared database (e.g., Redis/Postgres) for production reliability and durable sessions.
- The room link is a convenience secret; this is not an authenticated account system. Use random codes and share only with intended players.
- Voice recognition uses the browser SpeechRecognition API where available. Availability varies on iPhone Safari and Android Chrome. When unavailable, users can use their keyboard dictation mic. Reliable cross-browser voice entry requires an HTTPS speech-to-text service and a backend endpoint, with appropriate privacy handling and consent.
- Alphabet-aware grouping is heuristic: “Alligator Brown Bear Cat Dog” usually maps A, B, C, D. Phrases containing words that begin with the next letter can split incorrectly; users can correct fields before submitting. Longer-term, speech timing and confidence or a more capable transcription parser would help.
- Category answers are judged by the other player; the server verifies the letter and exact recall, not category correctness.
- A successful A answer earns +1 and has no recall penalty. The opponent accepts/challenges subsequent answers. Final recall is +5.
- This is a prototype, not yet a hardened public game service. Add rate limits, abuse protections, persistent storage, and automated tests before broader release.
