# ReachAI Backend

The event driven backend for [ReachAI](https://reachaiapp.online), built with [Motia](https://motia.dev).

Every step in `src/` is a small handler that either exposes an API endpoint or listens for an event, does one job and emits the next event. See the [main README](../README.md) for the full flow, the env variables and setup.

## Running it

```bash
npm install
npm run dev
```

This starts the API on port 3000 and the Motia Workbench, where you can see both flows and follow each event as it runs.

## Steps

**Free flow** (`src/freeUser`)

| Step | Trigger | Does |
| --- | --- | --- |
| `submit.step.ts` | `POST /submit` | Validates the channel and email and starts a job |
| `resolve-channel.step.ts` | `yt.submit` | Finds the YouTube channel |
| `fetch-videos.step.ts` | `yt.channel.resolved` | Gets the latest videos |
| `fetch-niche.step.ts` | `yt.videos.fetched` | Works out the channel's niche with AI |
| `trending-videos.step.ts` | `yt.niche.fetched` | Finds trending videos in that niche |
| `AI-generatedTitles.step.ts` | `yt.trendingVideos.fetched` | Writes two titles for each of the 5 latest videos |
| `send-email.step.ts` | `yt.AI-Title.fetched` | Emails the titles |
| `error-handling.step.ts` | any `*.error` event | Emails the user if something failed |
| `get-status.step.ts` | `GET /status` | Returns job progress |
| `contact.step.ts` | `POST /api/contact` | Sends contact form messages |

**Paid flow** (`src/paidUser`)

| Step | Trigger | Does |
| --- | --- | --- |
| `CreateOrder.step.ts` | `POST /api/payment/create-order` | Creates a Razorpay order |
| `checkPayment.step.ts` | `POST /api/payment/verify` | Verifies the payment signature and starts the job |
| `webhook.step.ts` | `POST /api/payment/webhook` | Starts the job from Razorpay's webhook |
| `fetchVideosPaid.step.ts` | `paidUser.payment.success` | Gets the 10 latest videos |
| `fetchNichePaid.step.ts` | `paidUser.videosfetched.success` | Works out the niche |
| `TrendingVidPaidUser.step.ts` | `paidUser.Nichefetched.success` | Finds trending videos |
| `AI-generatedMetadata.step.ts` | `paidUser.trendVid.success` | Writes titles, descriptions, tags and hashtags |
| `SendEmail-PaidUser.step.ts` | `paidUser.AImetadata.success` | Emails the full bundle |
| `paidUser-errorHandling.step.ts` | any paid `*.error` event | Emails the user if something failed |
| `retry-manual.step.ts` | `POST /api/jobs/:jobId/retry` | Restarts a failed paid job |
