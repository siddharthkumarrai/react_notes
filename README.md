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
## Redux
> Installation
```javascript
npm install @reduxjs/toolkit
```
```javascript
npm install react-redux
```
step1:- Making store
- src/app/store.js
```javascript
import {configureStore} from '@reduxjs/toolkit'

export const store = configureStore({})
```
step2:- Making Reducers 
- src/features/todo/todoSlice.js
```javascript
import { createSlice, nanoid } from '@reduxjs/toolkit'

const initialState = {
    todos:[
        {
            id: 1,
            text: "hello world"
        }
    ]
}

export const todoSlice = createSlice({
    name: 'todo',
    initialState,
    reducers: {
        addTodo: (state, action) => {
            const todo = {
                id: nanoid(),
                text: action.payload
            }
            state.todos.push(todo)
        },
        removeTodo: (state, action) => {
            state.todos = state.todos.filter((todo) => todo.id !== action.payload)
        },
    }
})

export const {addTodo, removeTodo} = todoSlice.actions

export default todoSlice.reducer

- step3:- pass reducers to store
```javascript
import {configureStore} from '@reduxjs/toolkit'
import todoReducer from '../features/todo/todoSlice'

export const store = configureStore({
    reducer: todoReducer                ⬅️
})
```
- step4:- dispatched the value in store
> AddTodo.jsx
```javascript

import { useDispatch } from 'react-redux'                ⚠️
import {addTodo} from '../features/todo/todoSlice'       ⚠️

function AddTodo() {

    const [input, setInput] = useState('')
    const dispatch = useDispatch()                        ⚠️

    const addTodoHandler = (e) => {
        e.preventDefault()
        dispatch(addTodo(input))                           ⚠️
        setInput('')
    }


  return (
    <form onSubmit={addTodo} >
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
      />
      <button
        type="submit"
      >
        Add Todo
      </button>
    </form>
  )
}

export default AddTodo
```
- step5:- Select the value in store
> TodoList.jsx
```javascript

import { useSelector, useDispatch } from 'react-redux'            ⚠️
import { removeTodo } from '../features/todo/todoSlice'           ⚠️

function Todo() {
    const todos = useSelector( state=> state.todos)               ⚠️
    const dispatch = useDispatch()
    
  return (
    <>
    <div>Todos</div>
    <ul className="list-none">
        {todos.map((todo) => (
          <li
            key={todo.id}
          >
            <div>{todo.text}</div>
            {console.log(todo.text)}
            <button
             onClick={() => dispatch(removeTodo(todo.id))}
            >
            </button>
          </li>
        ))}
      </ul>
    </>
  )
}

export default Todo
```


