# Install Tailwind v4

### Vite + TailwindCSS
```bash
npm create vite@latest

npm install

npm install tailwindcss @tailwindcss/vite

# index.css
@import "tailwindcss";
```
create tailwind.config.js
### tailwind.config.js
```js
/ @type {import('tailwindcss').Config} */
export default {
   content: [
      "./index.html",
      "./src//*.{js,ts,jsx,tsx}",
   ],
   theme: {
      extend: {},
   },
   plugins: [],
}
```

(melhor compatibilidade com a extensao :P)
# Install Tailwind v3
```bash
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p

# index.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
