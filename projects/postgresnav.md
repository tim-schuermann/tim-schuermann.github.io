## Electron-based PostgreSQL Navigator App

**Project description:** Many data scientists may be familiar with the scenario that colleagues from teams that are not coding-related can struggle to retrieve and manage information from databases. Based on a project where budgetary information was handled via a PostgreSQL database, in this project I host a PostgreSQL database with fake budgetary information on railway.app. I use a FastAPI backend to expose endpoints to an Electron app where end users can filter transaction items based on company domain, project, or cost type and retrieve summary information on those items. While the project itself is fairly limited in functionality, the following sections highlight more involved aspects to show my experience with them.

### Interaction with a cloud-hosted database

Setting up interaction with a cloud-hosted database involves the additional step of secrets management, which this project handles via a .env file that naturally is not committed to the repository. The connection details are used within the FastAPI backend [here](https://github.com/tim-schuermann/PostgresNavigator_Backend/blob/master/app/database.py).

### Securing a FastAPI app with OAuth2 and JWT tokens

Authentication in a FastAPI app impacts a decent amount of code: aside from handling password hashing and generation of access tokens, the API routes themselves need to properly account for authentication. In this project, I additionally separate access rights between user and admin accounts by integrating the user role into the access token [here](https://github.com/tim-schuermann/PostgresNavigator_Backend/blob/master/app/routers/auth.py). While undeniably overkill for this project, user roles with different access permissions are mandatory in many real-world examples.

### Enabling authenticated communication between the Electron app and FastAPI

For the Electron app to access API endpoints that require authentication, it needs to issue a POST call to the FastAPI backend [here](https://github.com/tim-schuermann/PostgresNavigator/blob/main/api.js) and store the access token locally. Then, for each fetch call to an endpoint, the access token needs to be included in the request. For the user interface, that means putting a login mask in front of the actual database navigation aspect of the app. Once user authentication is complete, the sidebar of the application is populated with options to filter the items as described above. This way, the end user will always get the currently available selection of domains, projects, and cost types reflecting the state of the database.

### Related repositories
For more details on the FastAPI backend, see [FastAPI backend](github.com/tim-schuermann/PostgresNavigator_Backend).
For more details on the Electron frontend, see [Electron frontend](github.com/tim-schuermann/PostgresNavigator).
