<div align="center">
  <img src="public/favicon.png" alt="" width="64" height="64">
  <h1>Ralph Laurence Sayo</h1>
  <p><strong>Full-stack web and mobile developer</strong></p>
  <p>
    <a href="https://ralph-portfolio-tau.vercel.app/">View portfolio</a>
    &nbsp;·&nbsp;
    <a href="https://ralph-portfolio-tau.vercel.app/#contact">Get in touch</a>
  </p>
</div>

## Selected work

| Project | Overview | Links |
| --- | --- | --- |
| **GraphiScan** | A capstone project that uses a convolutional neural network to screen handwriting for signs of dysgraphia, with student records and evaluator access. | [Live app](https://graphiscan.vercel.app/) · [Source](https://github.com/Laurenceonly/Graphiscan) |
| **Odyssey** | An interactive 3D solar system built with Three.js, featuring guided exploration and field guides. | [Play in browser](https://odyssey-explorer-psi.vercel.app/) · [Source](https://github.com/Laurenceonly/Odyssey-Space-Exploration-Game) |

## About the site

The portfolio is built with Nuxt 4, Vue 3, TypeScript, and custom CSS. It uses prerendered HTML, responsive imagery, locally hosted fonts, and light and dark themes that follow system preferences. Theme and interface transitions respect reduced-motion settings.

## Local development

Use Node.js 24 LTS and npm.

```sh
npm ci
npm run dev
```

| Command | Purpose |
| --- | --- |
| `npm run typecheck` | Validate Vue and TypeScript source |
| `npm run build` | Create the production build |
| `npm run preview` | Serve the production build locally |

## Repository layout

| Location | Contents |
| --- | --- |
| [`app/app.vue`](app/app.vue) | Intro, theme controls, and project gallery |
| [`app/components/PortfolioDetails.vue`](app/components/PortfolioDetails.vue) | Skills, credentials, experience, and contact |
| [`app/assets/css/main.css`](app/assets/css/main.css) | Design tokens, responsive layouts, and transitions |
| [`public/`](public/) | Published images, fonts, and icons |
| [`nuxt.config.ts`](nuxt.config.ts) | Prerendering, color mode, and asset caching |
