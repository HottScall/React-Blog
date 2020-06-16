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

Whilst it looks like the return function in actions/index.js is returning JS objects, it's not. Remember, React is a framework that compiles your code from the > ES2016 code we write into ES2015 code which can't take types and payloads.

Unsure on this? Check out Lesson 164 in Modern React with Redux on Udemy.

https://www.udemy.com/course/react-redux/learn/lecture/12586860#content
