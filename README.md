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
- props function ko property pass karna
    - inline style in React style={{backgroundColor:color}}
    - onClick = {function(){}}  onclick ko pura function chiye jo function se return aa rha hain wo nahi chiye ,  (setColor() ese likne ka matlab jo function return value kar rha use le rhe hain)

## useCallback
```javascript
    const passwordGenerator = useCallback(fn,dependencies array) //  mere pas ek function hain use memory mein rakh lo agar mein use dubara se run karu to jitna part use hota hain use use kar lo jo nahi ho pa raha wo nahi ho pa rha

    usecallback( ) hook function ko run karne ke leye responsible nahi hain ye usko memoisation kara hain usko optimize karta hain use cachememory mein rakhta hain

    let passwordGenerate = useCallback(()=>{
    let pass = ""
    let str = ""
    if(includeUpperCase){
      str += "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    }
    for(let i=0; i<passwordLength; i++ ){
     let index = Math.floor( Math.random()*str.length+ 1)
     pass += str.charAt([index])
    }  
  },[passwordLength,includeLowerCase,includeUpperCase,includeNumber,includeSymbol])
```
## useEffect
```javascript
    useEffect(callback,dependency array)  agar dependience mein kuch bhi ched char hui to mein dubara se run ho jaunga

    useEffect(()=>{
        passwordGenerate()
    },[passwordLength,includeUpperCase,includeLowerCase,includeNumber,includeSymbol,setWholePassword])
```
## useref
- useref ()   kisi bhi cheze ka reference lena hota hain tab useRef() hook use karte hain
  ```javascript
    const passwordRef = useRef(null)
    <input
        type="text"
        value={wholePassword}
        ref={passwordRef}
        readOnly
        className="outline-none"
    />
    <button
      onClick={() => {
      passwordRef.current?.select();
      navigator.clipboard.writeText(wholePassword)
      }}
    >
      copy
    </button>
  ```
> [!IMPORTANT]
> Remember the key in loops in react (agr aap react mein loop kar rahe ho to key ko use karna hi padega

## useId
- useId() hook use for uniqueId
```javascript
  const amountInputId = useId()

        <label htmlFor={amountInputId}>
            label
        </label>

        <input htmlFor={amountInputId}
          type="number"
          placeholder="Amount"
        />
```
## React Router
> install
```javascript
npm i react-router-dom
```
```javascript
Link a tag ki jgha use karte hain , Link page reload hone se bachata hain

import {Link, NavLink} from 'react-router-dom'

  <NavLink
    to="/home"
    className={({isActive}) =>`block py-2 pr-4 pl-3 duration-200 ${isActive?"bg-slate-500":"bg-slate-50"}`}
  >
     Home
  </NavLink>
```
> src/Layout.jsx
```javascript
import {Outlet} from 'react-router-dom'

function Layout() {
  return (
    <>
    <Header />
    <Outlet />
    <Footer />
    </>
  )
}
```
> Main.jsx
```javascript
import {RouterProvider } from 'react-router-dom'

Method 1 : for creacting router
const router = createBrowserRouter([
     {
       path:"/",
       element: <Layout />,
       children: [
         {
           path:"",
           element: <Home />
         },
         {
           path:"about",
           element: <About />
         }
      ]
    }
 ])

Method 2 : for creacting router

const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path='/' element={<Layout/>} >
      <Route path='' element={<Home />}/>
    </Route>
  )
)

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <RouterProvider router={router}/>
  </React.StrictMode>,
)
```
> Extract value from url
```javascript
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path='/' element={<Layout/>} >
      <Route path='' element={<Home />}/>
      <Route path='user/:id' element={<User />} />
    </Route>
  )
)

- User.jsx
import React from 'react'
import {useParams} from 'react-router-dom'

function User() {
    const {id} = useParams()
  return (
    <div className=' text-center'>User: {id}</div>
  )
}
export default User
```
> api handle
- Method: 1
```javascript
import React,{useEffect, useState,} from 'react'

function Github() {
    const data = useLoaderData()
       const [data, setData] = useState([])
       useEffect(()=>{
         fetch("https://api.github.com/users/siddharthkumarrai")
         .then( res => res.json())
         .then( (data) => {
            console.log(data)
            setData(data)
      
         },)
       },[])

  return (
    <div>
        <h3>folowers : {data.followers} </h3>
    </div>    
  )
}

export default Github
```
- Method: 2
```javascript
- Github.jsx

import React,{useEffect, useState,} from 'react'
import { useLoaderData } from 'react-router-dom'

function Github() {
    const data = useLoaderData()

  return (
    <div>
        <h3>folowers : {data.followers} </h3>
    </div>    
  )
}
export default Github

export async function githubloader (){
    const res = await fetch("https://api.github.com/users/siddharthkumarrai")
    return res.json()
}

- Main.jsx

import Github, { githubloader } from './components/github/Github.jsx'

const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path='/' element={<Layout/>} >
      <Route path='' element={<Home />}/>
      <Route path='github' element={<Github />}
             loader={githubloader}
       />
    </Route>
  )
)
```

