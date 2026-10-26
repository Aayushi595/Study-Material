# React/React.js
React is a JavaScript library for building user interfaces using components. React is a library and not framework because React only focuses on building UI components, not the entire application structure.

# JSX 
JSX - Javascript Xml - JS syntax sugar which makes easier to create react elements instead of the older way of writing react elements.
And we can write dom elements and js simply in .js file.
We can say JSX an invalid js because the js Engine or browser don't understand it.
JSX allows you to write something that looks like HTML inside JavaScript/TypeScript.

=> class is a JavaScript reserved word/keyword context, so JSX uses className to represent the HTML class attribute.
className can also receive a JavaScript expression.

=> when you put an array directly inside JSX, Array.prototype.toString() on it — which joins elements with commas by default (["North Indian", "Chinese"] → "North Indian,Chinese").

=> console.log('1234') returns undefined (technically, void).
=> React DOM skips rendering anything for undefined, null, or false values in JSX, in browser. (ReactJS)

=> Javascript expression inside jsx is written inside {} ,  Ex : onClick={() => setEntryState('login')}

=> React Native is strict about what can be rendered inside <Text>.It expects a valid string, number, or another <Text> — not undefined.
Note we can't even call a function in ReactNative JSX, that returns void.

Note : JSX doesn't inherently require React. It needs a JSX compiler/transpiler. Thus it is not React part.

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

# Identification as React Component
1.  <MyComponent /> - this syntax
    Note : React components only need to return a React element, or null, or string, or number, or array, or portal, or even nothing.
2.  wrappings (navigator etc.)
3.  React will treat <login /> as a custom HTML tag, not a component. - capital initial letter for react component

# Non-screen components 
import { useNavigation } from '@react-navigation/native';
const MyComponent = () => {
const navigation = useNavigation();
return <Button title="Go" onPress={() => navigation.navigate('Home')} />;

# React Routes 
import { createRoot } from 'react-dom/client'
import { createBrowserRouter, RouterProvider } from "react-router-dom";
createBrowserRouter([...]) returns a Router object.

# React Rendering.



# Ways to get react into our app :
1. cdn links
2. npm install react - we are not gonna install react as dev dependency.
3. npm install react-dom
4. import ReactDOM from "react-dom"

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


# Hooks. Normal js utility functions
state Hooks : useState - keeps the state of component. (scope of local state variable -component)
- const result = useState({email : "" , password : " "})    // [{ email: '', password: '' },   function setState() {}] -  array of two elements.
useEffect : Is called after every re-render- if dependency array, executes its callback after the first render (UI) and change in deps. Best for API calls. So if I had to do something after rendering the UI, we use UseEffect.
approch 1 (inefficient) : page load -> api call -> render UI
approch2 : (efficient) : page load -> render ui -> api call -> re-render.

useState hook : How a const variable is getting updated in local state variable.?
The same variable is not getting updated, when we do setState(), the component re-renders and this time, the local variable is a new instance/reference, and its default value is the value which we set.
   
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


# Closure Applications
  - When we pass in function reference from parent to child for any event in child. On the event call, executes the parent’s function.  Beacause of Closure the  function still has access to the parent’s state and variables at the time it was created.


# Challenges
   - Issue in state set and incorrect UI update  -  React batches state updates and causes re-render after function execution completes + related functions using stale closures (older reference or value)

# State update within batch
second update waits for the first to complete entirely - they run synchronously within the batch execution.
State updates are enqueued in React’s internal Fiber update queue.

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


