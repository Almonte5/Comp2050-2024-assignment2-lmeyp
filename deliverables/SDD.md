# Software Design Document

> Use this markdown file to create the report. For the submission print this document to a PDF, and upload it to iLearn. The maximum number of pages is 30.
> Use the document structure below.

## Structure

## Title Page

 - DIrectionsMQ SDD

 ### Members
 - Latthasay Saisanavong
 - Yuvraj Gill
 - Ethan Almonte
 - Sonny Sayaloune

## Vision statement  

DirectionsMQ will serve as a comprehensive naviagation tool for students, teaching staff and visitors at Macquarie University. Providing seamless, realtime navigation, DirectionsMQ is designed to help users naviagate
through the enormous campus and help them get to their location as fast, and with less trouble as possible.


## System Design Document 

This will include the basic architecture of the system and the high-level strategic decisions. You need to include a description of the:

### System architecture
#### 1. Overview
The DirectionsMQ system will be composed of different subsystems that will interact with one another to provide users with turn-by-turn directions, real time updates, accessible routing and customised features like "dryest path" routing. The architecture will be based on a layered model meaning that both mobile and backend components will be used to support functionality like GPS tracking, database management and real time route optimisation. The two main components is the **Client App (Frontend)** and **MQ-Directions-Service (Backend)**, **Database** and external services for weather and live updates.

#### 2. Components
##### 2.1 Client App (Frontend)
This is the main mobile app that will be installed on user devices, it iwll be responsible for user interface, GPS tracking and local storage. They key modules will include:
- Routing Module: calculates and displays routes within the app also providing a turn by turn guide
- User Interface (UI): Displays the map, directions and information on Points of Interests (POI) like Central Courtyard.
- Offline Mode: Allows for users to download portions of the map and relevant information 
- Feedback & Reporting: Collect and sends the users feedback for things such as reports of route issues, directly to the backend.

##### 2.2 MQ-Directions-Servcse (Backend)
The MQDS server is the core of the system managing things such as route calculations, data storage and interfacing with the Directions MQ app. The key Modules will include:
- Routing Engine: Responsible for calculating optimal and accessible routes that will also factor in real tie updates like weather and contruction.
- Dry Path algorithm: Calculates routes with minimal exposure to rain.
- Real-Time Update processor: Revcieve and integrate data on building opening hours, crowd density to optimise routes.

##### 2.3 Database
The database is centrol for stroing campus maps, user reports, crowd density, route data and user preferences. Kep elements incldue:
- Campus Map & Point of Interest (POI): Stores all map data including building layouts, rooms and waypoints.
- User prefrences and Data: Contains all of the users customized settings like prefered routes and saved locations.
- Real Time data: Hold temporary data on any live updates like closed rooms and crowd density.

##### 2.4 External Services
The system will have a few key External Services which will inclde:
- Weather: Providing real time weather data for dry path calculations
- MQ Authentication: Enables for secure single sign in for Macqurie staff, studends and other users.
- MQ Campus updates: Supplies regular updates on campus events construction and other obstacles.



- Storage/persistent data strategy


- Noteworthy trade-offs and choices
- Concurrent processes (if any) and how they will be coordinated
- A package diagram showing the subsystems you will use

## Data Definitions 


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





<<<<<<< HEAD
## Analysis and Design
=======

### Analysis and Design
>>>>>>> 3c8e1262051f5bdf26f164ce476794dccee50577

### Class Diagram // Alvin

You need to do an initial design of your system -- what basic objects should it have? And what are the methods associated with those objects? You will represent your design decisions in a class diagram. In a full plan, you need to make sure any classes or methods in any sequence diagrams have been included in the class diagram -- it might help you to draw some sequence diagrams to help you to decide what your class diagram should contain. Method signatures should be given. The diagram must include, as appropriate classes, attributes, associations, inheritance and/or aggregation (if applicable) and multiplicities.

### One or more State Diagrams for the more interesting objects in your design //sonny

State Diagrams: You are required to consider the relevant states of each object in your system and to submit state diagrams for those that have interesting states or complex behaviour. One way to measure if a state is interesting is to consider whether you need to test that state before performing a particular action or if the state changes after an action is performed. What is interesting will depend on the application.

## Requirements Traceability Matrix //ethan

Requirements Traceability Matrix (RTM): Set up an RTM with the following columns:

- Requirement-ID (from SRS)
- Use Cases
- Classes
- Methods
- Packages
- Build Number (kept blank at this stage)

> There should be one row for each requirement. For this deliverable, just fill in the first five columns, since the last column (and usually a couple more after that which I've already deleted) are concerned with the design of the system.

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

## Test specifications 

| Test ID |Test description | Input specifications | Output specifications |
| -------- | ----------- | ---- | ------------------- | 
| TC1.UM01 | Checks that the map is high fidelity|User initiates a request for the map | The system Map should ouput all the university building in high fidelity as requested|
| TC2.UM02 | Use GPS to determine accurate location |Accurate user location| The system shall correctly output the users location on the map|
| TC3.UM03 | The System shall use door-door navigation within buildings |Accurate location within a building|The System shoudd be able to diplay the student location within a building and |
| TC4.UM04 | Test accessible routes for individuals with disabilities |Given a destination and a disability flag| The system should find a route in which is handicapped accesible|
| TC5.UM05 | Building Closing time |BuildingId and Current Time| The system should display the closing times of the Building and how long till it will be closed |
| TC6.UM06 | Update to University Infrastructure | Admin Updates| The system shall provide a notification to the DirectionsMQ and updates routes if needed |
| TC7.UM07 | Validate crowd density display and route adjustment | Crowd density data (location and phone traffic) | The system shall display crowd density within an area and adjust routes |
| TC8.UM08 | Verify photos of landmarks and buildings are provided |The user quick views an image | The System shall give a quick display and description of a building,landmark and restaurant|
| TC9.UM10 | Working custom waypoints on map| User sets a custom weighpoint| A saveable waypoint is outlined on map which can have quick acess to find a route|
| TC10.I01 | The app should connect to the internet | User request System acess | The app should require internet acess as to function and get updates |
| TC11.I02 | The app should be connected through MQ servers | Server URL, port number, and valid credentials |The system should establish a connection to the MQ server successfully |
| TC12.I03 | Check User LogIN e.g(visitor, student admin)| User Submits request to acess App| The App should provide acess to the app based on the credentials |
| TC13.I04 | 
  
- Test plans
Introduction
The purpose of this test plan is to outline the testing methodology for the DiscoverMQ application. By executing the defined testing scenarios, the system should demonstrate efficient functionality and meet all specified requirements. This test plan aims to identify and rectify code defects, ensuring that the application operates smoothly and reliably for users.
Scope

This testing effort will encompass various types of testing, including but not limited to:
  -Functional Testing: Theses will check that the FUnctional parts of the code should fucntion as expected.
  -Performance Testing: To evaluate the application's perfomance and usability.
  -Usability Testing: Evaluate how well the DiscoverMQ usability is. these may include UI and other 

Testing Resources
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

- Test milestone
  
| Milestone                     | Date          |
|-------------------------------|---------------|
| Completion of Test Case Development | week 2 |
| Commencement of Unit Testing   | week 3 |
| Completion of Unit Testing     | week 4  |
| Commencement of Integration Testing | week 5  |
| Completion of Integration Testing | week 6  |
| User Acceptance Testing        | week 7  |
| Final Review and Approval      | week 8  |

  
- Test plans, including for example a test schedule, testing resources required, testing milestones and test deliverables. Test plans, covering scheduling and resourcing of all testing processes. Test plans can be more open format and should provide a description of how you would organise the actual testing of the Test Case Specifications that you've identified.

> The test specification section should cover at least one-third of the report.

## Project Management

### Minimal Viable Product // yuvi

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


These features will need to be supported by the following architecture components:
- A user authentication service for secure login and account management.
- A mapping and geolocation service to enable interactive map features and real-time tracking.
- A backend server to handle data storage and processing for user accounts, waypoints, and location sharing.


### Milestones // yuvi

A description of the main implementation milestones, in the order in which they should occur in the project. A milestone marks the end of a stage in the project when a version of the product can be reviewed as a whole.

The Milestones for the DirectionsMQ project outline key stages of development, marking the completion of essential features and readiness for review. Each milestone reflects a significant achievement in the project timeline.

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

### Tasks // ethan

Describe the main tasks that need to be completed, in the form of a table. The table should include

- An ID for the task
- A description of the task
- Dependencies, i.e. tasks that need to be completed before this task can start.
- Effort. Since you do not know how, or even how many people work on the project, it does not make sense for this assignment to estimate workdays. Instead pick a suitable scale (S, M, L, XL or 1 to 5 stars or ...)
- Milestone. Which milestone do they belong to?

### Risks // ethan

A table with the following types of risks
- Organizational risks that come from changes in the organizational environment. Think of changing stakeholders or management, or a change of mind of stakeholders or management.
- Requirements risks, that come from changes to the requirements, or wrong requirements, and the process of managing requirement changes.
- Technology risks that come from the software or hardware technologies that are used by the system. **Include here parts of the system that you may need from the team that works on the other half of the system, or parts that you both depend on.**
- Tools risks that come from the software tools and other support software used to develop the system.

For each risk include

- An ID
- A description of the risk.
- The probability of that risk happening (use an appropriate scale: low to high, or 0% to 100%, or ...)
- The severity of the risk (use an appropriate scale: none to catastrophic, or 0 to 10, or ...)
- Mitigation strategies. Suggest measures that can be taken to reduce the risk.

> Since you do not know how many people work on the project, or what resources you may have, it does not make too much sense to talk about people risk, or estimation risk, yet. Furthermore, if something like a probability is unknown, is better to say that it is unknown, instead of making something up.

## Summary and Outlook // yuvi

Your famous final words.

## Appendices

- Log of interactions with stakeholders.
- References.
- Third-party-resources

# Note

> Do not forget that we also expect you to complete an individual reflection on iLearn.


