
#### RUN
```bash
npm run dev
```
#### BUILD
```bash
npm run build
```

### INSTALL
```bash
npm create vite@latest .

npm install
```

##### Minimal Folder Structure
- node_modules
- public
- src/Button/Button.jsx (example)
- src/main.jsx
- src/App.jsx
- index.html
- package.json
- package-lock.json
- vite.config.js

##### Minimal `index.html` Structure
```html
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React</title>
  </head>

  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

##### Minimal `main.jsx` Structure
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```
##### Minimal `App.jsx` Structure
```jsx
import React from 'react';

const App = () => {
  return <div>App React</div>;
};

export default App;
```

##### Minimal `ESLINT` Structure
```js
module.exports = {
  env: {
    browser: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true,
    },
  },
  settings: {
    react: {
      version: 'detect',
    },
  },
  plugins: ['react-refresh'],
  rules: {
    'react-refresh/only-export-components': 'off',
    'react/react-in-jsx-scope': 'off',
    'react/prop-types': 'off',
    'no-unsafe-finally': 'off',
    'no-unused-vars': 'off',
    'react/jsx-key': 'off',
  },
};
```


# My React knowledge
# [[React ⚛️]]