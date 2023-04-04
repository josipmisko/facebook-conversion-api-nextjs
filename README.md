Facebook Conversion API for Next.js

> Next.js wrapper for [Facebook's Conversion API](https://developers.facebook.com/docs/marketing-api/conversions-api/)

# Facebook Conversion API for Next.js
This package helps you implement Facebook Conversion API in Next.js.

It includes an API route handler for sending server-side events to Facebook and client-side functions to trigger the events.

Facebook recommends sending events with Facebook Pixel and the Conversion API with the same event id to match duplicated events.

Therefore, we have added the option to enable standard Facebook Pixel events in this package, where we handle duplicated events out of the box.

Support Next.js API routes on both Vercel and AWS Amplify.

## Install

NPM
```bash
npm install @rivercode/facebook-conversion-api-nextjs
```

Yarn
```bash
yarn add @rivercode/facebook-conversion-api-nextjs
```

## 1. Create Next.js API Route
pages/api/fb-events.js
```jsx
import { fbEventsHandler } from '@rivercode/facebook-conversion-api-nextjs/handlers';

export default fbEventsHandler;
```

### Add Facebook Access Token and Pixel ID
.env
```dotenv
FB_ACCESS_TOKEN=accessToken
NEXT_PUBLIC_FB_PIXEL_ID=pixelId
NEXT_PUBLIC_FB_DEBUG=true # optional
```

Read more here on how you can get your [access token](https://developers.facebook.com/docs/marketing-api/conversions-api/get-started/#access-token) and [pixel id](https://www.facebook.com/business/help/952192354843755?id=1205376682832142).

## 2. Load Facebook Pixel (Optional)
This is only needed if you want to fire standard Pixel Events.

### Add Facebook Pixel Provider & Script
pages/_app.js
```jsx
import { FBPixelScript, FBPixelProvider } from '@rivercode/facebook-conversion-api-nextjs/components';

...
<>
  <FBPixelScript />
  <FBPixelProvider>
    <Component {...pageProps} />
  </FBPixelProvider>
</>
...
```

## 3. Start Sending Events
Trigger the events you need. For example, add to cart or purchase events.
```jsx
import { fbEvent } from '@rivercode/facebook-conversion-api-nextjs';

fbEvent({
  eventName: 'ViewContent', // ViewContent, AddToCart, InitiateCheckout or Purchase
  eventId: 'eventId', // optional, unique event id's will be generated by default
  emails: ['email1', 'email2'], // optional
  phones: ['phone1', 'phone2'], // optional
  firstName: 'firstName', // optional
  lastName: 'lastName', // optional
  country: 'country', // optional
  city: 'city', // optional
  products: [{
    sku: 'product123',
    quantity: 1,
  }],
  value: 1000, // optional
  currency: 'USD', // optional
  enableStandardPixel: false // default false (Require Facebook Pixel to be loaded, see step 2)
});
```
