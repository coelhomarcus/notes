# Install Tailwind v4

### Vite + TailwindCSS
```bash
npm create vite@latest

npm install

npm install tailwindcss @tailwindcss/vite
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