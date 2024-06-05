- [1. Vite Project](#1-vite-project)
- [2. Add Tailwind](#2-add-tailwind)


## 1. Vite Project

```sh
npm create vite@latest project-name -- --template react
npm i
```

## 2. Add Tailwind

```sh
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```js
content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
],
```