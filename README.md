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

   <Link to="/" >
      Logo
   </Link>
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
## Context API

- Method 1:
```javascript
-> src/context
-> context/User.js

- step 1 : Making context
> context/User.js

    import React from 'react'

    const UserContext = React.createContext()

    export default UserContext

- step 2 : Making context Provider
> context/UserContextProvider.jsx

import React from 'react'
import UserContext from './UserContext'


const UserContextProvider = ({children}) => {
    const [user, setUser] = React.useState(null)
    return(
    <UserContext.Provider value={{user,setUser}}>
     {children}
    </UserContext.Provider>
    )
}

export default UserContextProvider

- step 3: wrap component
> App.jsx

import Login from './components/Login'
import Profile from './components/Profile'
import UserContextProvider from './context/UserContextProvider'

function App() {

  return (
    <UserContextProvider>
      <Login />
      <Profile />
    </UserContextProvider>
  )
}

export default App

- step 4: setData

import React,{useState,useContext} from 'react'
import UserContext from '../context/UserContext'

function Login() {
    const [username,setUsername] = useState("")
    const [password, setPassword] = useState("")

    const {setUser} = useContext(UserContext)

    const handleSubmit = (e) =>{
        e.preventDefault();
        setUser({username,password})
    }
  return (
    <div>
        <h2>Login</h2>
        <input 
        type='text' 
        placeholder='username'
        value={username}
        onChange={(e) => setUsername(e.target.value)}
        />
        <button 
        onClick={handleSubmit}>Submit
        </button>
    </div>
  )
}

export default Login

- step 5: read data

import {React, useContext} from "react"
import UserContext from "../context/UserContext"


function Profile() {
    const {user} = useContext(UserContext)

    if(!user) return <div>Please login</div>
    return <div>Welcome {user.username}</div>
}

export default Profile
```
- Method 2:
```javascript

- setp 1
>src/context/theme.js
import { createContext, useContext } from "react";

export const ThemeContext = createContext({
    themeMode: "light",
    darkTheme: () => {},
    lightTheme: () => {},
})

export const ThemeProvider = ThemeContext.Provider


export default function useTheme(){
    return useContext(ThemeContext )
}

- step 2

import React from 'react'
import useTheme from '../contexts/theme';

export default function ThemeBtn() {
    
    const {themeMode, lightTheme, darkTheme} = useTheme()
    const onChangeBtn = (e) => {
        const darkModeStatus = e.currentTarget.checked
        if (darkModeStatus) {
            darkTheme()
        } else {
            lightTheme()
        }
    }
    return (
        <label className="relative inline-flex items-center cursor-pointer">
            <input
                type="checkbox"
                value=""
                className="sr-only peer"
                onChange={onChangeBtn}
            />
        </label>
    );
}

- step 3:

import { ThemeProvider } from './contexts/theme'

function App() {
  const [themeMod, setThemeMod] = useState("light")

  const lightTheme = () =>{
    setThemeMod("light")
  }

  const darkTheme = () =>{
    setThemeMod("dark")
  }

  useEffect(()=>{
    document.querySelector('html').classList.remove("light","dark")
    document.querySelector('html').classList.add(themeMod)
  },[themeMod])

  return (
    <ThemeProvider value={{themeMod,lightTheme,darkTheme}}>
           { <ThemeBtn/>}
           { <Card />}
    </ThemeProvider>
  )
}

export default App
```
## Local storage
```javascript
  useEffect(() => {
    const todos = JSON.parse(localStorage.getItem("todos"))

     if(todos && todos.length>0){
      setTodos(todos)
     }
  }, [])

  useEffect(() =>{
     localStorage.setItem("todos",JSON.stringify(todos))
  },[todos])
```
## react
```react
import React from "react";
import styled from "styled-components";

const Card = () => {
  return (
    <StyledWrapper>
      <div className="card">
        <div className="wrap">
          <div className="terminal">
            <hgroup className="head">
              <p className="title">
                <svg
                  width="16px"
                  height="16px"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  strokeLinejoin="round"
                  strokeLinecap="round"
                  strokeWidth={2}
                  stroke="currentColor"
                  fill="none"
                >
                  <path d="M7 15L10 12L7 9M13 15H17M7.8 21H16.2C17.8802 21 18.7202 21 19.362 20.673C19.9265 20.3854 20.3854 19.9265 20.673 19.362C21 18.7202 21 17.8802 21 16.2V7.8C21 6.11984 21 5.27976 20.673 4.63803C20.3854 4.07354 19.9265 3.6146 19.362 3.32698C18.7202 3 17.8802 3 16.2 3H7.8C6.11984 3 5.27976 3 4.63803 3.32698C4.07354 3.6146 3.6146 4.07354 3.32698 4.63803C3 5.27976 3 6.11984 3 7.8V16.2C3 17.8802 3 18.7202 3.32698 19.362C3.6146 19.9265 4.07354 20.3854 4.63803 20.673C5.27976 21 6.11984 21 7.8 21Z" />
                </svg>
                Terminal
              </p>

              <button className="copy_toggle" tabIndex={-1} type="button">
                <svg
                  width="16px"
                  height="16px"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  strokeLinejoin="round"
                  strokeLinecap="round"
                  strokeWidth={2}
                  stroke="currentColor"
                  fill="none"
                >
                  <path d="M9 5h-2a2 2 0 0 0 -2 2v12a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2v-12a2 2 0 0 0 -2 -2h-2" />
                  <path d="M9 3m0 2a2 2 0 0 1 2 -2h2a2 2 0 0 1 2 2v0a2 2 0 0 1 -2 2h-2a2 2 0 0 1 -2 -2z" />
                </svg>
              </button>
            </hgroup>

            <div className="body">
              <pre className="pre">
                {" "}
                <code>-&nbsp;</code>
                <code>npx&nbsp;</code>
                <code className="cmd" data-cmd="create-react-app@latest" />
              </pre>
            </div>
          </div>
        </div>
      </div>
    </StyledWrapper>
  );
};

const StyledWrapper = styled.div`
  .card {
  padding: 1rem;
  overflow: hidden;
  border: 1px solid #c5c5c5;
  border-radius: 12px;
  background-color: #d9d9d92f;
  backdrop-filter: blur(8px);
  min-width: 344px;
}
.wrap {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  position: relative;
  z-index: 10;
  border: 0.5px solid #525252;
  border-radius: 8px;
  overflow: hidden;
}
.terminal {
  display: flex;
  flex-direction: column;

  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas,
    "Liberation Mono", "Courier New", monospace;
}
.head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  overflow: hidden;
  min-height: 40px;
  padding-inline: 12px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
  background-color: #202425;
}
.title {
  display: flex;
  align-items: center;
  gap: 8px;
  height: 2.5rem;
  user-select: none;
  font-weight: 600;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: #8e8e8e;
}
.title > svg {
  height: 18px;
  width: 18px;
  margin-top: 2px;
  color: #006adc;
}
.copy_toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.25rem;
  border: 0.65px solid #c1c2c5;
  margin-left: auto;
  border-radius: 6px;
  background-color: #202425;
  color: #8e8e8e;
  cursor: pointer;
}
.copy_toggle > svg {
  width: 20px;
  height: 20px;
}
.copy_toggle:active > svg > path,
.copy_toggle:focus-within > svg > path {
  animation: clipboard-check 500ms linear forwards;
}
.body {
  display: flex;
  flex-direction: column;
  position: relative;
  border-bottom-right-radius: 8px;
  border-bottom-left-radius: 8px;
  overflow-x: auto;
  padding: 1rem;
  line-height: 19px;
  color: white;
  background-color: black;
  white-space: nowrap;
}
.pre {
  display: flex;
  flex-direction: row;
  align-items: center;
  text-wrap: nowrap;
  white-space: pre;
  background-color: transparent;
  overflow: hidden;
  box-sizing: border-box;
  font-size: 16px;
}
.pre code:nth-child(1) {
  color: #575757;
}
.pre code:nth-child(2) {
  color: #e34ba9;
}
.cmd {
  height: 19px;
  position: relative;
  display: flex;
  align-items: center;
  flex-direction: row;
}
.cmd::before {
  content: attr(data-cmd);
  position: relative;
  display: block;
  white-space: nowrap;
  overflow: hidden;
  background-color: transparent;
  animation: inputs 8s steps(22) infinite;
}
.cmd::after {
  content: "";
  position: relative;
  display: block;
  height: 100%;
  overflow: hidden;
  background-color: transparent;
  border-right: 0.15em solid #e34ba9;
  animation: cursor 0.5s step-end infinite alternate, blinking 0.5s infinite;
}

@keyframes blinking {
  20%,
  80% {
    transform: scaleY(1);
  }
  50% {
    transform: scaleY(0);
  }
}
@keyframes cursor {
  50% {
    border-right-color: transparent;
  }
}
@keyframes inputs {
  0%,
  100% {
    width: 0;
  }
  10%,
  90% {
    width: 58px;
  }
  30%,
  70% {
    width: 215px;
    max-width: max-content;
  }
}
@keyframes clipboard-check {
  100% {
    color: #fff;
    d: path(
      "M 9 5 H 7 a 2 2 0 0 0 -2 2 v 12 a 2 2 0 0 0 2 2 h 10 a 2 2 0 0 0 2 -2 V 7 a 2 2 0 0 0 -2 -2 h -2 M 9 5 a 2 2 0 0 0 2 2 h 2 a 2 2 0 0 0 2 -2 M 9 5 a 2 2 0 0 1 2 -2 h 2 a 2 2 0 0 1 2 2 m -6 9 l 2 2 l 4 -4"
    );
  }
}

`;

export default Card;
```




