# npx create-react-app filename
# npm create vite@latest
**React/ReactDom (ye dono library kam aati hain web ke dom ko manipulation karne ke leye)
*React (core foundational library hain jo ki sare references lene ka kam karti hain)
*ReactDom (react ka implimantain hain web pr)
    # react wrking
    index.js
      import React from 'react';
      import ReactDOM from 'react-dom/client';
      import App from './App';

      ReactDOM.createRoot(document.getElementById('root')).render(<App />);
