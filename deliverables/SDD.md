# Software Design Document

> Use this markdown file to create the report. For the submission print this document to a PDF, and upload it to iLearn. The maximum number of pages is 30.
> Use the document structure below.

## Title Page

 - DirectionsMQ SDD

 ### Members
 - Latthasay Saisanavong
 - Yuvraj Gill
 - Ethan Almonte
 - Sonny Sayaloune

## Vision Statement  

DirectionsMQ will serve as a comprehensive naviagation tool for students, teaching staff and visitors at Macquarie University. Providing seamless, realtime navigation, DirectionsMQ is designed to help users navigate
through the enormous campus and help them get to their location as fast, and little trouble as possible.


## System Design Document 

This will include the basic architecture of the system and the high-level strategic decisions. You need to include a description of the:

### System architecture
#### 1. Overview
The DirectionsMQ system will be composed of different subsystems that will interact with one another to provide users with turn-by-turn directions, real time updates, accessible routing and customised features like "dryest path" routing. The architecture will be based on a layered model meaning that both mobile and backend components will be used to support functionality like GPS tracking, database management and real time route optimisation. The two main components is the **Client App (Frontend)** and **MQ-Directions-Service (Backend)**, **Database** and external services for weather and live updates.

#### 2.1 Components
##### 2.1.1 Client App (Frontend)
This is the main mobile app that will be installed on user devices, it iwll be responsible for user interface, GPS tracking and local storage. They key modules will include:
- Routing Module: calculates and displays routes within the app also providing a turn by turn guide
- User Interface (UI): Displays the map, directions and information on Points of Interests (POI) like Central Courtyard.
- Offline Mode: Allows for users to download portions of the map and relevant information 
- Feedback & Reporting: Collect and sends the users feedback for things such as reports of route issues, directly to the backend.

##### 2.1.2 MQ-Directions-Servcse (Backend)
The MQDS server is the core of the system managing things such as route calculations, data storage and interfacing with the Directions MQ app. The key Modules will include:
- Routing Engine: Responsible for calculating optimal and accessible routes that will also factor in real tie updates like weather and contruction.
- Dry Path algorithm: Calculates routes with minimal exposure to rain.
- Real-Time Update processor: Revcieve and integrate data on building opening hours, crowd density to optimise routes.

##### 2.1.3 Database
The database is central for storfing campus maps, user reports, crowd density, route data and user preferences. Key elements include:
- Campus Map & Point of Interest (POI): Stores all map data including building layouts, rooms and waypoints.
- User Preferences and Data: Contains all of the users customized settings like preferred routes and saved locations.
- Real Time Data: Hold temporary data on any live updates like closed rooms and crowd density.

##### 2.1.4 External Services
The system will have a few key External Services which will include:
- Weather: Providing real time weather data for dry path calculations
- MQ Authentication: Enables for secure single sign in for Macqurie staff, studends and other users.
- MQ Campus updates: Supplies regular updates on campus events construction and other obstacles.



#### 2.2 Storage/persistent data strategy

##### 2.2.1 Data Types and Storage Requirements
- User data: Inclides user settings, preferences and accesibility needs
  - Storage Location: On the user device via local storage.
  - Persistent: Saved for consistent experience across different sessions

- Campus Map data: Detailed maps o campus with Points of Interests
  - Storage Location: Local storage for offline access with optional updates from server.
  - Persistent: Cached on the device for offline usage and reloaded when the app is online.

- Route Data: Includes optimal paths, alternate routes and accessible paths. Some of which can be submitted by users.
  - Storage Location: Short term storage on the device and uploaded to MQ Direction services when online.
  - Persistent: Cached for offline use and is cleared/updatd periodically.

- User Reprots and Feedbacks: Inclides reports on closed routes, contruction, accessibility feedback and other user submitted data.
  - Storage Location: Temporarily stored on the device and uploaded to  MQDS when connected online.
  - Persistent: Retained on the server so that it can be reviewed by admins and be acted upon accordingly.

- Real Time Data: Crowd Density, weater updates used to modify router based on current conditions
  - Storage Location: Temporarily cached on the device and will refresh regularly
  - Persistent: Cleared after each session or after further updates

##### 2.2.2 Synchronization Strategy
- Offline Sync: The app allows users to download a static map aswell as route data. When the user is back online it will ush user reports and route submissions to MQDS and retireve relevant updates.
- Conflict Resolution: User data conflicts like preferences and waypoints that are updated offline will have a "last modified" displayed.
- Real-Time Data Updates: Real time data will be refreshed on 2 minute intervals.

##### 2.2.3 Data Security and Privacy Considerations
- Encryption: Sensitive data, like user location history and reports, will be encrypted on the device and during transmission.
- Access Control: Based on the role of the user, only authorised people can modify certain data 

 
#### 2.3 Noteworthy trade-offs and choices
   ##### 2.3.1 Real Time vs Offline functionality
Implementing Real-Time routing requires as expected constant internet connection this will greatly increase the systems capabilities. Having this feature will allow The app to provide live location, new updates and best routes. This comes as a sacriface as  offline capabilities are lot less minimal but this comes at the advantage of usability in areas with poor connectivity.

   ##### 2.3.2 Complexity vs. Usability
Including advanced feature onto the system will increase capabilities but inturn take away from the simpleness of the UI. Feature rich System can overwhelm new user compared to a more simplistic Design. The learning curve can deter new users but will in turn be a more complete app in the end.

   ##### 2.3.3 User Privacy vs. Feature Richness
Many feature of the The app DirectionMQ require Users to give up some private info. This may include sensitive data such as location. Though the app may take in compromising data it gives acess to several functions in the app.

   ##### 2.3.4 User Authentication vs. Guest Access 
The app requires a log in depending to connect to MQ servers. The app will also have a guest feature for visitors that dont require login. This give different levels of acess to users depending their login credentials. Visitors for example may only get acess to Main buldings and lucnh spots but not to classrooms and lecture halls. Concurrent processes (if any) and how they will be coordinated
       
#### 2.4 A package diagram showing the subsystems you will use
 ![Package Diagram](/third-party-resources/packageDiagram.PNG)

#### 2.5 Data Definitions 

| Field Name            | Type       | Meaning in Problem Domain                                               | Example of Valid Data               |
|-----------------------|------------|-------------------------------------------------------------------------|-------------------------------|
| `userID`              | Integer    | Unique identifier for each user in the system                           | 1069                     | 
| `username`            | String     | Username for the user                                                   | "Bob"       |
| `gpsOrigin`           | Coordinates (float)  | Starting point GPS location of the user’s requested route, showing latitude and longitude     | "lat": -33.00, "long": 151.1121 |
| `gpsDestination`      | Coordinates (float)  | GPS endpoint of the user’s requested route, showing latitude and longitude                    | "lat": -40.51, "long": 130.3123 |
| `origin`              | String     | Starting point of the user’s requested route                            | "Central Building"       |
| `destination`         | String     | Endpoint of the user’s requested route                                  | "MQ Business Building"      |
| `routeID`             | Integer    | Unique identifier for each generated route                              | 501                           |
| `routeSteps`          | Array      | Array of step-by-step directions for the route                          | "Turn left", "Go straight"  |
| `landmark`            | String     | Major landmark near a route for easier navigation                       | "Central Building"              |
| `estimatedTime`       | Integer    | Estimated travel time in minutes for the route                          | 25                            |
| `distance`            | Float      | Distance of the route in kilometers                                     | 15.2                          |
| `lastUpdated`         | DateTime   | Timestamp of the last update to the route or traffic conditions         | "2024-10-27T14:30:00"         |
| `preferredRouteType`  | String     | Preferred route type (e.g., shortest, dryest, scenic)                   | "Fastest"                     |
| `userRating`          | Integer    | Rating provided by the user for a facility or route                     | 4 (ratings are out of 5)                   |
| `userFeedback`        | String     | Feedback provided by users on route accuracy or convenience             | "Very accurate directions"    |
| `weatherCondition`    | String     | Current weather condition affecting the route                           | "Rainy"                       |
| `safetyAlert`         | Boolean    | Indicates if there’s a safety alert (e.g., road closures or accidents)  | True                          |
| `reRouteNeeded`       | Boolean    | Flag to indicate if rerouting is necessary due to real-time conditions  | True                          |
| `realTimeUpdates`     | Array      | Array of real-time alerts or messages about route changes               | "Construction near 4 Research Park Drive"    |
| `notificationOptIn`   | Boolean    | Indicates if the user has opted in for notifications                    | True                          |
| `friendID`            | Integer    | Unique identifier for a friend of the user                              | 1070                          |
| `friendshipStatus`    | String     | Current status of friendship (e.g., Pending, Accepted, Blocked)         | "Accepted", "Pending", "Denied" |
| `friendAlias`         | String     | User-assigned nickname for the friend, if set                           | "nerd" "bestie"                       |
| `accessibilityOptions`| String     | User-selected accessibility preferences (e.g., wheelchair access)       | "Wheelchair Accessible"        |
| `infrastructureStatus`| String     | Current condition of infrastructure, including route and facility health | "Operational"              |
| `userSession`         | String     | Identifier for the current user session                                | "session_1"               |
| `authenticationStatus`| String     | Status of user authentication (e.g., signed in, access granted)        | "Authorized"                   |

## 3 Analysis and Design

### 3.1 Class Diagram 
 ![Class Diagram](/third-party-resources/ClassDiagram.png)

### 3.2 One or more State Diagrams for the more interesting objects in your design 

 ![State Diagram](/third-party-resources/StateDiagram.png)

## 4 Requirements Traceability Matrix 

| **Requirement ID** | **Use Cases**        | **Classes**           | **Methods**                        | **Packages**           | **Build Number** |
|--------------------|----------------------|-----------------------|------------------------------------|-------------------------|------------------|
| **REQ.UM1**        | Routing A to B       | `MapDisplay`, `Building` | `showMap()`, `loadBuildingDetails()` | `uni.map`, `building`   |                  |
| **REQ.UM2**        | Routing A to B       | `LocationManager`     | `getUserLocation()`               | `location`              |                  |
| **REQ.UM3**        | Routing A to B       | `Navigation`          | `provideIndoorDirections()`       | `navigation`, `location`|                  |
| **REQ.UM4**        | Routing A to B       | `AccessibilityManager`| `getAccessibleRoutes()`           | `accessibility`         |                  |
| **REQ.UM5**        | Routing A to B       | `BuildingSchedule`    | `updateRoutesBasedOnHours()`      | `scheduling`, `routing` |                  |
| **REQ.UM6**        | User Report          | `InfrastructureMonitor` | `updateInfrastructure()`          | `maintenance`           |                  |
| **REQ.UM7**        | Routing A to B       | `CrowdMonitor`        | `adjustForCrowdDensity()`         | `traffic`               |                  |
| **REQ.UM8**        | Routing A to B       | `MapDisplay`          | `showLandmarkPhotos()`            | `uni.map`               |                  |
| **REQ.UM9**        | User Report          | `FeedbackSystem`      | `rateFacility()`, `addComment()`  | `feedback`              |                  |
| **REQ.RT1**        | Routing A to B       | `RouteCalculator`     | `findOptimalRoute()`              | `routing`               |                  |
| **REQ.RT3**        | Routing A to B       | `RouteScheduler`      | `adjustBasedOnTime()`             | `routing`, `scheduling` |                  |
| **REQ.DP1**        | Routing in Rain      | `WeatherService`      | `getWeatherData()`                | `weather`               |                  |
| **REQ.DP2**        | Routing in Rain      | `WeatherService`, `RouteCalculator` | `findDryestPath()` | `weather`, `routing` | |
| **REQ.UR1**        | User Report          | `FeedbackSystem`      | `reportFacilityIssue()`           | `feedback`, `reporting` |                  |
| **REQ.UR2**        | User Report          | `FeedbackSystem`      | `reportRouteClosure()`            | `feedback`, `routing`   |                  |
| **REQ.N01.1**      | Routing A to B       | `RouteCalculator`     | `calculateOptimalRoute()`         | `routing`               |                  |
| **REQ.N02.1**      | All Use Cases        | `SystemMonitor`       | `monitorUptime()`                 | `monitoring`            |                  |
| **REQ.N05.1**      | All Use Cases        | `DataEncryptor`       | `encryptLocationData()`           | `security`, `data`      |                  |
| **REQ.N05.2**      | Authentication       | `AuthManager`         | `signInUser()`, `authorizeAccess()` | `security`, `auth`    |                  |


## List of design assumptions (if any) // together

This will help the reader to understand why you have done certain things. Please review the assumptions carefully before submission. (But note: A poor assumption should not be used as an excuse for poor design decisions.)

## 5 Test
### 5.1 Test Specifications
| Test ID |Test description | Input specifications | Output specifications |
| -------- | ----------- | ---- | ------------------- | 
| TC1.UM01 | Checks that the map is high fidelity|User initiates a request for the map | The system map should ouput all the university building in high fidelity as requested|
| TC2.UM02 | Use GPS to determine accurate location |Accurate user location| The system shall correctly output the users location on the map|
| TC3.UM03 | The System shall use door-door navigation within buildings |Accurate location within a building|The System shoudd be able to diplay the student location within a building |
| TC4.UM04 | Test accessible routes for individuals with disabilities |Given a destination and a handicap request| The system should find a route in which is handicap accesible|
| TC5.UM05 | Building Closing time |BuildingId and Current Time| The system should display the closing times of the Building and how long till it will be closed |
| TC6.UM06 | Update to University Infrastructure | Admin Updates| The system shall provide a notification to the DirectionsMQ and updates routes if needed |
| TC7.UM07 | Validate crowd density display and route adjustment | Crowd density data (location and phone traffic) | The system shall display crowd density within an area and adjust routes |
| TC8.UM08 | Verify photos of landmarks and buildings are provided |The user quick views an image | The System shall give a quick display and description of a building,landmark and restaurant|
| TC9.UM10 | Working custom waypoints on map| User sets a custom weighpoint| A saveable waypoint is outlined on map which can have quick acess to find a route|
| TC10.I01 | The app should connect to the internet | User request System acess | The app should require internet acess as to function and get updates |
| TC11.I02 | The app should be connected through MQ servers | Server URL, port number, and valid credentials |The system should establish a connection to the MQ server successfully |
| TC12.I03 | Check User LogIN e.g(visitor, student admin)| User Submits request to acess App| The App should provide acess to the app based on the credentials |
  
#### 5.2 Test plans
Introduction
The purpose of this test plan is to outline the testing methodology for the DiscoverMQ application. By executing the defined testing scenarios, the system should demonstrate efficient functionality and meet all specified requirements. This test plan aims to identify and rectify code defects, ensuring that the application operates smoothly and reliably for users.

##### 5.2.1 Test Scope
This testing effort will encompass various types of testing, including but not limited to:
  - Functional Testing: Theses will check that the FUnctional parts of the code should fucntion as expected.
  - 5.2.1.2 Performance Testing: To evaluate the application's perfomance and usability.
  - 5.2.1.3 Usability Testing: Evaluate how well the DiscoverMQ usability is. these may include UI and other 

##### 5.2.2 Testing Resources
List the resources required for testing, such as:
  - Personnel 
  - The Testing Requires several testers to validate the test these include 
  - Functional Testesr
      - checks that the application meets the requirements
  - Tools
    - Mapping and GPS Simulation Tools:
    - Performance Testing Tools:
    - Test Management Tools:

Example:

##### 5.2.2 Test milestone
| Milestone                     | Date          |
|-------------------------------|---------------|
| Completion of Test Case Development | week 2 |
| Commencement of Unit Testing   | week 3 |
| Completion of Unit Testing     | week 4  |
| Commencement of Integration Testing | week 5  |
| Completion of Integration Testing | week 6  |
| User Acceptance Testing        | week 7  |
| Final Review and Approval      | week 8  |

## 6 Project Management

### 6.1 Minimal Viable Product 

A description of the _minimal viable product_. This is a version of the product, that is suitable for the client, trusted customers, or early adopter to use for evaluation. Which of the requirements does it implement, and which part of the architecture needs to be in place?

The Minimal Viable Product (MVP) for the DirectionsMQ app is a foundational version that is designed to provide navigation and location sharing features for users at Macquarie University. This version focuses on delivering core functionalities that validate the app's concept and gather user feedback for future enhancements.

1. User Authentication: Requirement ID: R1
Description: Users can create an account and log in securely to access personalized features.
2. Real-time Location Tracking:
Requirement ID: R2
Description: Users will be able to see their current location on the map to facilitate navigation.
3. Turn-by-Turn Navigation:
Requirement ID: R3
Description: The app will offer turn-by-turn navigation from the user's current location to a selected destination on campus.
4. Waypoint Management:
Requirement ID: R4
Description: Users can save and manage waypoints, enabling them to quickly access frequently visited locations.
5. Location Sharing:
Requirement ID: R5
Description: The app will allow users to share their locations with friends within the app, enhancing user interaction and coordination.

### 6.2 Milestones

| Milestone ID | Milestone Description | Target Date |
|--------------|-----------------------|-------------|
| M1 | Project Kickoff: Team formation and project planning, including requirements review and initial architecture discussions. | Week 1|
| M2 | Completion of User Authentication Module: Implement registration, login, and profile management features. | Week 3|
| M3 | Implementation of Map Navigation: Develop the interactive map interface and integrate with mapping API.| Week 5|
| M4 | Real-time Location Tracking: Enable location tracking features and ensure accuracy of displayed location. | Week 7 |
| M5 | Classroom Directions Feature: Implement the ability to get directions to classrooms from the current location.	| Week 9 |
| M6 | Friends Feature Implementation: Enable users to add friends and share their locations.	| Week 10 |
| M7 | MVP Review: Conduct user testing with early adopters, gather feedback, and identify areas for improvement.	| Week 11 |
| M8 | Final Adjustments: Make adjustments based on feedback from the MVP review and prepare for final deployment. | Week 12 |
| M9 | Project Closure: Complete documentation, review project outcomes, and gather insights for future iterations.	| Week 13 |

### 6.3 Tasks 

| Task ID | Description                                                        | Dependencies             | Effort | Milestone                                               |
|---------|--------------------------------------------------------------------|--------------------------|--------|--------------------------------------------------------|
| T1 | Conduct a kickoff meeting to define project goals and expectations. | None | S | M1: Project Kickoff|
| T2 | Develop the user authentication module, including registration and login features. | T1 | M | M2: Completion of User Authentication Module |
| T3 | Design the interactive map layout and integrate it with mapping services. | T2 | L | M3: Implementation of Map Navigation |
| T4 | Implement real-time location tracking functionality to show user location on the map. | T3 | L | M4: Real-time Location Tracking |
| T5 | Develop the classroom directions feature, enabling navigation to specific classrooms. | T4 | M | M5: Classroom Directions Feature |
| T6 | Implement the friends feature, allowing users to add friends and share their locations. | T5 | M | M6: Friends Feature Implementation |
| T7 | Conduct user testing with early adopters to gather feedback on the MVP. | T6 | M | M7: MVP Review |
| T8 | Make final adjustments based on user feedback and prepare for deployment. | T7 | L | M8: Final Adjustments |
| T9 | Complete project documentation and conduct a project closure review. | T8 | S | M9: Project Closure |

### 6.4 Risks 

> Since you do not know how many people work on the project, or what resources you may have, it does not make too much sense to talk about people risk, or estimation risk, yet. Furthermore, if something like a probability is unknown, is better to say that it is unknown, instead of making something up.

| Risk ID | Risk Type             | Description                                                                 | Probability | Severity | Mitigation Strategies |
|---------|-----------------------|-----------------------------------------------------------------------------|-------------|----------|-----------------------|
| R1 | Organizational Risk | Changes in university administration or stakeholder priorities may affect project scope and funding. | Medium | 7 | Maintain regular communication with stakeholders to align expectations and adapt project scope as needed. |
| R2 | Requirements Risk | Changes in user requirements or misunderstanding of requirements may lead to project delays or rework. | High | 6 | Implement a clear requirements-gathering process with stakeholder sign-offs and conduct regular reviews. |
| R3 | Technology Risk | Dependency on third-party mapping API may result in service outages or changes in API terms. | Medium | 8 | Establish backup solutions, such as alternative mapping services, and monitor API status regularly. |
| R4 | Tools Risk | Issues with the development tools or environments could hinder development progress and collaboration. | Medium | 5 | Ensure a stable development environment and provide team members with access to necessary tools and training. |


## 7 Summary and Outlook

The DirectionsMQ project focuses on developing a navigation and location sharing app for the Macquarie University community. The MVP will provide essential features like user authentication, real-time location tracking, and turn-by-turn navigation to meet the basic needs of students and staff.

Moving forward, we will prioritize refining the app based on user feedback from the MVP review, ensuring it evolves to enhance the user experience. Future enhancements may include personalized route recommendations and integration with university events.

Ultimately, DirectionsMQ aims to streamline navigation on campus and foster a stronger sense of community among users, positioning itself as a vital tool within the university environment.

## 8 Appendices
   ### 8.1 Log of interactions with stakeholders
|Date       |Stakeholder |Interaction Summary   |Action Items |
|-----------|------------|----------------------|-------------|
|2024-10-03 | Amit Barua (Group2TeamB1) &  Yash Shitole (Group2TeamB1) | Followed up to confirm design assumptions | Confirm design choices |
|2024-10-10 | Emmanuel Almonte (Group2TeamB2) | Ask about the requirements to confirms when creating test specifications | Take input and translate into the SDD |
|2024-10-17 | Yash Shitole (Group2TeamB1) &  Emmanuel Almonte (Group2TeamB2) | My group went to the two groups to enquire about privacy risk in the design | Create risk according to stakeholder demands |

- References: Software Requirements Specification (SRS) provided by Group2TeamB1 and Group2TeamB2.
- Third-party-resources: draw.io, markdownguide.org


# Note

> Do not forget that we also expect you to complete an individual reflection on iLearn.


