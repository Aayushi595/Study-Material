# Framework vs Library
Library - Collection of reusable functions or components . Eg. React
Framework - Complete structure for building an application . Eg. React Native

Latest react version : 19.2 , released in 2025, Oct.
Major changes : performance + developer experience upgrade.
1. <Activity /> (biggest feature) - hide UI without unmounting.
Previous - {isVisible && <Page />}
Current - .

# npm
npm - manages packages , provide a large repo containing packages for multiple purpose and functionality
how to make our app use npm : npm init
npm start --- npm run start (because start is a word reserved by npm)


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