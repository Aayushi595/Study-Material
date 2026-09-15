# Module System 




# Why react components can directly use import/export ?
    React apps do not run directly in Node.js - Parcel or Metro or Webpack or Vite compile your JSX/TSX into normal JavaScript before Node or the browser ever sees it. React Native bundler (Metro) + JS engine (Hermes/JSC).
    note : App.js is processed and run by the React Native bundler (Metro) + JS engine (Hermes/JSC).

# Execution














# REndering and Performance 












# Why REact fast? 






# Identification as React Component
1.  <MyComponent /> - this syntax
    Note : React components only need to return a React element, or null, or string, or number, or array, or portal, or even nothing.
2.  wrappings (navigator etc.)
3.  React will treat <login /> as a custom HTML tag, not a component. - capital initial letter for react component

# React App setup related
after the configurations it asks for, the package.json is created

Package.Json is configuration for npm. Require the manage and use the dependencies we imported in our package. keeps the approx version of deps.
package.json : {
name - name of your package/app
version - of app (major.minor.patch)
main - This tells Node.js which file to load if someone runs : require(learn-react) - usually points entry point
scripts - defines custom command-line shortcuts that you can run with npm run <script-name>
}
Basic dependencies and packages for our react app ; bundler

Bundler - WebPack, Parcel, Vite. Bundles the apps, cleans and caches the app, minify/compress this app, before running or shifting app to production
when we write : npx create-react-app learn-react - this uses webpack & Babel (translator/JS compiler) behind the scenes.

npm install -D parcel - Dev Dependency - required for developement phase - this will come in devDependencies object .  
version of dependency : ^ - means whenever there's available a new minor version of dependency , it will be installed automatically, it in package.json - the value will stay as it is.
~ - means in case of upgrade, our app will install newer major version of dependency automatically.
no symbol - means we don't want any automatic update

package.lock.json - npm install some-package - locks the version of dependencies and keeps the record of exact version of the dependency to which it is upgraded.
intergrity - is a hash stored, which checks the version on local and the production , it should be the same

node-modules - npm install parcel - installed all the modules from parcel and the dependencies which parcel depends upon(transitive dependencies) and put it into node_modules.

1. we need to put package.json and package.lock.json, to match the dpendencies and version with production's
2. We must not put node_modules to git because with the help of package.json and package.lock.json these dependencies get installed.
   -If I delete node modules and then simply do npm install, It will install all the dependencies reading package.json and package.lock.json

npx parcel 01_Inception/index1.html : at this time .parcel-cache folder is created.

# Parcel
Parcel is a bundler that does :

- parcel goes to index.html (entry point) and builds a dev build for our app - involves compilation, translation : TS --> JS
- hosts that build to our local server.
- HMR = Hot Module Replacement - refreshing page on save (using File Watching Algo - written in C++). It builts app on every save .
- Caching (parcel-cache folder) - Faster build times : but takes lesser time than before on each save. 495ms is lesser time
- Image optimization - most expensive thing in browser is to load images.
- Minification , Compression(eg. remove white spaces etc.), of files in production build.
- Consistent Hashing
- Code Splitting.
- Differential Bundling - support older browsers.
- Better Error suggestions
- host app with https also. npx parcel 01_Inception/index1.html --https . Parcel (like most dev servers) generates a self-signed SSL certificate for local HTTPS use. But it's not signed by a trusted Certificate Authority (CA), so browsers mark it as not secure or untrusted.
- Tree shaking - remove unused codes, unused exports.

npx means executing a package.


# why do we add browserslist to config ?
If we don't, parcel will look for taking care of all the versions and create multiple bundles.

# Ways to get react into our app :
1. cdn links
2. npm install react - we are not gonna install react as dev dependency.
3. npm install react-dom
4. import ReactDOM from "react-dom"
5. npm list react-dom
   npm list react-dom --> npm uninstall react react-dom (if wrong version)
   npm install react@18 react-dom@18 - version specific install

# JSX 
JSX - Javascript Xml - JS syntax sugar which makes easier to create react elements instead of the older way of writing react elements.
And we can write dom elements and js simply in .js file.
We can say JSX an invalid js because the js Engine or browser don't understand it.

# HTML/JSX TAGS AND ATTRIBUTES
In JSX, you use curly braces {} to insert JavaScript expressions.
A self-closing tag is an HTML/JSX tag that doesn’t need a separate closing tag because it doesn’t wrap any content.

JSX Props:
1. tabIndex -
   controls keyboard focus on tab.
   tabIndex={0} : Make it tabbable in natural order
   tabIndex={-1} : Remove from tab order, but still focusable by JS.
   Some HTML elements are naturally focusable: <button>, <a href="">, <input>, <textarea>, <select>
   Others (like <div>, <span>) are not focusable unless you give them a tabIndex.

# React/React.js
React is a JavaScript library for building user interfaces using components. React is a library and not framework because React only focuses on building UI components, not the entire application structure.

# React Benefits
1. we can navigate to a new page without reloading the whole page via client-side routing. (react nativation) - single-page-application
2. Efficient component rendering using reconciliation algo. Fast and efficient dom manipulation.
   - How React Works? 
       UI is broken into reusable components.
       Components manage state and props.
       React compares Virtual DOM vs Real DOM.
       Only changed parts of the UI update.

3. Batches the rendering on child component for optimization.
4. Sanitizes data which is executing inside the React Component / JSX - Avoids cross-site scripting.
   (Malicious JS Code that intends to execute on server(where our application is hosted) through API response or user Input)
5. Performs html syntax validation : <h1>Having child H2<h2>H2 inside h1</h2></h1> - validateDOMNesting(...): <h2> cannot appear as a child of <h1> - Because of React's internal HTML Validation.

# React Execution outcomes
console.log('1234') returns undefined (technically, void).
React DOM skips rendering anything for undefined, null, or false values in JSX, in browser. (ReactJS)
React Native is strict about what can be rendered inside <Text>.It expects a valid string, number, or another <Text> — not undefined.
Note we can't even call a function in ReactNative JSX, that returns void.

=> when you put an array directly inside JSX, Array.prototype.toString() on it — which joins elements with commas by default (["North Indian", "Chinese"] → "North Indian,Chinese").

# React Element 
JavaScript object, that will be converted and rendered as DOM Nodes, can be written in JSX syntax or using core React library method(createElement).
Its basically building blocks of a react component.
render an element : root.render(jsxhead)

# React Component
React Component - A function or class that returns React Element. / JSX. Write name in PascalCase.
types - Functional or Class Based
Functional Component : Normal JS function that returns JSX.
Render a component : root.render(<FunctionalComponent />)
We can say, Everything inside React is a component.

Functional Component sctructure and navigation ---------------
const PackerLineAssign: FC<Props> = ({ navigation }) => {}

# Functional Component :
FC : A generic type exported by React.Short for FunctionComponent (alias: React.FC). Tells TypeScript: “This component is a React functional component.”
<Props> : The generic type argument. You tell React what shape of props your component expects.
If your component is registered as a screen in a navigator, react automatically injects two props by default.
1. navigation : object to navigation (navigate, goBack(), reset)
2. route : route info (params, name etc.)

passing params when navigating : navigation.navigate("Profile", { userId: 42 });
We cannot directly pass custom props during navigation, if your component is registered as a screen in a navigator, react automatically injects two props by default. (navigation, route)

# React Code Execution
IMP : JSX/other ES6+ features => Parcel => Babel(Transpiling converts modern JavaScript (ES6+, JSX) into browser-compatible JavaScript (usually ES5)) => bundling happens.
Behind the scenes:
const heading = <h1>Hello</h1> => React.createElement("h1", null, "Hello") => ReferenceError: React is not defined
(get transpiled to, by Babel, even without React, ) (Without React)
JSX => React.createElement('div', {id : 'head', xyz : 'abc'}, 'Hello') (element object) => HTMLElement
(With React)
const jsxhead = <h1>hello jsx</h1> and const heading = React.createElement('div', {id : 'head', xyz : 'abc'}, 'hiii') logs the exact same obj.
IMP : .jsx and .tsx must only return a JSX, so we can only have a JS Expression inside anywhere in jsx which should be wrapped in {2+3}.

# Hooks. Normal js utility functions
state Hooks : useState - keeps the state of component. (scope of local state variable -component)
useEffect : Is called after every re-render- if dependency array, executes its callback after the first render (UI) and change in deps. Best for API calls. So if I had to do something after rendering the UI, we use UseEffect.
approch 1 (inefficient) : page load -> api call -> render UI
approch2 : (efficient) : page load -> render ui -> api call -> re-render.

useState hook : How a const variable is getting updated in local state variable.?
The same variable is not getting updated, when we do setState(), the component re-renders and this time, the local variable is a new instance/reference, and its default value is the value which we set.

# React Native App optimization 
3 pieces : 1. Component should load and reload faster.
            2. Re-render faster / faster updates.
            3. Should not re-render unnecessarily.
1. Rendering and UI
   - Load components or libraries only when required - using code splitting and lazy loading, use specially in tab views.
   - Preventing unnecessary re-creations on state updates - using memoization hooks useCallback(), useMemo(), useRef() .
        - Especially if we pass these functions as child props 
        - Component Memoization : React.Memo() : An HOC (Higher-Order Component) is a function that takes a component as input and returns a new      component with enhanced behavior. HOC = Component Wrapper Function. Use React.memo for child components that don’t need to update on every parent render.  
   - Use and update state variables only where it is required. - #Component State

   # Element Level
      1. Lists : 
      - Use Flatlist instead of ScrollView for long Lists. 
         # Problem with ScrollView 
           ScrollView renders all children at once. If you have 1000+ items, it tries to put all 1000 into memory and DOM/Native UI tree → lag, memory issues, crashes.

         # Flatlist advantage
           Flatlist by default do windowing and recycling, removing clipped subviews. For more efficiency we can props below : 
           windowSize={10}  - 
           initialNumToRender={10}  - 
           removeClippedSubviews    -  Unmount views that scroll out of the window (performance boost).

      - Use pagination for lists.

      - Use Unique id for keyExtractor to avoid complete list rendering and optimize reconciliation process.

      2. Images 
      - Fast Image - Key Benefits: 
        1. Caching : Automatically caches images (memory, disk, or both), Avoids re-downloading images when scrolling or reopening screens.
        2. Priority Loading  : You can set high, normal, or low priority for images, so critical images load faster.
        3. Resize and Scale Handling :Handles resizing efficiently, reducing memory usage.

   - Utilize profiler tool for js thread optimization and component.

2. User Experience Optimization
   - Fast rendering - #Rendering and UI
      - Use react-navigation with lazy: true for tab screens → screens load only when visited.
      - Component State 
         - Only store what is necessary in the component state, don't use for calculative/derive values. 
               Reason - Inconsistency. if the variables we used in derivation are updated later than we need to also update the derived variable - unnecessary state update else inconsistency.

         - Use functional updates when new state depends on previous state to avoid stale closures.
         - Instead of one large state object, split into multiple states for independent parts of the UI.
                    
   - Use Shimmer Effects
   - Fast Image - Image resize before rendering - Placeholder images.
   - Debouncing & Throttling Inputs - Prevents excessive API calls while typing/searching.
   - Offline Support (Caching, AsyncStorage, SQLite). Store last-fetched data for offline access.
   - Good styles and required animations - proper layout design - Flexbox grid.

3. Memory Optimizations
     #Rendering and UI - clear events properly

4. Code Optimizations - Code Scalability
   Code should be simple to (scale) understand and extend in terms of features and modules, debug   -   modular code.
   1. Single Responsibility Principle - Each component should do one thing - separate UI, business logic, and API calls   -  Easy to debug and understand.
   2. Use Custom Hooks - Extract reusable logic for better maintainability and debugging.
   3. Organize code into components/, hooks/, services/, screens/, utils
   4. Build resuable components for common features like dropdown and modals.
   5. Proper Try Catch and Error Logging - Type Safety
   6. Define Constants in separate files, eg. colors, testIds.
   7. Use meaning names and same naming convention for same category througout the app.
   
# Global State
   Share state across multiple components, module screens without 
    - props drilling for deep nested child components
    - Passing navigation params when screen navigations happens from one screen to another.

   Context API - light weight state across module.
  # Zustand Example and Use Case : 
   Step 1: Create the Store - src/store/scanStore.ts
     import { create } from 'zustand';

      type ScanStore = {
      onCapture?: (value: string) => void;
      setOnCapture: (fn: (value: string) => void) => void;
      clearOnCapture: () => void;
      };

      export const useScanStore = create<ScanStore>((set) => ({
      onCapture: undefined,
      setOnCapture: (fn) => set({ onCapture: fn }),
      clearOnCapture: () => set({ onCapture: undefined }),
      }));

   Step 2: Save from One Screen 
      import { useScanStore } from '@/store';
      import { useNavigation } from '@react-navigation/native';

      export default function ScanStarter() {
      const setOnCapture = useScanStore((s) => s.setOnCapture);
      const navigation = useNavigation();

      const handleCapture = (value: string) => {
      };
      }

   Step 3: Access in Another Screen
      import React from 'react';
      import { useNavigation } from '@react-navigation/native';

      export default function ScanStarter() {
      const setOnCapture = useScanStore((s) => s.setOnCapture);
      const navigation = useNavigation();

      const handleCapture = (value: string) => {
      };
      }
   
   # Context API Use Case
  A warehouse user has both the roles (moderation_manager, normal receiver), In this case,  if the user navigates to internal screens / sub-modules, we need to keep this info as state at module level, so that we can pass params into the Api on each screens, So that the backend system and logic could know, how exactly is the user requesting the resource.

# React.js - make production build
make production build : npx parcel build 01_Inception/index1.html  
dist folder contains the build files and we dont push to git , these are regenrated.
dist/ folder contains plain HTML, CSS, and JS files. so the thing we deploy is plain html/css/js which does not require anything to be installed or any command to run to be run on server.

# Structure Multiple API Calls and data fetching on a screen.
# Mobile App Developement Life cycle team co-ordination.
  1. Requirement discussion with frontend + backend mates handling different services and task allocation.

  2.  Development Phase 
      UI/UX Design 
        - layout - navigations - UI on state updates and user Interactions, Visual Representation.
        - Develop and unit test cases and interactions with Mock data
      
      Backend services developement 
         - Decide the architecture pattern (MVVM, MVC, Redux, BLoC). Define database schema, APIs, and backend requirements.Choose third-party integrations (payment, analytics, push notifications) -> write code.
         - Unit test APIs in PostMan with mock data.

   3. Integration Phase : Frontend + Backend  integration by Frotend Developer.
      Unit Test.

   4. Service Deployment Phase - Testing Servers

   5. Technical Testing 
        - Any issues -> issue report to frontend  , backend developer.
   
   6. Enhancement and Fixing -> unit testing -> testing.

   7. Functional Test

   8. Build Launch then Beta Testing. 

   9. Maintenance and updates
   - Tip : Always try to build the functionality first, intead of just trying to correct and enhance the UI

# Closure Applications
  - When we pass in function reference from parent to child for any event in child. On the event call, executes the parent’s function.  Beacause of Closure the  function still has access to the parent’s state and variables at the time it was created.


# dynamic width vs transform 
  Due to dynamic width the component is rebuilt, but with transform : translate(), it simply slides over the required part.

# Utils folder vs custom Hooks vs services
  - Utils are meant for writing pure JS Helper functions, it must not import React/use hooks so it does not depend on react state or life cycle.
  - It can be used anywhere frontend, backend, testing logic (eg. loginHelper)
  
  - Hooks are write the react logic which includes managing states, context,refs.
  - Only to use inside React Functional components, other hooks.

  - Services are meant for handling external interactions (like APIs, localStorage, or backend).


# Challenges
   - Issue in state set and incorrect UI update  -  React batches state updates and causes re-render after function execution completes + related functions using stale closures (older reference or value)

# Challenging Projects, Resolution

# Arrow function vs Normal function
  - Binding of this : Regular function have their own binding of this, arrow functions use their lexical scope for this (value of this in lexical scope = value of this inside arroe function, where it is written) Note : Object is not a scope.


# State update within batch
second update waits for the first to complete entirely - they run synchronously within the batch execution.
State updates are enqueued in React’s internal Fiber update queue.


# App heirarchy and libraries
Index.js - react-native-gesture-handler : for gestures like navigation, touch, taps - simply import at top-level of file.
Need to install this library manually.

App.jsx - app-level initailizations inside useEffect , no deps - app-level exports like BASE_URL. - App Wrapping in JSX

ThemeProvider : Styled-components is a library that lets you write CSS inside JavaScript to style React Native components.
PaperProvider: React-native-paper is a UI library that implements Google Material Design using React Native.it gives you ready-made, customizable UI components like: <Button />, <TextInput />, <Card />, <Dialog />

Core RN Layout components.
   Layout components:
   View
   Text
   Image
   ScrollView
   FlaList
   Pressable
   TouchableOpacity
   SafeAreaView


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


# Non-screen components 
import { useNavigation } from '@react-navigation/native';
const MyComponent = () => {
const navigation = useNavigation();
return <Button title="Go" onPress={() => navigation.navigate('Home')} />;



# Context API vs Redux vs AsyncStorage
AsyncStorage/session storage, IndexedDB, cookies, redux-persist - state persistence across re-renders.
Context api - Lightweight state keeping APi for sharing state without prop-drilling. => keeps module level global state variables
We can use a context for multile state, but:
   Component A:
   const { lang } = useContext(LanguageContext);

   Component B:
   const { theme } = useContext(LanguageContext);
   👉 Changing lang will re-render BOTH A and B ❌

Combinations :
localStorage + Context - sharing and persisting user selected language. Without Context, we have to read localStorage in each component Else lift state up and pass like props.


# Firebase Auth : 
stores the ID token + refresh token on the client side (browser) Firebase does not Enforce idle timeout.
When you log in, Firebase gives you:
🔐 1. ID Token (short-lived)
Expires in ~ 1 hour
🔄 2. Refresh Token (long-lived)
Used automatically to get a new ID token
👉 Firebase silently refreshes your session in the background.


# React Query 
server-state manager - “It manages data that comes from APIs”
Biggest feature - Caching. The data lives inside the app’s JavaScript memory (RAM). Resets on refresh.
Disadvantage - cached data lost on refresh.

 EX: useQuery({
      queryKey: ["movies", "nowPlaying"],
      queryFn: fetchMovies
    });
 
 
   conceptually : 
    queryClient = {
      queries: {
        ["movies", "nowPlaying"]: {
          data: [...movies],
          status: "success",
          lastFetched: 123456789
        }
      }
    }


