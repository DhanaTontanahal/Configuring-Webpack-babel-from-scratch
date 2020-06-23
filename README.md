# Configuring-Webpack-babel-from-scratch
Configuring Webpack babel from scratch


Webpack 4 , Babel 7 is a good combination.

J FYI
Babel 7 drops the support for unmaintained node.js versions (0.10, 0.12 , 4.5)
https://github.com/babel/babel/tree/main/packages
We have all the babel packages under 1 name space babel , so we use @babel namespace.
yearly presets like @babel/preset-es2015 etc are not required and we can use @babel/preset-env
stage presets are also removed like @babel-stage-0

We start by removing the "react-scripts" dependancy in package.json which has the webpack and babel configuration for CRA.

We need to install dependancies first.
Webpack --> bundler
Babel ---->Transpiler

We have import statements which are not supported by the browsers which needs to be transpiled to ES5.
We have class property functions or ES6 functions which needs to be transpiled to be understood by all the browsers.

Webpack:

Add webpack as DEV dependancy webpack webpack-cli 
webpack-dev-server (local server with hot reload)

html-webpack-plugin@next (creating html file at runtime) to serve webpack bundles (https://webpack.js.org/plugins/html-webpack-plugin/)
babel-loader @babel/core (https://babeljs.io/setup#installation)
https://webpack.js.org/loaders/babel-loader/
@babel/plugin-proposal-class-properties (search for class properties in babel packages website https://github.com/babel/babel/tree/main/packages
@babel/preset-react(for jsx transpilation)

npm install --save-dev webpack webpack-cli webpack-dev-server html-webpack-plugin@next babel-loader @babel/core @babel/preset-env 
@babel/preset-react @babel/plugin-proposal-class-properties

Now create webpack.config.js in the root location

module.exports={
entry//by default it goes to src/index.js so no need to mention
build//by default it goes to dist/main.js folder
devtool (check here and select as per your requirement) https://webpack.js.org/configuration/devtool/ -->cheap-module-source-map(not that slow, fit for prod , can see source
next is module---> module:[] --->from https://babeljs.io/setup#installation
module: {
  rules: [
    { test: /\.js$/, exclude: /node_modules/, loader: 'babel-loader' } /// line 1.)
  ]
}

the above line is not going to appply any presets or plugins
we neeed to explicitly do this with babelrc file, there are many ways including the passing options:{} in the line 1.) but we are going to use babelrc file
https://babeljs.io/docs/en/configuration



}

.babelrc

{
 "presets":[
     "@babel/env","@babel/react"   
    ],
    "plugins": [
        "@babel/proposal-class-properties",
        @babel/plugin-proposal-decorators
    ]
}



Now go to package.json and move the scripts to top
and customize the scripts

now add the htmlwebpack plugin to webpack config file 

https://webpack.js.org/plugins/html-webpack-plugin/

var HtmlWebpackPlugin = require('html-webpack-plugin');

plugins: [new HtmlWebpackPlugin()]
this plugin creates a index.html file at runtime
this takes the sample template as one of the arguments
add a template (check https://github.com/jantimon/html-webpack-plugin)
plugins: [new HtmlWebpackPlugin({sampletemplate
})]

We can move the index.html to the root location and delete the public folder.
This index.html file is just acting as a template and webpack creates a final build whch includes everything ready to deploy.

Now add options to the webpack deve server

you can add either in webpack config file or directly from package.json

Final config files.
webpack

var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports={
    devtool:'cheap-module-source-map',
    module: {
        rules: [
          { test: /\.js$/, exclude: /node_modules/, loader: "babel-loader" }
        ]
      },
      plugins: [new HtmlWebpackPlugin({
          template:'index.html'
      })]
}

package.json

{
  "name": "react-webpack4",
  "version": "1.0.0",
  "license": "MIT",
  "main": "src/index.js",
  "scripts": {
    "start": "webpack-dev-server --open --port 4000 --compress --mode development",
    "dev":"webpack --mode development --progress",
    "build": "webpack --mode production -progress"
  },
  "dependencies": {
    "@material-ui/core": "^3.0.1",
    "@material-ui/icons": "^3.0.1",
    "react": "^16.4.2",
    "react-dom": "^16.4.2"
  },
  "devDependencies": {
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "@babel/preset-react": "^7.10.1",
    "babel-loader": "^8.1.0",
    "html-webpack-plugin": "^4.0.0-beta.14",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.0"
  }
}


.babelrc
{
    "presets":[
     "@babel/env","@babel/react"   
    ],
    "plugins": [
        "@babel/proposal-class-properties"
    ]
}



Credit:
https://www.youtube.com/watch?v=A4swyDR45SY
https://codesandbox.io/s/qq4oz0ym69

Thanks


Now start the the webpacck webserver

npm runstart


Application openss up at the port 4000

Lets moveon for adding support for old browsers with the help of polyfill babel package.

install babel polyfill

npm install @babel/polyfill
Add the package to entry file as import '@babel/polyfill
import it at the top in index.js


Add useBuiltIns:'entry' in the presets in babelrc file

Now add the targets , i.e the build for the  target browsers to focus on.
Check jhere for more details.
https://jamie.build/last-2-versions

https://browserl.ist/



"browsers": [
  ">0.25%",
  "not ie 11",
  "not op_mini all"
]



final babelrc


{
    "presets": [
        [
            "@babel/env",
            {
                "useBuiltIns": "entry",
                "targets": {
                    "browsers": [
                        ">0.25%",
                        "not ie 11",
                        "not op_mini all"
                    ]
                }
            }
        ],
        "@babel/react"
    ],
    "plugins": [
        "@babel/proposal-class-properties"
    ]
}

CreditsL:

https://codesandbox.io/s/qq4oz0ym69
https://www.youtube.com/watch?v=A4swyDR45SY
