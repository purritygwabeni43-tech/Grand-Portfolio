# Purrity Gwabeni — Portfolio City

A GTA-inspired, interactive portfolio city built with Vue 3 and Vite. Visitors can explore the
About, Projects, Certifications, and Contact sections by selecting buildings on the city map.

## Personalizing the portfolio

Edit the `profile`, `projects`, and `certificates` data near the top of
[`src/views/HomeView.vue`](./src/views/HomeView.vue) to add your bio, real project links and details,
certificate names and issuers, location, and contact email. The contact form submits through
Formspree, and the direct email link is configured alongside it. The loading screen and in-world character use
[`src/assets/purrity-art.png`](./src/assets/purrity-art.png).

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
- Firefox:
  - [Vue.js devtools](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
  - [Turn on Custom Object Formatter in Firefox DevTools](https://fxdx.dev/firefox-devtools-custom-object-formatters/)

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
