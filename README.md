# Crashlander
---

Crashlander is a javascript/svelte project for learning the basics of Svelte.
It was converted from a VIC-20 Basic program made in 1983 which again was converted to a Turbo Pascal program in 1989.

The app itself is not great by any means, it was never radical, but it serve the purpose to learn a new framework and as a start to utilize different functions.
Not wanting to write another "Hello World" app, converting older programs is OK if you want to learn new stuff.
Originated from the 80's, it has only half-hearted been converted with mobile and tablet in mind.

Feel free to modify and use the code as you wish. Learn by **doing and experimenting**.

A working demo is located at [Crashlander](https://crashlander.netlify.app).
---

## Get started

Clone this repo or download the zip.
Install the dependencies.

```bash
cd crashlander
npm install
```

then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see the app running. Edit the App or a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.

## Building and running in production mode

To create an optimised version of the app:

```bash
npm run build
```

You can run the newly built app with `npm run start`. This uses [sirv](https://github.com/lukeed/sirv), which is included in your package.json's `dependencies` so that the app will work when you deploy to platforms like [Netlify](https://www.netlify.com/) or other similar platforms.
