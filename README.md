# fullstackopen2023part5
## Course Content
In this part we return to the frontend, first looking at different possibilities for testing the React code. We will also implement token based authentication which will enable users to log in to our application.

This project continues from the bloglist app that was done in _part 2, part 3 and part 4_. It adds Unit,Integration Tests and End-to-end Testing from the frontend (_part 2_) to the backend (_part 3/4_) using ```cypress```.
This repository was slowly build up across the various exercise for this part. Therefore, the current state of the repository is also the final state after all the exercises. Looking at the commit history may allow you to look through the code for the various parts.

For more information, click [here](https://fullstackopen.com/en/part5) 

### Login to the Frontend ( Exercise 5.1 - 5.4 ) 
A login page was created to enable user authentication that was previously created in the backend (_part 4_). By using ```jsonwebtoken```(JWT), a token was saved on the local storage. This then allows for 'permanent' login even after the browser was closed and re-opened. Following the login page, an interface was also added to allow for new blogs to be added. Finally, notification of success and failure of the login/new blog creation was also included. Each notification was made to last a few seconds.

While the local storage of token is not very secure and a server-side session would usually be preferred, this was used as an inital step to enable login authentication. Further on in the course, this bloglist would be upgraded to a server-side session [(_part13_)](https://github.com/xhello00o/fullstackopen2023part4/tree/part13relationDBbackend/blog_list).

### Props.children and proptypes ( Exercises 5.5 - 5.12 ) 
The course guides us through the use of ```useRef``` to toggle the visibility of certain components. However, in this project, the states of the login form is controlled in the parent ```App``` component and the ```setVisible``` is passed through the children through props. The ```useRef``` was not use due to its added complexities. 

the login form visibility is controlled by the ```user``` state, which correspond to the login status. If the user is logged in, the blogs would be shown. On the other hand, if the user is not logged in, the login form would be shown. A similar idea was also applied to that of the new blog entry form. The blog entry form would only appear if the button ```create``` is clicked. The state of the button would then be controlled by the parent as mentioned above. 

New functionalities of liking a blog, updating the blog and deleting the blog was also added.

### Testing React App ( Exercise 5.13 - 5.16 )
```Jest``` and ```@testing-library/react``` was used to test this frontend bloglist. The test file is found in [_\src\components\blog.test.js_](https://github.com/xhello00o/fullstackopen2023part5/blob/main/src/components/blog.test.js)
```node
npm install --save-dev @testing-library/react @testing-library/jest-dom jest-environment-jsdom @babel/preset-env @babel/preset-react
```
**Tests**
- Tests for Blog
  - renders the blog's title and author, but does not render its URL or number of likes by default
  - blog's URL and number of likes are shown when the button controlling the shown details has been clicked.
  - if the like button is clicked twice, the event handler the component received as props is called twice
- Tests for BlogForm
  - form calls the event handler it received as props with the right details when a new blog is created

### End to End Testing ( Exercises 5.17 to 5.23 )
```cypress``` is used for the end to end testing of the bloglist app. The tests can be found in [_\cypress\e2e\bloglist.cy.js](https://github.com/xhello00o/fullstackopen2023part5/blob/main/cypress/e2e/bloglist.cy.js)

```node
npm install --save-dev cypress
```

**Tests**
- Bloglist App
  - web opens successfully with correct default login page
  - Login Tests
    - succeed with correct login
    - failed with wrong login
  - When logged in
    - create a blog
    - like a blog
    - delete a blog
    - only show delete for blogs created by user
    - Blogs are sorted
   
#### Personal Note
Commands can be created in the commands folder under cypress, such as createBlog.

```javascript
Cypress.Commands.add('createBlog', ({ title, author, url, likes}) => {

  cy.request({
    method: 'POST',
    url: 'http://localhost:3003/api/blogs',
    body: { title, author, url,likes },
    headers: {
      Authorization: `Bearer ${JSON.parse(localStorage.getItem('loggedUser')).token
      }`,
    },
  })
})
```


