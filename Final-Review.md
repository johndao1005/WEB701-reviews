# WEB701-Final Milestone

Improved on the prototype build for the MERN stack which I used React hooks to manage the state and data display on the webpage, for the final product, I used Redux for strong state management throughout the webpage. With Reduct, the information is managed and can only be updated through actions and depending on the result from API, the different states will be returned such as error, loading information or success message and in turn re-render the page.

After some experience and testing, It is noted that there are some limitations with hooks such as if users already login into one tab of the web browser, if they open another tab on the same browser then they can log in again. This can cause a lot of issues with security, and data control so, in order to remove the problem, I need to use Redux to control the login state of the users.

Furthermore, I learned to split routers depending on different states so users can only access certain pages without login credentials, this method will decrease the time needed to handle putting up a router and navigation guard in case users try to hack through the webpage navigation link. It also requires Redux to work as it is hard for the user to edit the state without appropriate action.

Lastly, I also apply Ant design to improve the outlook as well as decrease the design time. The front-end framework comes with prebuilt components, style and icons which allows the design to be optimised. Customisation is also available to apply personal style or theme so the developer can change the colour as well as the behaviour of the element. 

In conclusion, the project helps me a lot with improving my skill in using React, I am very interested to dive deeper into React ecosystem.

