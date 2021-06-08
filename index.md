# Month 2 - Building something from scratch

## 11 May 2021 - 07 Jun 2021

---

## Summary

This month we completed the building from scratch phase. In each weekly blog I documented what did I learn in terms of technology; in this blog I will talk about the project that I and my team developed.

The project was a repository of dental records, as well as an appointment system with the ability to notify the dentist of the arrival of a patient. This project is a web application; This means that users can access from a web server through the internet, and through a browser. It is full stack; this means the frontend (the interface that is shown to the user) and the backend (the part that receives information from the frontend to process it).

The use of software development technologies commonly used in the industry was developed, such as Node.js (along with npm), React, Heroku, MongoDB, Github, among others. The way of working in a software development team was practiced. These practices include the use of tickets for the division of work using Jira, integration of the work in the project of each team member using GitHub, application of the Agile methodology to obtain feedback from the client regarding the work developed, among other practices.

At the end of the project, the development of a functional web application was completed according to the client's requirements for this period.

## Intro

*Purpose*: Continue the development of the web-app according to the client's requirements.

*Scope*: The basic requirements to be developed are:

- Redesign of the website; The customer was not satisfied with the design of the page.
- Creation of the functionality for the capture of medical records, as well as their consultation.
- Ability to upload existing files.
- Link of an appointment system already implemented with the corresponding dentist by means of a notification of the patient's arrival at the clinic.

## Objectives

The objectives are divided according to the requirements:

- Website redesign: Unify the design of the clinic page; the original design does not have clinic logos, and does not comply with the manual of its colors.
- Ability to create and consult histories: More effective search, as well as the use of tablets, telephones for the creation of histories.
- Ability to upload existing files: Digitization of the file, as well as its effective search.
- Link through notification: Minimize contact between receptionist and patient, as well as indicate to the dentist the arrival of a patient, with the ability to view their file quickly.

## What did I use?

Technologies used:

- Node js: It was used to create the backend.
- Express: Framework that uses Node js to make backend development easier. It basically provides a means of creating routes, as well as means of responding or moving to the next function.
- React: Framework used to create the frontend; allows you to create components based on- html, and reuse them. This technology facilitates the creation of the frontend.
- Heroku: Web application used to upload the web page and to be able to test the- behavior in a real environment.
- Visual Code: Text editor used to create the code.
- GitHub: Version Control Tool; used for collaborative work on the website.
- Insomnia: Client for creating REST requests to a specified URL. Support in the- creation of the backend.
- NPM: Javascript Package Manager for Node js.
- Nodemon: Node js package for backend development. It allows the automatic restart of- the backend when there are changes detected in the code.
- Yup: Javascript library used for data validation.
- Formik: Javascript library used in the frontend for the creation of forms easily, as- well as the monitoring of changes in the fields for data validation purposes.
- MongoDB: No SQL database whose advantage is the focus on web requests.
- Mongoose: Javascript library used for the communication of the database in the backend.
- Jira: Ticket system for the team's division of labor. Lets find out who is doing what.

## What did I do in this project?

### Development of the backend for the medical record

The work carried out consists of receiving this data in the backend, as well as its validation and processing. This involves several tasks:

- Endpoint creation: An endpoint is the URL to which the data is sent. For example: <https://my-site.com/api/datos>. The url in question is a backend path for receiving or requesting data. The backend endpoint recognizes the REST method used, and returns information according to the operation in the request. For example, GET requests return data represented by the url, commonly used with search parameters. The POST request creates an object, and returns the created object if the operation was successful.

- Data validation: Determines if the data is valid for insertion into the database. For example, a recurring validation is that there are no numbers in the names.

- Validation of foreign keys: For example, when you need to create a file, you must have the corresponding profile of the patient. The design does not allow the creation of a file if the profile of the patient it refers to does not exist.

- Communication with the database: Use of interfaces for communication with the database. This implies the creation, modification, elimination and search of data in the database according to what is required by the backend.

### User modification

The summary task is: a user can now have a guardian or guardian. The tasks carried out are:

- Modification of the frontend to show fields of the tutor when the user is registering. The fields are displayed or not according to a checkbox.
- Validation of the tutor fields in the frontend.
- Modification of the sending of the data to the backend to include the tutor's information if necessary.
- Modification of endpoints in the backend for the reception and validation of the tutor's data.
- Modification of the insertion of the data in the database to include the fields of the tutor.

### Creation of the file search filter (per patient)

A search filter is developed on the user profile page to be able to consult files with the characteristics entered. Modification of the frontend and creation of requests to the back with the search parameters, as well as the creation of endpoints for the search, and the implementation of paging in the backend. Pagination works as follows:

- The backend is asked for the results of a search, specifying how many files are expected to return, as well as how many to skip before starting to return (offset).
- The backend returns the number of total files that meet the parameters, and the number of files specified.

### Backend for receiving the information of the informed consent

Development of the backend to process the information from the informed consent:

- Validation of incoming data.
- Verification of the existence of a treatment to which the informed consent is linked.
- Insertion in the information database.

The informed consent has signatures that need to be reconstructed to show the format again.

### Development of the ‘Clinical Exam’ tab

Development of the tab on the front, as well as sending the information to the backend.

- This tab contains reusable components, and with the ability to remember their status using React hooks.
- The tab has validations on the frontend and the backend, like all the information.

### Modified database design

According to customer requirements, the previous database was modified to accommodate certain characteristics, such as a user's tutor, discussed above. Two documents were added (It is a NoSQL database): The patient's profile, and the patient's records. The patient's profile contains information that the client determined as non-modifiable: such as the patient's data, medical history, and dental history. The patient's record is relatively modifiable information; Procedures can be added and informed consents added to these procedures. This file is in the form of history: there are previous versions of the files.

### Conclusions

In this project I learned a lot about some tools used in the industry for software development, such as Node.js, React, GitHub, Nodemon, Yup, MongoDB, etc. I learned a lot about the way of working in a team of software developers, such as tickets, the Agile methodology for the way of working, and more than anything, experience in how to work in a team, how to learn from others, and how reach consensus when there are disagreements. The project is in its second major iteration, and is expected to be completed and put into production in the third iteration.

## All blogs

| Blog | Info |
| --- | --- |
| [Week 1 - Innovation and hard\smart work](/Week_Pages/Week1_April.md) | 05 Apr 2021 - 12 Apr 2021 |
| [Week 2 - Polyglot Programming](/Week_Pages/Week2_April.md) | 13 Apr 2021 - 19 Apr 2021 |
| [Week 3 - Fancy Topics](/Week_Pages/Week3_April.md) | 20 Apr 2021 - 26 Apr 2021 |
| [Week 4 - It's all about science](/Week_Pages/Week4_April.md) | 27 Apr 2021 - 03 May 2021 |
| [Week 4 - Pretotypes](/Pretotypes/Pretotypes_April2021.md) | My pretotypes for week 4 |
| [Month 1 - Reset Phase](/Month_Pages/Month1_April.md) | 05 Apr 2021 - 03 May 2021 |
| [Week 6 - Building something from scratch (1)](/Week_Pages/Week6_May.md) | 11 May 2021 - 17 May 2021 |
| [Week 7 - Building something from scratch (2)](/Week_Pages/Week7_May.md) | 18 May 2021 - 24 May 2021 |
| [Week 8 - Building something from scratch (3)](/Week_Pages/Week8_May.md) | 25 May 2021 - 31 May 2021 |
| [Week 9 - Building something from scratch (4)](/Week_Pages/Week9_Jun.md) | 01 Jun 2021 - 07 Jun 2021 |
| [Month 2 - Building something from scratch](/Month_Pages/Month2_May.md) | 11 May 2021 - 07 Jun 2021 |
