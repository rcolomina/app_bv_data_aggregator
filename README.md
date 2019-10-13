# Example of Web Application using Bootstrap Vue JS
This is an example of web application using Bootstrap plus VueJs. This app is showing static data aggregating them together and making some brief short calculations. This app was developed on Ubuntu 18.04, however this should work on Windows or Mac as well. 

## Running the App

First use git to clone this reposotiry onto your local computer from a terminal window:

$ git clone https://github.com/rcolomina/app-bv-data-aggreg

Enter within the cloned folder:

$ cd app-bv-data-aggreg

Install npm to run this app:

$ sudo apt-get install npm

Type in the following command@

$ npm run serve

On your terminal windows you should have the following output indicating that the web app is up and running:r


DONE  Compiled successfully in 5076ms                                                                                                                                                                     14:06:14


  App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.0.24:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

Now you can open a web browser and typing http://localhost:8080


## WARNING 

Node Modules have been included into the repository due to an issue with css (error EISDIR: illegal operation on a directory), despite of these can be installed. In the latter case, doing and installation of the node modules, the css will not be able to compile properly.   

If you decided to fix this problem you can install node modules doing, you can delete the current node_modules:

$ rm -rf node_modules

Install them again using npm and the current package.json:

$ npm install package.json

Then you can follow the above instructions. Unfortunatelly, this will fail with a compilation error on css, so you will have to disable the imports on bootstrap css declared within src/App.vue in order to build it.
 
