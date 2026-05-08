# DevOps-Projects-03
<<<<<<< HEAD

#MERN STACK TO-DO WEB APPLICATION


#Project Overview

##In this project, I deployed a full stack To-Do web application built on the MERN stack - running entirely on an AWS EC2 instance (Ubuntu 20,04). This is not just a "Hello World" its a complete, working web application with React frontend, an Express/Node.js backend, and a MongoDB all talking to each other in real time

##A user can create, veiw, and manage To-Do items through the browser. Every interaction that changes datatravels from the React UI -> Express server -> MongoDB database and back. This project demonstrates how modern web applications are structured and deployrd - which is at the heart of what Devops engineers support, maintain, and ship.

##**Step-by-Step Implementation


##Step 1: **Launch & Prepare the EC2 Instance
sudo apt update
# Upgrade installed packages
sudo apt upgrade -y
[Screenshot: EC2 instance running in AWS dashboard — show the Name tag and
“Running” status]
[Screenshot: Terminal showing sudo apt update and upgrade completing successfully]
Step 2: Install Node.js & NPM
Got the Node.js v18 setup script from NodeSource and installed both Node.js and NPM in
one command.
# Get Node.js 18 setup script
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
# Install Node.js (npm is included)
sudo apt-get install -y nodejs
# Verify both are installed
node -v
npm -v
[Screenshot: Terminal showing node -v and npm -v version outputs confirming
installation]
Step 3: Set Up the Project Directory
Created the project folder and initialised it as a Node.js project using npm init . This
generates the package.json file — the heart of any Node project.
# Create project folder
mkdir Todo
cd Todo
# Initialise the Node project
npm init
# (Press Enter to accept defaults, or fill in project details)
[Screenshot: Terminal showing the Todo directory created and npm init running with
package.json generated]
Step 4: Install ExpressJS & Set Up the Backend Server
Installed ExpressJS as the web framework and created the main server entry file.
# Install Express
npm install express
# Install dotenv for environment variables
npm install dotenv
# Create the main server file
touch index.js
Then opened index.js and wrote the basic Express server:
const express = require('express');
require('dotenv').config();
const app = express();
const port = process.env.PORT || 5000;
app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers",
"Origin, X-Requested-With, Content-Type, Accept");
next();
});
app.use((req, res, next) => {
res.send('Welcome to Express');
});
app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
# Start the server to test it
node index.js
[Screenshot: Terminal showing “Server running on port 5000” — server started
successfully]
Open Port 5000 in AWS Security Group before testing in the browser
[Screenshot: AWS Security Group showing Port 5000 inbound rule added]
[Screenshot: Browser showing “Welcome to Express” at http://your-ec2-public-
ip:5000]
Step 5: Create the RESTful API Routes
Created a routes folder and defined the three API routes the To-Do app needs — GET
(fetch all todos), POST (create a todo), and DELETE (remove a todo).
mkdir routes
touch routes/api.js
// routes/api.js
const express = require('express');
const router = express.Router();
// GET all todos
router.get('/todos', (req, res, next) => {
// logic here
});
// POST a new todo
router.post('/todos', (req, res, next) => {
// logic here
});
// DELETE a todo
router.delete('/todos/:id', (req, res, next) => {
// logic here
});
module.exports = router;
[Screenshot: VS Code or terminal showing the routes/api.js file structure]
Step 6: Set Up MongoDB with Mongoose
Signed up for MongoDB Atlas (cloud-hosted MongoDB), created a cluster, and connected
it to the application using Mongoose.
# Install Mongoose
npm install mongoose
Created the models folder and defined the Todo schema:
mkdir models
touch models/todo.js
// models/todo.js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
});
const Todo = mongoose.model('todo', TodoSchema);
module.exports = Todo;
Created a .env file to store the MongoDB connection string securely:
[Screenshot: MongoDB Atlas dashboard showing cluster created and running]
[Screenshot: Terminal or VS Code showing .env file with DB variable (blur/hide your
actual credentials)]
[Screenshot: Terminal showing server started with “Database connected successfully”
message]
Step 7: Test the API with Postman
Before building the frontend, I tested all three API endpoints using Postman to make sure
the backend was working correctly.
Tests performed:
touch .env
# Add this inside .env:
# DB = 'mongodb+srv://<username>:<password>@cluster.mongodb.net/todo?retryWrites=true&w=major
Test Method Endpoint Expected Result
Create a To-Do POST /api/todos Returns new todo object
Get all To-Dos GET /api/todos Returns array of todos
Delete a To-Do DELETE /api/todos/:id Removes todo from DB
[Screenshot: Postman — POST request creating a new To-Do, showing 201 response
with data]
[Screenshot: Postman — GET request showing all To-Dos returned from MongoDB]
[Screenshot: Postman — DELETE request removing a To-Do by ID, showing success
response]
This is a crucial screenshot moment. Postman results prove your backend and
database are fully connected before you even touch the frontend.
Step 8: Set Up the React Frontend
With the backend confirmed working, I created the React frontend using create-react-app
inside the Todo directory.
# From inside the Todo directory
npx create-react-app client
# Install required dependencies
cd client
npm install axios
Then installed concurrently and nodemon to run frontend and backend together:
# Back in the root Todo directory
npm install concurrently --save-dev
npm install nodemon --save-dev
Updated package.json scripts to run both servers at once:
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
}
[Screenshot: Terminal showing both the React frontend (port 3000) and Express
backend (port 5000) running together]
Open Port 3000 in AWS Security Group so you can view the React app in the
browser
Step 9: Build the React Components
Created the three key React components that make up the To-Do UI:
cd client/src
mkdir components
touch components/Input.js
touch components/ListTodo.js
touch components/Todo.js
Input.js — The text input and button to add new To-Dos
ListTodo.js — Displays all existing To-Do items
Todo.js — The parent component that ties Input and List together
Also updated App.js to import and render the Todo component, and updated App.css
with styling.
[Screenshot: VS Code showing the components folder structure with all 3 component
files]
Step 10: Launch & View the Full Application
Started the full application and opened it in the browser.
# From the root Todo directory
npm run dev
[Screenshot: Browser showing the To-Do app running at http://your-ec2-public-ip:3000
— the full working UI]
[Screenshot: Adding a new To-Do item in the browser and seeing it appear in the list]
[Screenshot: MongoDB Atlas dashboard showing the To-Do documents stored in the
database]
Outcome
Successfully deployed a fully functional full-stack MERN To-Do web application on AWS
EC2. The React frontend communicates with the Express/Node.js backend via RESTful API,
and all data is persisted in a MongoDB Atlas cloud database.
What this proves:
Ability to build and deploy a full-stack web application end-to-end
Understanding of how frontend, backend, and database layers communicate
Hands-on experience with RESTful API design and testing (Postman)
Working knowledge of the MERN stack — one of the most in-demand tech stacks
Cloud deployment skills on AWS EC2 with proper port and security configuration
Understanding of NoSQL databases (MongoDB) vs relational databases
Challenges & How I Fixed Them
Challenge What Went Wrong How I Fixed It
App not accessible
in browser
Port 5000 / 3000 not
open
Added inbound rules to AWS Security
Group
MongoDB
connection failing
Wrong connection
string format in .env
Fixed the Atlas URI and restarted the
server
React app not
connecting to
backend
Proxy not configured in
client package.json
Added "proxy":
"http://localhost:5000" to
client/package.json
nodemon not found Installed locally, not
globally
Used npx nodemon or added it to
scripts in package.json
CORS errors in
browser
Express not handling
cross-origin requests
Added CORS headers middleware in
index.js
Key Takeaways
The MERN stack is a fully JavaScript-based stack — one language from frontend to
backend to database queries
MongoDB stores data as flexible JSON-like documents, making it ideal for applications
where data structure may evolve over time
RESTful APIs are the bridge between the frontend and backend — always test your API
(with Postman) before building the UI
React components break the UI into reusable, manageable pieces — much cleaner than
building everything in one file
Environment variables ( .env ) keep sensitive credentials like database passwords out
of your source code — never commit your .env file to GitHub
Running a full-stack app means managing multiple processes — tools like concurrently
make local development much smoother
Security Note
The .env file containing the MongoDB connection string is NOT committed to this
repository. It is listed in .gitignore . If you are cloning this project, create your own .env
file and add your own MongoDB Atlas connection string.
# .gitignore
.env
node_modules/
Project Structure
Todo/
│
├── index.js ← Express server entry point
├── package.json ← Project dependencies and scripts
├── .env ← Environment variables (NOT committed)
├── .gitignore
│
├── routes/
│ └── api.js ← API route definitions (GET, POST, DELETE)
│
├── models/
│ └── todo.js ← MongoDB/Mongoose data schema
│
└── client/ ← React frontend (created with create-react-app)
├── public/
└── src/
├── App.js
├── App.css
└── components/
├── Input.js
├── ListTodo.js
└── Todo.js
images/
├── ec2-running.png
├── node-npm-version.png
├── server-running-port-5000.png
├── express-in-browser.png
├── security-group-ports.png
├── postman-post-todo.png
├── postman-get-todos.png
├── postman-delete-todo.png
├── mongodb-atlas-cluster.png
├── mongodb-documents.png
├── react-frontend-running.png
└── todo-app-working.png
Connect With Me
If you found this project helpful or you’re on a similar DevOps / full-stack learning journey,
let’s connect!
LinkedIn ← add your LinkedIn URL here
GitHub Profile ← add your GitHub profile URL here
Other Projects in This Series
# Project Key Skills
01
=======
MERN WEB STACK PROJECT
>>>>>>> 9c98c147b4853e2f54b5f7b22cfd51194676600c
