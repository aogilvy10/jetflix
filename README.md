# JETFLIX a MERN Stack App
JETFLIX was my first group project for my Software Engineering Immersive course. We created a full-stack MERN application built with a Mongo database and React front-end.

### Project Members: 

- Andrew Oglivy: https://github.com/aoglivy10
- Jacqueline de Leeuw: https://github.com/jacquelinedeleeuw
- Justine Solano: https://github.com/justinesolano
- Oliver Lewis: https://github.com/olilewis1

### Deployed version

https://jetflixapp.herokuapp.com/

To explore the app, you can use the below login credentials:

Email: admin@email.com
Password: password

![image](https://user-images.githubusercontent.com/68297258/117764874-00424380-b1e2-11eb-8375-9d1e6ff2966e.png)


### Code Installation

- Clone or download Repo
- Install dependencies yarn 
- Start the database `mongod --dbpath ~/data/db` 
- Change Directory to backend `nodemon`
- Change Directory to client `yarn start`



### Brief & Timeframe

- Build a full-stack application by making your own backend and your own front-end
- Use an Express API to serve your data from a Mongo database
- Consume your API with a separate front-end built with React
- Be a complete product which most likely means multiple relationships and CRUD functionality for at least a couple of models
- Implement thoughtful user stories/wireframes that are significant enough to help you know which features are core MVP and which you can cut
- Have a visually impressive design
- Be deployed online so it's publicly accessible
- Timeframe: 8 days

### Technologies Used: 

**Frontend:**
- React
- Axios
- Semantic UI
- Bulma
- SASS
- HTTP-Proxy-Middleware
- React Router DOM

**Backend:**
- Node
- MongoDB
- Mongoose
- Express
- Bcrypt
- Json web token

**Development Tools:**
- Git & Github
- VS Code
- Insomnia
- Yarn
- Cloudinary
- Google Chrome Dev tools
- Trello Board 

## Functionality

Users can:
- Register & Login
- Add destinations to their list for easy access
- Add a photo to profile
- Drag a photo from computer and upload
- Comment and like other users profiles
- View all users posts and profiles
- Search for certain tags or locations for a destination

Our group was picked at random and once we came together I initially had an idea of what to do for the project before hearing my groups idea. Luckily everyone liked the idea and thus we created JETFLIX. JETFLIX is a travel destination aaplication that allows you to register and add your favorite destinations to your profile, making them easily accessable for you. Each destination has an about snippet with key tags giving you an idea of the location.


### Getting Started (Day 1 & 2)
 We started by creating a wireframe on one of my partners whiteboards and sketching out each page we wanted throughout or application. We then created a Trello board where each member was able to access our process along the way. 
 ![image](https://user-images.githubusercontent.com/68297258/119444224-bbd59e00-bcdf-11eb-9174-d49b28ecc768.png)


 ![](2021-05-10-14-30-04.png)

 Backend: My group and I decided since it was all of our first times creating a backend using mongoDB, we would do it together to speak through the process and answer one anothers questions. The first step was creating the database and the models. For our models we decided on creating two main models, one for destinations and one for the User. I was in control of creating the seeds files for our destinations which I gathered from lonelyplanet website as the API was not available.  


### Front-end (Day 3 - 8)

 When it came to the front end we split everything evenly and got on our way. We decided it would be best if each member was a Full Stack Engineer and take full ownership of their area of responsibility. Some components were more challenging than others so at times multiple partners would work together. Personally I was in charge of setting up the login and register page, the feed page and assiting with the home page when needed. 

### Login and Register

The setup for both pages were pretty similar. I used React hooks to set state and hold the information the user gives us. From there I used functions that handled when a user submitted their information. This informaiton would then be sent to our database with an axios request where the information will be stored for when the user would like to login. 

![image](https://user-images.githubusercontent.com/68297258/117744972-8187df00-b1be-11eb-8929-ae2a7ae9a19d.png)


``` javascript
const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: '',
    passwordConfirmation: '',
    myTags: []
  })

 const handleSubmit = async event => {
    try {
      event.preventDefault()
      await axios.post('/api/Register', formData)
      const response = await axios.post('/api/login', { email: formData.email, password: formData.password })
      window.localStorage.setItem('token', response.data.token)
      history.push('/home')
    } catch (err) {
      setErrors('input is-danger')
      window.alert('Your email or username is already in use.')
      console.log(err)
    }
  }

```


The registration page allows for the user to input data based off the destinations they would like to view. Once logged in their chosen tags would populate the home page with similar destinations that as well have those tags. 

``` javascript
  const groupedOptions = [
    { label: 'Suitable For', options: suitableOptions },
    { label: 'Tags', options: tagOptions },
    { label: 'Continents', options: continentOptions }
  ]

const handleMultiChange = (selected) => {
    const values = selected ? selected.map(item => item.value) : []
    setFormData({ ...formData, myTags: [...values] })
  }
```


## Feed
For the feed page I tried to make it look like your typical social media appliction where the users are able to see all posts made by other users. First I needed to gather all the users and the information attached to them using an axios request. Putting this inside the useEffect wiht a dependency I made sure the information only rendered once when the page is loaded. 

After dealing with the users given inforamtion and displaying the information I needed, I now focused on adding comments and likes to a users posts. Here I used several funcitons to handle all the events. Inside the JSX I used the bulma framework to layout how each post would look. Within the JSX I then needed to map over several bits of information to make sure all posts, comments and likes were shown once the page refreshed. 




``` javascript
 const handleComment = async event => {
    const setComment = async () => {
      try {
        event.preventDefault()
        const token = window.localStorage.getItem('token')
        console.log(token)
        // console.log('COMMENTS', comments)
        await axios.post(`/api/profiles/${event.target.target}/photos/${event.target.name}`, commentData, {
          headers: {
            Authorization: `Bearer ${token}`
          }
        })
        window.location.reload()
      } catch (err) {
        window.alert('You cannot comment on the photo, you are not logged in. 😬')
      }
    }
    setComment()
  }
```


![image](https://user-images.githubusercontent.com/68297258/117764580-97f36200-b1e1-11eb-9ac4-7c568de28c9d.png)



## Wins
 I believe this project turned out really well. I did not know what to expect working with a team, but I really enjoyed it and we all got along really well. The styling for this application makes you feel as though you really are using Netflix. The functionality is clean and the user is able to do many commands.

## Bugs and Blockers
One of our major blockers would be finding a common style and making sure everyone followed. We struggled at first with pages not looking in sync but after communicating we were able to make the application flow. One bug we were not able to fix is when a user likes or comments on a photo, the page will then reload to display that like or comment. 
 


## Future Improvements

Improved styling on the profile side of the app
Fully responsive design
Ability to edit and delete comments and likes
Likes and comments to appear without reloading the page
Remove duplicates when using the search bar filter

## Key Learnings

Personally this project helped me develop a lot an software engineer especially working alongside three other people. There were some challenges with functionality we all struggled with, but hearing everyones different solutions made you think differently and eventually come to a resolution. I really loved coding in a group. 

![image](https://user-images.githubusercontent.com/68297258/117764999-2d8ef180-b1e2-11eb-8024-ebc1d08d9e96.png)