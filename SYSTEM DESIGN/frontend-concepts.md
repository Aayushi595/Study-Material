# MVC Architecture
Model - logical handling , controller - communication / api ,  view - frontend
Flaw - If the feature id big in inplemenatation, then controller also becomes big - Fat Controller problem.

# micro frontend architecture
If we write all the code related to all the entities like customer, cart, orders etc. in a single repo or src then:
For any small change - the whole repo will be built and deployed again, which is not good - breaking Scalability, maintainability.
we break the features into multiple apps/repo like order, cart etc. similar in analogy to backend microservices architecture.
these small apps are contained in a single app called Shell App. 
      - Module Federation binds these different apps together. It is a feature of webpack 5, It says any js code app can load the code of another js app at runtime (not at build time
      )

SHELL app is host app - eg. swiggy.com
      Remote exposes code
      Host loads it at runtime.

Remote apps are the apps that exposes its code to host.

How this code is exposed.
1. On host app, we make a webpack.config.js file. Find file in sc.
2.On remote apps, similarily there is a webpack.config.js file in the remote app also. it tells 
remoteEntry.js file is made when we build our remote app. this files access path is defined in the host webpack configuration file.

# How to cancel previous API requesta.
1. Abort Controller - with fetch, axios, and in useEffect cleanup.
2. Ignore previous responses - not optimized.
3. Debounce - Handling request cancellation in first place.

# Monorepo and Polyrepo architecture 
In Micro-frontend architecture, 
Polyrepo - when we divide the applications in different small apps and make their individual repo whose build and deployment pipeline will be different.
Advantage - Different developers can work on different repo independently.
Disadvantage - If using common component or something, we need to either write code in all the repo or create an npm package for button.
Feasible for small teams

Monorepo - All small apps in same repo. But their build and deployment pipelines are different
Advantage - can use common features seamlessly.

# GraphQL vs REST
REST Architecture - Everything existing on web is resource and for each resiurce there should be an API.
All APIs should ve stateless - Each request is a new request for the server. (server will not store the context of any request)
Problem : overfetching - I want to only fetch specific data of user like address on mobile screen and whole user details on web page but not address. Mobile is getting all the data of user which actually is not needed. 
Underfetching - Also just for address I need to make a different API call.
server decides what to send to the client.
GraphQL - kind of SQL for Frontend, Frontend  decides which data to request from server. No extra data, no extra api.

REST - simple data.             GRAPHQL - complex data
       Limited Queries                    Multiple Clients - Mobile + TV + Web
       Single Client                      Public API
       Internal APIs                      Dynamic data
       Microservices                      Rapid Iteration.


# Cursor Based Pagination vs Offset Based Panigation
offset - fetch data after a specific offset - select * from posts order by created_at desc offset 10 limit 5;
Eg. page number based pagination where user can go from any page 1 to directly 9 by clicking it.
Performance Issue - because if I say offset 10, so the db not simply skips the first 10 rows, it scans the first 10 1rows than discard it.
TC - O(N+K)

cursor - uses reference point, eg if I have 15 enteries in my db, and user asks 1st 10 enteries, db will return 10 entries and also the address of 11 entry. 
Postgres m generally we index Id column thus TC - O(logn).thus this pagination is generally faster than offset based. Used where we don't want to skip any page, we simply need next page or previous page.
Infinite scroll, whatsapp chat.

# BFF
netflix homepage slow on TV - TV - limited Memory and slow process
Reason - multiple datafetching, response aggregation happening on the same page by frontend.

BFF is an extralayer between frontend and backend. request sent to BFF , BFF decides how many api's to call for homepage , do error handling and segregate data, sends optimized response acc to client like web, TV, Tablets.

# Handle slow APIs on frontend for UX
1. shimmer effect
2. Loading
3. App level caching , react query
4. Fetch data in chunks from db, because the DOM tree will be heavy, UI will be laggy, use graphql or service workers for thing thing.


# Monolith Architecture : 
Every service (db connection, backend apis, sms sending, ui, authentication) in same project. Need to deploy the whole project even for small changes. (eg. color change)
Microservice Architecture : (separation of concerns and single responsibility principle ).
different services can have different tech stack.
Can deploy different service on different ports and connects through API calls.


# client-side routing vs server side routing
we can navigate to a new page without reloading the whole page via client-side routing. No network call is made when we navigate unlike in server-side where a new page is fetched via network call from sever.



# System Design
 Design - Requirements, Architecture, Interface
 Components - scalable, secure, maintainable

# Notifications
Push vs In-app vs Local:
- push : Notifications sent from a server to a user’s device (sent via FCM/APNS from server), even if the app is not running.
  appears on device notification tray
- In-app : Notifications or messages that appear only when the app is open.
  App is open . (modals, alerts)
- Local : Scheduled by app on device. (reminders)
  appears on device notification tray

1. react-native-push-notification : client-side library for push notifications + local notifications.
   Does not provide a backend service - You must manage push tokens and send pushes via Firebase/APNS yourself.

2. react-native-onesignal : Push and in-app notifications.
   Provides both the React Native SDK and a backend service.

# Data/State Keeping and persistence: 

question :  where you need to keep that data? Like you need the state / data / token globaaly throughout the app ? 
question  : Do much peristence you need ? 

page refresh, browser close : 
local storage
session storage
Indexed DB
Cookies
state management & state management persistence

cross-device, cross-browser : 
server side storage and events , database, firebase firestore.

# Authentication 

1) Identity validation [SERVER] - server believes the user is valid. 
otp login  - server verifies via stored otp
email / password  -   (Note : this hash is not jwt and this is not what is sent in response after registration or login success)
social login 

with firebase email/password auth : Password hash:  Managed internally by Firebase, unique uid is generated (but its simply kind of user_id)

2) Authentication token / identity token genration and sending to client in response  [SERVER]

- JWT : server creates a token - sent to client inside the response body .
Header : HTTP/1.1 200 OK
Content-Type: application/json

Body :  {
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
      "id": 123
    }
  }

- session_id  : inside the response headers as a cookie
Header : HTTP/1.1 200 OK
Set-Cookie: session_id=abc123xyz; HttpOnly; Secure

with firebase email/password auth : jwt signed token and refresh token is generated and sent in response.
const userCredential = await signInWithEmailAndPassword(auth,email,password); 
const user = userCredential.user;
const idToken = await user.getIdToken();
const uid = user.uid

3) identity token Stored on client side.
localstorage, session storage,  ... etc according to requirement.
Browser stores the cookie automatically when permitted by its cookie policies - in case of session_id returned by server.
JWT or any token - client app decides and stores the token where it needs to be.

with firebase email/password auth :  the SDK manages authentication state and token persistence for you. bydefault it is localstorage persistence.

4) Next requests - client side . client attaches token in requests

JWT : Client app through code attaches this token to the request headers.
Request Header : GET /api/profile HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

Cookie / session : The browser usually attaches the cookie automatically when cookie policies allow it.
Request Header :  GET /api/profile HTTP/1.1
Cookie: session_id=abc123xyz

with firebase email/password auth :  Calling your own backend - you explicitly attach the Firebase ID token to the request.

5) Request validation and response [server side]
JWT :  Jwt is never stored anywhere on the server. 

server recalculates :
jwt expected signature  =  header from the token received in response . payload from the token received in response . signature (calculated with server decided algo and secret_key)
Incoming token === jwt expected signature

session_id :  The server stores session id and corresponding data - db / redis /  etc.

with firebase email/password auth :  Calling your own backend - our Node.js backend can verify it using firebase-admin/auth.

5) How much time the user will remain logged-in ? 
based on the persistence power of where you stored (localstorage, sessionStorage etc.)   +  additional expiry and refresh token kind of thing.

Expire a token : 
server can send expiry time  and app codes accordingly.
server can send refresh token - client uses it to obtain new access token without being logged out, along with access_token / identity token.

{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "expires_in": 900,
  "refresh_token": "refresh_xyz...",
  "refresh_expires_in": 604800
}

when refresh token expires, the user is logged out.
Ex : setTimeout(async () => {
  await refreshAccessToken();
}, 14 * 60 * 1000);


# Using Third-party
configure our app on the platform
The platform provide tokens for your app
Intitialize the third-party sdks from your app passing those tokens.
The initialization returns a 
