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