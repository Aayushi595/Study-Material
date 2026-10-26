
- Web APIs (browser APIs - provided by browser)
DOM API - document, window,Element, Node, HTMLElement,document.querySelector()
Network API - XMLHttpRequest, EventSource, WebSocket
Storage APIs - local Storage, Session Storage
Event APIS - addeventListener, removeEventListener
Default events listened by browser - click, hover, mousemove.
Geolocation API - history, location, navigator
Rendering API - 
Media API

# rendering website in browser - rendering pipeline / critical rendering path 
browser download and parses html => create nodes for every tags => these nodes are structed in the form of tree where root is Document > Html =>this tree is DOM (Document Object Modal) => browser parses the CSS and buids a style tree called CSSOM => matches css rules to the corresponding HTML elements => decides which styles apply to which tags - like div, p, body etc => constructs a treee for this known as render tree => Reflow( calculates the position of every element on the screen eg. An element is 100px , but where exactly it will appear on screen (co-ordinates on screen)?) => Paint (generates pixel in memory for each element) => Composite (when user actually sees the first visible content) the browser uses GPU to combine layers created during paint and 1st frame is pushed.

# Critical Rendering Path
All those steps which are involved, which causes my 1st pixel to be visible on screen.


# Web Vitals - performance of a website - performance debugging - Performance tab
Three metrics defined by google for performance of website, these vitals are core web vitals - 
LCP (Largest Contentful Paint) - measures time from navigation start until the largest element is visible on screen - Usually Header or Hero Image. should be 2-2.5s. otherwise the website will feel slow as the largest elemt is not visible fastly. 

Improvements - preload for important or large image improves LCP.
SSR improves website ranking my optimizing performance.

CLS (Cumulative Layout Shift) - 0.25 tk okay , >0.25 is bad

INP (Interaction to next paint) - it measures responsiveness during user interaction , not load time. it measures the time for a change to reflect on user interaction.
should be less than 200ms ,  if > 200ms , than our main thread is busy, Interaction will feel delayed.

# Debugging of website performance in Network Tab
In network tab there happens - Document Request, css request, js request, waterfall Analysis.
Requests starting late are usually depenedent on other.
TTFB - Time to First Byte - How long it takes for the server to send the first byte of a response after browser makes a request. If ttfb is high, problem is with the backend server.

Document Request - first request for fetching the html. Then css and js requests are done. So if the size of my html bundle is 2-3 MB (high), then browser need to download and parse bigger bundle .
Minimal Bundle size == high performance


# Prefetch , # Preflight #Preload, CORS
Preflight - Request sent by browser to the API / server to avoid cors issue, to ensure the request can be sent to that server from this domain. Eg. Flipart frontend cannot call Amazon API (private).
The server responds with allowed origins, methods and headers.
If eerything is fine than only browser sends actual request to server else blocks the request. 

Preload - normally browser parses html line by line , as soon as any resource is found, it starts downloading it . we use :
<link rel="preload" href="/images/hero.jpg" as="image"> in head tag
some html
<img src="/images/hero.jpg">

Prefetch - Make future navigations fast.
Normal flow : 
User opens Home page => clicks About Us => Browser send request for about page => JS bundle downloads => Page Renders

If we add in hoempage : <link rel="prefetch" href="/js/about.js">
Brower downloaded about-us page in idle time. or with low priority that why these don't disturb the page performance.

# Functional and non-functional requirements
Functional - features
Non-functional - which makes existing features better, more about optimization , like scrolling should be smooth, videos should load fast,app handles millions of user traffic.

# Client Side Rendering vs ssr

Browser sends an empty html to server with a root div, then server responds with a blank html and js bundle file. The jsx we write in React, compiler converts it into a JS functional call : React.createElement() 

Now 0ms : Browser received empty html screen is completely whiet
100ms : bundle.js downloding
400ms : JS is executing (Screen : Slight flicker)
600ms : HTML Starts filling up
800ms : API Call fired.
1200ms : Data received - page ready.
This shoots down website ranking. 

ssr : The creation of html using bundle.js, instead of happening  on client side should happens on server.


# Hydration
when ssr do the job of html structuring then initially when the page is rendered, the interactions are not there like input box is there , but there is no onChange of input box, submit button is there but there is no onClick of submit, because it only sends structure and not the event listeners.


100ms : HTML rendered (page is visible but like dead)
       JS Bundle Downloading
200ms : css fully applied

300ms : JS Bundle download cmplete
310ms : React reconciles the page
Browser does again creates html or render tree but before attaching it compares the dom structured by server and current dom node by node , if structure is same then it attaches event listeners to existing node only.

profit : No extra node attaching => no structure change => so browser don't have to do layouting again => no repaint required. these costly steps of rendering pipeline are skipped. and this process is called hydration.


# CORS
browsers didn't allowed sharing resource or data for between different origin or same origin with different port.
aayushi.in <---> google.com X
aayushi.in <---> api.aayushi.in X
aayushi.in <---> aayushi.in:5050 X
https://aayushi.in <---> http://aayushi.in/ X

For the sake of Microservices architecture, browsers support cors through a mechanism:

1. A preflight call() is made by the requesting server/origin, containing additional http headers to verify as a valid call.
2. REquested sever again sends some additional http headers so that the client knows the response.
   for public api, the server responds [accept-control-allow-origin : \*]
   for private api, when preflight call is success [accept-control-allow-origin : https://aayushi.in]
   accept-control-allow-methods for restricting put, post and other method.

# Caching Strategies and session / web and mobile - what are the options when to use what.

localstorage / IndexedDB : provided to each website.
session expires - when user manually signOut(auth),  browser data cleared. If we login/logout in 1 tab, you will automatically loggedin/loggedout in another tab.

sessionStorage - provided to each tab, even if same website is opened in multiple tabs.
User stays loggedin after page refresh. - saving form data.

no persistence : setPersistence(auth, inMemoryPersistence); -> tokens are stored in JS memory - lost on page refresh, Cleared when tab/browser closes

cookie storage : Superpower - server also can directly add data with the help of browser, set-Cookie attribute in response header and maps domain to it (from where is the response coming)
now the browser automatically will attach that data in the cookie header for next call to that domain.
Http only : attribute sent by server in response header with set-cookie, which means, cookie can be modified/that data can be accessed only by server and not by JS in browser. JS can not even access HttpOnly cookie - This way all the attacks possible through JS are failed. Authorization token is generally stored in the http-only cookies.

# Http and Websocket
Websocket - Connection protocol, having rules, status codes, data flow architecture.
Http - client sends request to server, server process request and gives response , then connection is closed. Analogy : A tunnel created for each request, and tunnel exhausts after server response. Data flows in one direction (server respponses to client)
Websocket - Permanent tunnel between client and server - persistent. Data flows in both directions. We need to close the tunnel explicitly. ex- chat applications
For a websocket connection in browser, we use Websocket API (Ananlogy : fetch for http).
  - At first http connection is established between server and client with special headers being attached by browser - Connection : Upgrade, Upgrade : websocket, sec-websocket-key.
  - server recognizes the headers , upgrades the connection from http to websocket and responses with 101 status code. 101 status code means switching protocol.

# server sent events
Normal Http connection + 1 special header (text/event-stream)
Client requests once -> server keeps connection open -> server pushes data whenever ready.
Ex - chatgpt - response is sent words by words as content is generated.