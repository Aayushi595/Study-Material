Steps to setup : 
1. create react app. npx create-react-app netflix-gpt 
(basic structure like scripts, packages, bundler, testing etc are created)

# What CRA uses internally
1. Bundler: Webpack
2. Dev Server: webpack-dev-server
3. Transpiler: Babel
4. Linting: ESLint
5. Testing: Jest

webpack-dev-server → enables HMR
Webpack HMR plugin → replaces updated modules
React Fast Refresh → preserves component state during updates

2. npm start - starting the developement server

3. Setup Tailwind
See Tailwind setup steps for cra.

4. Setup Routing
npm i -D react-router-dom
import Router provider from React Router DOM
Put app router in Body

