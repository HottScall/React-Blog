<h1 align="centre">Outline</h1>

A simple web application similar to blog posts or twitter, built around 2 main components; The Blog List and the Header (Author).

The application uses React, Redux and API's. Details of the dependancies below.

<h2 align="centre">Dependancies</h2>

Redux = The Redux Library
React-Redux = Integration layer between React & Redux
Axios = Allows me to make network requests to external API's
Redux-Thunk - Middleware to help make requests in a redux application

<h2 align="centre"> Basic App Set Up </h2>

- Clear out all original boiler plate information
- Add main index.js. Import relevant dependancies and add provider tags for create store.
- Add Reducers and Components folders with relevant files (App.js and Index.js). Add empty boiler template into index.js (reducers) to clear out any error messages regarding no reducers.
- Added a dummy reducer to clear an error message within the console, for an example of this see this commit https://github.com/HottScall/React-Blog/commit/8c1258cc41a6423d182a3304b6235e8c0fb963ea
- Create the PostList file and a class based PostList component, remember to import the PostList component to the App.js file and replace the App text with the new PostList component

<h2 align="centre"> General Data Loading with Redux </h2>

_These 3 are generally responsible for fetching data they need by calling an Action Creator_

- Component get rendered to the screen
- Components componentDidMount lifecycle method gets called
- We call an action creator from componentDidMount

_These 3 are responsible from making API requests... Where Redux-Thunk comes into play_

- Action creator runs code to make an API request
- API responds with data
- Action Creator returns an action on the Action Creator with the payload property

_We then receive the fetched data into a component by generating new state in our Redux Store, then passing that into our component through mapStateToProps_

- Some Reducer sees the action and returns the data off the payload
- Because we generated some new state object, redux/react-redux will cause the app the be rerendered.

<h2 align="centre"> Adding an Action Creator </h2>

To recap this, view the lesson 161 - How to fetch data in a Redux app.

- Create a new folder inside the src folder called Actions with an index.js file. Within that add an export const fetchPosts and return an action with a type of "type" and a payload which is set to a string of say "FETCH_POSTS"
- Within PostList.js import the connect function and fetchPosts. Then now you can change the export default to a connect function for PostList, initially because we have no state pass in null and then add the action creator as a 2nd argument {fetchPosts}
- Now above the render method you can add the componentDidMount method and call this.props.fetchPosts

<h2 align="centre"> Making a request from an Action Creator</h2>

- Create a new folder for your api's called apis and add a js file inside of that (maybe name it the name of the api you're using).
- Import axios from axios then add default export axios.create({baseURL: "http://blahblahblah"}) and inside of there add a baseURL with the base url as a string
- Now inside actions/index.js you can import your api from "./apis/fileName". Convert your function into an async function by add async before the () and then add your await along with the file name and the end path in parenthesis e.g apiFileName.get("/posts")
- Assign the await command to a variable called response, then add response to payload: within your return function.

_Note_ At this point you may see an error message on your application "Actions must be plain objects. Use custom middleware for async actions" This is where Redux-Thunk comes in to play

<h2 align="centre">Understanding Async Action Creators</h2>

Q: What's wrong with fetchPosts?
A: 2 things:

- Action Creators must return plain JS objects with a type property (which we're not).
- By the time our action gets to a reducer we won't have fetched the data.

_Action Creators must return plain JS objects with a type property (which we're not)._

Whilst it looks like the return function in actions/index.js is returning JS objects, it's not. Remember, React is a framework that compiles your code from the > ES2016 code we write into ES2015 code.

Unsure on this? Check out Lesson 164 in Modern React with Redux on Udemy.

https://www.udemy.com/course/react-redux/learn/lecture/12586860#content

Example flow diagram of what's happening:

- PostList.js has a a componentDidMount function which requests this.props.fetchPosts().
- What's likely happening in Redux is the store.dispatch is looking to collect fetchPosts()
- In actions/index.js you have the following function:

  export const fetchPosts = async()=>{
  return jsonPlaceholder.get("/posts")
  }

  return{type: "FETCH_POSTS": payload: response}

- When this code is compiled to ES2015 it gets compiled like a switch statement with cases, the store.dispatch is essentially looking for the first return statement, which is the request. Here is what it looks like once it's compiled.

export const fetchPosts = async()=>{
CASE: 0
return jsonPlaceholder.get("/posts")
}
CASE: 1
return{type: "FETCH_POSTS": payload: response}

So the first case is actually the request, not the type and payload, hence the error message.

If you're confused about this transfer the code into babel.io and watch it compile it and you will see.

This is mainly down the fact we're using async awaits for an api along with Redux.

<h2 align="centre">More on Async Awaits Action Creators</h2>

_By the time our action gets to a reducer we won't have fetched the data._

A reminder of our Redux Cycle -

- To change the state of our application we call an ACTION CREATOR
- This produces an ACTION
- Which gets fed to DISPATCH
- Which forwards the ACTION to a REDUCER
- That then creates a new STATE

Keep in mind that when we complete the entire redux cycle to update state this happens in a fraction of a millisecond, but where we are using an API we also have to make another request at the same time over to the API.
The API request may take longer to retrieve the data from the API than the Redux Cycle does to process it's entire flow.
This will result in the Redux Cycle believing there is no data to return which will cause an error "Hey, I've processed this but there's no data here"
There is no way to delay the timing of the Redux cycle, hence the reason we can't replace the async/awaits and use promises to return our data instead.

<h2 align="centre">Middlewares in Redux</h2>

- Synchronous Action Creators = Instantly returns an action with data ready to go
- Asynchronous Action Creators = Takes some amount of time to get the data and return it (anything that makes a network request will be an Async Action Creator and you have to install a middleware such as Redux-Thunk).

Understanding middleware in our Redux Cycle:

- To change the state of our application we call an ACTION CREATOR
- This produces an ACTION
- Which gets fed to DISPATCH
  - DISPATCH forwards the ACTION to a MIDDLEWARE
- Which then sends that ACTION to a REDUCER
- That then creates a new STATE

What is a middleware?

- A plain JS function that gets called with every action that we dispatch
- It has the ability to STOP, MODIFY and otherwise mess around with ACTIONS
- There are lots of Open Source Middlewares that can be used
- The most popular use of middleware is for dealing with async actions
- We use Redux-Thunk as a middleware in these applications
- A Redux Library can use multiple different middlewares on a single project

<h2 align="centre">Behind the scenes with Redux-Thunk</h2>

Normal rules of a a Redux Application:

- Action Creators must return an Action Object
- Actions must have property types
- Actions can "optionally have a payload"

Rules of a Redux Application with middleware:

- Action Creators can return an Action Object OR return functions
- If an Action Object gets returned, it must have a type
- If an Action Object gets returned, it can have an optional payload

To understand a little more about how Redux Thunk works as a flow, look at the Redux Thunk Flow picture in private folder.

_Important to note_

If your action is returning a function as opposed to an object then the function that you call MUST take 2 separate function arguments of:

dispatch - Which we can pass through various actions which are processed and then forwarded on to our Reducers. Ultimately provides unlimited power to make changes to the Redux side of our application.

getState - This can be called on the Redux Store which will return all of the data/state inside the the store.

Now using Redux Thunk we can manually disptach an action, ultimately meaning that we don't have the overload previously mentioned where the Redux Store request will be completed before your API returns data, thus preventing error messages around "no data".

For more information on Redux Thunk, see here: https://github.com/reduxjs/redux-thunk

To get a reminder of what the function you need to use Redux Thunk then click in src folder and then into the index.js file which contains the function required for Redux Thunk.

<h2 align="centre">Shortened Syntx with Redux Thunk</h2>

If you've not installed Redux-Thunk into your project by now, do it. Once you have installed into your project you also have to wire it up to the Redux Store.

root/index.js

- install thunk from "redux-think"
- add a second argument to the redux import, next to createStore called applyMiddleware
- Remove createStore from the Provider tag and assign it a variable above your render method, then call that variable in the provider tag
- Now call the applyMiddleware within your createStore, next to the provider and pass in thunk.

action/index.js

- You can remove both the async and awaits and replace the variable "response" to promise. We will be replacing this back shortly.
- You now want to return an inner function which takes the dispatch and getState arguments and wraps the remainder of the function inside. You should now be returning both a function with dispatch and getState and another return statement which a type and a payload. You don't need 2 x return statements.
- Now replace that return statement with dispatch, which take the type and the payload.
- Add the async/awaits back into the return function.
- Refactored the final function. See this commit for the last piece.

You can see the final changes to the component here:

https://github.com/HottScall/React-Blog/commit/94c6b1e7d168820e7eda50c775972ab69d224cb5

<h2 align="centre">Rules of Reducers</h2>

When using Reducers it makes sense to create a new file for each Reducers and then imports them into the reducers/index.js file.

So as a recap:

- We have an action creator which will _fetchPosts_
- Our actions has a type: FETCH_POSTS and a payload: response
- Now we'll create a Reducer which collects postReducers, this will be responsible for looking for actions which have a type of FETCH_POSTS and any time it see's this it will pull off the payload: response and add it into an array

reducers/

- Create a postsReducers.js file and add an export default statement which simply returns a string or 123 or anything you want.

reducers/index.js

- Import the postReducers files
- Remove the dummy data and then replace it with postReducers: postReducers

<h2 align="centre">Return values from Reducers</h2>

A Reducer:

- Must return any value other than "undefined"

  - _NOTE_ Earlier in the application we input some dummy data into the reducer to stop the error message, this is an example of that).
  - _REMEMBER_ Javascript functions must return something! A number, a string, an array, an object or null. If they do not return anything you will get an error returning undefined!

- They are pure and they must return either state or data that is being used inside your application using only previous state and the action

  - When you first start up your Redux application, each reducer will get automatically called 1 time.
  - The first time that Reducer gets called it's going to receive 2 arguments. The first argument will have a value of undefined. The second argument some action object.
  - The Reducer will then take the 2 initial argument and return some initial state value (E.G - State v1).
  - The 2nd time that the Reducer gets called it's going to return (State v1).

- Must not return reach 'out of itself' to decide what value to return

  - When a reducer gets called it should only look at 2 things, the previous value and the action object to decide what to return.
  - It shouldn't be making things like API requests for data.

- Must not mutate it's input state argument
  - This is quite a misleading rule,
  - Anytime we change the contents of an array it's referred to as a mutation

Below is the text from the Redux Github file around Reducers.

let hasChanged = false
// Set an intial value of state has changed to false

const nextState: StateFromReducersMapObject<typeof reducers> = {}
for (let i = 0; i < finalReducerKeys.length; i++) {
// The for loop will loop through all the different reducers you have
const key = finalReducerKeys[i]
const reducer = finalReducers[key]
const previousStateForKey = state[key]
// This is the previous state for the Reducers
const nextStateForKey = reducer(previousStateForKey, action)
// nSFK is the magic, it takes the previous state and runs the action we have provided
if (typeof nextStateForKey === 'undefined') {
const errorMessage = getUndefinedStateErrorMessage(key, action)
throw new Error(errorMessage)
}
// This is the check where Redux will look if your Reducer is returning undefined
nextState[key] = nextStateForKey
hasChanged = hasChanged || nextStateForKey !== previousStateForKey
}
// hasChanged will now check the values being returned. It wants to know if nextStateForKey and previousStateForKey are exactly the same in the memory
// After that check, if the 2 arrays being compared are the same then hasChanged will be false, otherwise it will be true.
hasChanged =
hasChanged || finalReducerKeys.length !== Object.keys(state).length
return hasChanged ? nextState : state
}
