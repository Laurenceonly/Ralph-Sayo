# Ralph Laurence Sayo · Portfolio

A developer portfolio showcasing selected web and mobile projects, technical experience, and networking coursework.

[View the live portfolio](https://ralph-portfolio-tau.vercel.app/)

## Selected work

| Project | Overview |
| --- | --- |
| [GraphiScan](https://graphiscan.vercel.app/) | A capstone project exploring CNN-based handwriting screening for dysgraphia research. |
| [Odyssey](https://odyssey-explorer-psi.vercel.app/) | A browser-based solar system explorer with guided discovery and interactive field guides. |

## Development

The site uses Nuxt, Vue, TypeScript, and custom CSS. It includes a responsive project gallery, system-aware color themes, and reduced-motion support.

```sh
npm ci
npm run dev
```

Run `npm run typecheck` to validate the Vue and TypeScript source. Run `npm run build` and then `npm run preview` to check the production build locally.

## Project structure

| Path | Contents |
| --- | --- |
| `app/app.vue` | Page shell, hero, theme controls, and selected work |
| `app/components/PortfolioDetails.vue` | Skills, credentials, experience, GitHub activity, and contact |
| `app/assets/css/main.css` | Design tokens, responsive layouts, and transitions |
| `public/` | Optimized imagery, fonts, and icons served by the site |

Inquiries can be sent through the contact form on the [live portfolio](https://ralph-portfolio-tau.vercel.app/#contact).
