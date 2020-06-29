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

<h2 align="centre"> Adding an Action Creator </h2>

To recap this, view the lesson 161 - How to fetch data in a Redux app.

- Create a new folder inside the src folder called Actions with an index.js file. Within that add an export const fetchPosts and return an action with a type of "type" and a payload which is set to a string of say "FETCH_POSTS"
- Within PostList.js import the connect function and fetchPosts. Then now you can change the export default to a connect function for PostList, initially because we have no state pass in null and then add the action creator as a 2nd argument {fetchPosts}
- Now above the render method you can add the componentDidMount method and call this.props.fetchPosts
