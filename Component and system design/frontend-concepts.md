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