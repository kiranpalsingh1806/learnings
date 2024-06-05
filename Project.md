- [1. Vite Project](#1-vite-project)
- [2. Add Tailwind](#2-add-tailwind)
- [3. React Toastify](#3-react-toastify)


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

## 3. React Toastify

```js
"react-toastify": "^10.0.5",
```

```js
import { ToastContainer } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';
<ToastContainer />
```

```js

```