# Movie-Reviewer-SpringBoot-React
Full stack web app using Mongodb for database, Java and Spring Boot for the backend, and React for the frontend.

<a href="https://www.freecodecamp.org/news/full-stack-development-with-mongodb-java-and-react/">Full Stack Dev with MongoDB, Java and React</a>

Summarized Backend Process:
<ul>
  <li>Set up MongoDB and import data.</li>
  <li>Set up Spring Initialzr and import into IntelliJ.</li>
  <li>Configure database for MongoDB.</li>
  <li>Create Movies and Reviews Enpoints with controllers, repositories and services for movies and reviews.</li>
</ul>

Summarized Frontend Process:
<ul>
  <li>Create React project.</li>
  <li>Apply Bootstrap to React Application.</li>
  <li>Implementing the Use State and Use Effect Hooks.</li>
  <li>Create Home and Hero Component.</li>
  <li>Style the Carousel.</li>
  <li>Create Header Component(Navigation).</li>
  <li>Create Trailer Component with react player.</li>
  <li>Create Movie Reviews Functionality.</li>
  <li>Add and Get Reviews with HTTP Requests.</li>
</ul>

Notes:
<ul>
  <li>Manager/controller receives the work, decides who should do it and then passes off the request to be completed. The worker/service is the one that takes that request and actually completes it.</li>
  <li>Controller:
    <ul>
      <li>manages the incoming work HTTP requests.</li>
      <li>decides which worker what service should do the work.</li>
      <li>splits up the work into sizable units.</li>
      <li>passes that work the necessary data from the HTTP requests off to the service(s).</li>
      <li>if the work requires multiple people services working on multiple things, orchestrates the work those service calls.</li>
      <li>but does not do the work himself/herself.</li>
    </ul>
  </li>
  <li>Service:
    <ul>
      <li>receives the request data it needs from the manager in order to perform its tasks.</li>
      <li>figures out the individual details algorithms/business logic/database calls/etc involved in completing the request.</li>
      <li>is generally only concerned with the tasks he/she has to complete.</li>
      <li>not responsible for making decisions about the "bigger" picture orchestrating the different service calls.</li>
      <li>does the actual work necessary to complete the tasks/request.</li>
      <li>returns the completed work a response to the manager.</li>
    </ul>
  </li>
  <li>Use "rafce" to import boilerplate code in frontend.</li>
</ul>

<b>Detailed Process:</b><br>
MongoDB:
<ul>
  <li>Create new Project MovieAPI on MongoDB Atlas. </li>
  <li>Connect to Cluster using MongoDB Compass. Copy connection string URI.</li>
  <li>Download Compass. Open Compass. Paste connection string URI into URI section of New Connection. Save and Connect.</li>
  <li>Create a new Database. Database Name: movie-api-db. Collection Name: movies. Create Database.</li>
  <li>Save movie data from JSON file from <a href="https://github.com/fhsinchy/movieist">movieist</a></li>
  <li>Import JSON file into Collection movies in movie-api-db.</li>
</ul>

Spring Initializr(Link):
<ul>
  <li>Project: Maven</li>
  <li>Language: Java</li>
  <li>Spring Boot: 3.0.2</li>
  <li>Packaging:Jar</li>
  <li>Java: 17</li>
  <li>Dependencies:
    <ul>
      <li>Lombok - DEVELOPER TOOLS : Java annotation library which helps to reduce boilerplate code.</li>
      <li>Spring Web - WEB : Build web, including RESTful, applications using Spring MVC. Uses Apache Tomcat as the default embedded container.</li>
      <li>Spring Data MongoDB - NOSQL: Store data in flexible, JSON-like documents, meaning fields can vary from document to document and data structure can be changed over time.</li>
      <li>Spring Boot DevTools - DEVELOPER TOOLS: Provides fast application restarts, LiveReload, and configurations for enhanced development experience.</li>
    </ul>
  </li>
  <li>Generate zip file, extract file, and import into IntelliJ</li>
</ul>

<b>Backend</b>:<br>
Run app and check "http://localhost:8080/" to ensure it says "Whitelabel Error Page."<br>
Writing the First Endpoint:
<ul>
  <li>Add <code>@RestController</code> annotation to <code>MoviesApplication.java</code>.</li>
  <li>In <code>MoviesApplication.java</code>, create method <code>apiRoot()</code> with <code>@GetMapping</code> annotation. This is the annotation for mapping HTTP GET requests onto specific handler methods.</li>
  <li>Run app and test "http://localhost:8080/" to ensure apiRoot returned value is there.</li>
</ul>
Database Configuration: 
<ul>
  <li>In application.properties, add spring.data.mongodb.database and spring.data.mongodb.uri. Run to ensure it works.</li>
  <li>Create .env file to hold sensitive information.</li>
  <li>Add new dependency to <code>application.properties</code>. <a href="https://mvnrepository.com/artifact/me.paulschwarz/spring-dotenv">spring-dotenv</a>. groupId: me.paulschwarz, artifactId: spring-dotenv, version: 2.5.4</li>
  <li>In application.properties, update the spring.data.mongodb.database and spring.data.mongodb.uri with .env values.</li>
</ul>
Movies and Reviews Endpoint
<ol>
  <li>Create new class "Movie" with <code>@Document</code> to let framework know this class represents each document in the movies collection. 
    <ul>
      <li>Annotate class with <code>@Data</code> from Lombok to take care of all the getters and setters methods. </li>
      <li>Annotate class with <code>@AllArgsConstructor</code> as an addition for creating a constructor that takes all these private fields as argument.</li>
      <li>Annotate class with <code>@NoArgsConstructor</code> as a constructor that takes no parameters.</li>
      <li>Annotate <code>ObjectId</code> with <code>@Id</code> to let framework know this is an ObjectId.</li>
      <li>Create private variables.</li>
      <li>Use <code>@DocumentReference</code> for <code>reviewIds</code> to cause the DB to store only the IDs of the review. The views will be in a separate collection.</li>
    </ul>
  </li>
  <li>Create new class "Reviews" with <code>@Document</code> to let framework know this class represents each document in the movies collection.
    <ul>
      <li>Use <code>@Data</code> annotation from Lombok to take care of all the getters and setters methods. </li>
      <li>Use <code>@AllArgsConstructor</code> annotation is an addition for creating a constructor that takes all these private fields as argument.</li>
      <li>Use <code>@NoArgsConstructor</code> annotation is constructor that takes no parameters.</li>
      <li>Use <code>@Id</code> to let framework know this <code>ObjectId</code></li>
      <li>Create private variables.</li>
      <li>Because we cannot pass an ID to the Review class, we generate a custom constructor that takes only the body. Right Click in IntelliJ->generate->body to create a constructor.</li>
    </ul>
  </li>
  <li>Create new interface MovieRepository to extend <code>MongoRepository</code>.
    <ul>
      <li>Need to let MongoRepository know what type of data(Movie) and type of ID(ObjectId).</li>
      <li>Annotate class with <code>@Repository</code> to let framework know this is a repository.</li>
    </ul>
  </li>
  <li>Create new class MovieService.
    <ul>
      <li>Annotate class with <code>@Service</code> to let framework know this is a service.</li>
      <li>Instantiate <code>movieRepository</code>. Use <code>@Autowired</code> to tell framework to instantiate this class for us, or create an instance or object of this class.</li>
    </ul>
  </li>
  <li>Create new class "MovieController" .
    <ul>
      <li>Annotate class with <code>@RestController</code>. </li>
      <li>Annotate class with <code>@RequestMapping</code> to new endpoint ("/api/v1/movies").</li>
      <li>Use <code>@GetMapping</code> for <code>allMovies</code> method. Test using "http://localhost:8080/api/v1/movies".</li>
      <li>Instantiate <code>movieService</code>. Use <code>@Autowired</code> to tell framework to instantiate this class for us, or create an instance or object of this class.</li>
    </ul>
  </li>
  <li>Get all movies
    <ul>
      <li>In MovieService create <code>allMovies()</code> method that returns the <code>movieRepository.findAll()</code> method.</li>
      <li>In MovieController create <code>getAllMovies()</code> method to return a ResponseEntity with movieService.allMovies() method and HttpStatus.OK.</li>
      <li>Test using "http://localhost:8080/api/v1/movies".</li>
    </ul>
  </li>
  <li>Get individual movie
    <ul>
      <li>In MovieController create getSingleMovie() method. Use <code>@GetMapping</code> to handle GET type of request method. Use <code>@PathVariable</code> to let framework know we will be passing the info we got in the mapping as a path variable.</li>
      <li>To use ObjectId, in MovieService, create method singleMovie() which takes an ObjectId id and returns the ResponseEntity with <code>movieRepository.findById(id)</code> and HttpStatus.OK. Because the repository may not return a movie, use <code>Optional</code>.</li>
      <li>Test using "http://localhost:8080/api/v1/movies/63ddcd47b3d6991d8c7281e2" or different id.</li>
      <li>To use imdbId, in  MovieRepository, we create a new method <code>findMovieByImdbId</code> and the framework knows what we are searching for a unique ImdbId</li>
      <li>In MovieService, we create new method <code>singleMovie()</code> and return <code>movieRepository.findMovieByImdbId(imdbId)</code></li>
      <li>To use ObjectId, in MovieService, create method singleMovie() which takes an imdbId and returns the ResponseEntity with <code>movieRepository.findByImdbId(imdbId)</code> and HttpStatus.OK.</li>
      <li>Test using "http://localhost:8080/api/v1/movies/tt3915174"</li>
    </ul>
  </li>
  <li>Create new interface ReviewRepository to extend <code>MongoRepository</code>.
    <ul>
      <li>Need to let MongoRepository know what type of data(Review) and type of ID(ObjectId).</li>
      <li>Annotate class with <code>@Repository</code> to let framework know this is a repository.</li>
    </ul>
  </li>
  <li>Create new class ReviewService.
    <ul>
      <li>Annotate class with <code>@Service</code> to let framework know this is a service.</li>
      <li>Create new method <code>createReview()</code>.</li>
      <li>To add Review to database, instantiate <code>reviewRepository</code>. Use <code>@Autowired</code> to tell framework to instantiate this class for us, or create an instance or object of this class. Use <code>reviewRepository.insert(review);</code>.</li>
      <li>To associate Review with a movie, create a template using <code>MongoTemplate</code>. We can use this template to form up a new dynamic query and do the job inside the database without the repository. Use <code>@Autowired</code> to tell framework to instantiate this class for us, or create an instance or object of this class.</li>
      <li>We perform an update on the Movie class because each Movie contains an empty array of reviewIds. Use matching to match the imdbId of the movie in the database to match imdbId we received from the user. We want to apply this update the reviewIds of the found movie with the value of the Review the user made. We use first() to make sure we are getting and updating a single movie.</li>
    </ul>
  </li>
  <li>Create new class "ReviewController" .
    <ul>
      <li>Annotate class with <code>@RestController</code>. </li>
      <li>Annotate class with <code>@RequestMapping</code> to new endpoint ("/api/v1/movies").</li>
      <li>Use <code>@PostMapping</code> for mapping HTTP POS requests.</li>
      <li>Instantiate <code>reviewService</code>. Use <code>@Autowired</code> to tell framework to instantiate this class for us, or create an instance or object of this class.</li>
      <li>In ReviewController create <code>createReview()</code> method to return a ResponseEntity with <code>service.createReview(payload.get("reviewBody"), payload.get("imdbId")</code> method and HttpStatus.OK.</li>
      <li>Test using "http://localhost:8080/api/v1/reviews" with {"reviewBody": "Entertaining movie for the whole family!", "imdbID": "tt3915174"}</li>
    </ul>
  </li>
</ol>

<b>Frontend</b>
<ol>
  <li>Create React Project
    <ul>
      <li>~<code>npx create-react-app movie-gold-v1</code></li>
      <li>From the src file, remove the App.test.js, setupTests.js,and reportRebVitals.js.</li>
      <li>In the package.json, remove "eslintConfig".</li>
      <li>In the index.js, remove the coderelated to the reportWebVitals.</li>
      <li>In the terminal, install Axios ~<code>npm install axios</code>.</li>
    </ul>
  </li>
  <li>Applying Bootstrap to React App
    <ul>
      <li>Install bootstrap ~<code>npm install bootstrap</code></li>
      <li>~<code>npm i react-bootstrap</code></li>
      <li>~<code>npm i @fortawesome/react-fontawesome</code></li>
      <li>~<code>npm i @fortawesome/free-solid-svg-icons</code></li>
      <li>~<code>npm i react-player</code></li>
      <li>~<code>npm i react-router-dom</code></li>
      <li>~<code>npm install @mui/material @emotion/react @emotion/styled</code></li>
      <li>~<code>npm install react-material-ui-carousel</code></li>
      <li>In index.js, <code>import 'bootstrap/dist/css/bootstrap.min.css'</code></li>
      <li>Create api folder, inside api folder create axiosConfig.js. Modified axiosConfig.js to be used with localhost:8080.</li>
    </ul>
  </li>
  <li>Implementing the use state and use effect hooks
    <ul>
      <li>In App.js
        <ul>
          <li>Import Axios object from within our Axios config.js file.</li>
          <li>Import useState and useEffect from react.</li>
          <li>When state of the variable tracked by react through the use state hook is changed, the component is rerendered by react. So the app component will be rerendered when the state of the movies variable changes.</li>
          <li>Implement useEffect hook so that the function is executed when the app component first loads.</li>
          <li>Test by console logging response.data in getMovies and then using ~<code>npm start</code></li>
        </ul>
    </ul>
  </li>
  <li>Create Home and Hero Component
    <ul>
      <li>Implement routing functionality.</li>
      <li>Create components folder. Create Layout.js file in components folder. Import Outlet from react-router-dom. Use "rafce" to import boilerplate code.</li>
      <li>In App.js, import  Routes, Route from 'react-router-dom'. Write code to establish the route mappings for our application.</li>
      <li>In index.js, import BrowserRouter, Route, Routes from 'react-router-dom'. Modify the render with the imported components.</li>
      <li>Create home folder in components folder. Create Home.js in home folder. Use "rafce" to import boilerplate code.</li>
      <li>Import Home.js into the App.js file and modify route to link to Home. Test with ~<code>npm start</code></li>
      <li>Create hero folder in components folder. Create Hero.js and Hero.css in hero folder. Use "rafce" to import boilerplate code. The hero component will display items in a carousel that are representative of the movies to the users of this application.</li>
    </ul>
  </li>
  <li>Create Header Component(Navigation)
    <ul>
      <li>Create header folder in components folder. Create Header.js in header folder.</li>
    </ul>
  </li>
  <li>Create Trailer Component with react-player
    <ul>
      <li></li>
    </ul>
  </li>
</ol>