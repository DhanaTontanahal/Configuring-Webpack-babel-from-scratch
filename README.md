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




