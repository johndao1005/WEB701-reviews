# WEB701-Design

With any successful and complete project, time management is an important factor to measure the progress, prioritise tasks and ensure the project is on track, especially with students' time budgets. Many tools can help with time management such as DevOps, or Asana but the user's commitment to follow the timeline is still the biggest factor.

Before any design or development, it is important to produce a report to gather and analyse important information regarding the website. First, we need to understand the goals and mission of the web application in order to understand the target audience. From there, further investigation can be conducted to refine the raw data by focusing on an aspect of a website such as UX(user experience), UI(user interface) and especially competitors or organisations with similar goals. For more details, I conduct research on the user of Kai Rescue service through Facebook and gather information from the organisation whose goals are aligned with Kai Rescue such as Salvation Amy, and KaiBosh to understand the requirements for the food donation webpage as well as special features which can be provided. Afterwards, the design and structure of the website can be layout following the gathered information as well as the business requirements. It is worth noticing that the design can be tricky and may require multiple revisions and updates before completely satisfying all the requirements. The layout design before mockup really helps with the process as the layout can be simple placement with any design or style for web developers to picture and test the UX aspect before going deeper into the design aspect. Afterwards, we got the mockup design to showcase how the final product can be as well as apply the theme and UI side for the webpage. The mockup can be interactive or include a description of how elements will respond to user input.

After the design, I start designing the back-end of the website with database connect, route and API architect. The backend would require MongoDB, ExpressJS, NodeJs and Postman to check the route function and data transition. Further package from npm is also installed to improve the functions such as dotenv to protect server data on the local machine. Setup up the connection with MongoDB by creating a config folder and config file to establish a connection and return the message to indicate the connection status.

```javascript
require('dotenv').config()
const mongoose = require('mongoose');
//Show the connection state with database connection

const connectDB = async(mongoURI=process.env.MONGO_URI) => {
    try {
        mongoose.connect(mongoURI, {
            useNewUrlParser: true,
            useUnifiedTopology: true,
        });
        console.log("MongoDB connection Successful")
    } catch (e) {
        console.error("MongoDB connection Failure")
        process.exit(1)
    }
}

module.exports = connectDB;
```

Afterwards, we can set up an application with express.

```javascript
require('dotenv').config();
const express = require("express")
const app = express();
const PORT = process.env.PORT || 3000 // if reading .env file fail => running on port 3000
app.listen(PORT, () => console.log(`Server running on port ${PORT}`))
```

A route to enable API can then be set up and use after set up the model and controller for the application.