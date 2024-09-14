# React Notes
## How to install react through cmd line
- npx create-react-app filename
- npm create vite@latest

#### React/ReactDom (ye dono library kam aati hain web ke dom ko manipulation karne ke leye)

#### React (core foundational library hain jo ki sare references lene ka kam karti hain)

#### ReactDom (react ka implimantain hain web pr)

## How react work
```javascript
    index.js
      import React from 'react';
      import ReactDOM from 'react-dom/client';
      import App from './App';

      ReactDOM.createRoot(document.getElementById('root')).render(<App />);

      {  }  => javaScript evaluate expression (matlab mein khali final outcome likhunga}
```

##  useState hook
```javascript
      let [ variablename, func ] = useState(parameter like number,array,string,{})
      let [ counter, setCounter ] = useState(0)
      function khta hain mein ui mein change karunga
      setCounter((prevCounter)=> preCounter + 1)
      setCounter((prevCounter)=> preCounter + 1)
```

        - react fiber [https://github.com/acdlite/react-fiber-architecture] difing algorithm

## props function ko property pass karna

- inline style in React style={{backgroundColor:color}}

- onClick = {function(){}}  onclick ko pura function chiye jo function se return aa rha hain wo nahi chiye ,  (setColor() ese likne ka matlab jo function return value kar rha use le rhe hain)

- const passwordGenerator = useCallback(fn,dependencies array)  mere pas ek function hain use memory mein rakh lo agar mein use dubara se run karu to jitna part use hota hain use use kar lo jo nahi ho pa raha wo nahi ho pa rha

- usecallback() hook function ko run karne ke leye responsible nahi hain ye usko memoisation kara hain usko optimize karta hain use cachememory mein rakhta hain

- useEffect(callback,dependency array)  agar dependience mein kuch bhi ched char hui to mein dubara se run ho jaunga
- useref() kisi bhi cheze ka reference lena hota hain tab useRef() hook use karte hain
  ```javascript
    const passwordRef = useRef(null)
    agrument ref={passwordRef}
    window.navigator.clipboard.writeText(password)
    passwordRef.current?.select()
    passwordRef.current?.setSelectionRange(0,5)
  ```
- Remember the key in loops in react (agr aap react mein loop kar rahe ho to key ko use karna hi padega
- useId() hook use for uniqueId
```javascript
  const amountInputId = useId()
```

