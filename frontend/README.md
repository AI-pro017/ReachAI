# ReachAI Frontend

The Next.js site for [ReachAI](https://reachaiapp.online): the landing page, the free title form, checkout, live job status and the legal pages.

See the [main README](../README.md) for how the whole project fits together.

## Running it

```bash
npm install
npm run dev -- -p 3001
```

It needs a `.env.local` that points at the backend:

```env
NEXT_PUBLIC_BACKEND_URL=http://localhost:3000
NEXT_PUBLIC_RAZORPAY_KEY_ID=
```

## Pages

| Route | Page |
| --- | --- |
| `/` | Landing page with the free title form, pricing and FAQ |
| `/pay/[channelId]` | Checkout for the full metadata bundle |
| `/thank-you/[paymentId]` | Confirmation and live progress of the paid job |
| `/about`, `/contact` | About page and contact form |
| `/legal/privacy`, `/legal/terms`, `/legal/refund` | Legal pages |

`hooks/JobStatus.ts` polls the backend's `/status` endpoint to show progress while a job runs.
