# Example of Web Application using Bootstrap Vue JS

This is an example of web application that has been developed with Bootstrap plus VueJs framework. This app makes data aggregation from a json local file. Also it allows the user to input parameters to perform calculations on the data aggregation. This has been developed in Ubuntu 18.04. Nkt tested neither Windows nor Mac. 

## Running the App

To run the app download the source code doing a git clone as follows:

$ git clone https://github.com/rcolomina/app-bv-data-aggreg

Navigate onto the downloaded folder:

$ cd app-bv-data-aggreg

Install the npm package manager to run the app:

$ sudo apt-get install npm

Type in the following command to run the server on the terminal window:

$ npm run serve

The last command should show the below text on the terminal window of the server. At this moment the app is up and running available at the localhost address and port 8080.

DONE  Compiled successfully in 5076ms                                                                                                                                                                     14:06:14

  App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.0.24:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

Now you can open a web browser and typing http://localhost:8080


## WARNING 

Node Modules have been included into this repository due to an issue with css (error EISDIR: illegal operation on a directory) could be resolved. Nevertheless node modles can be installled as well but these last versions defined i package.json,  will not be able to compile css bootstrap files.  If you decided to install node modles from npm you should remove the current folder node_modules:

$ rm -rf node_modules

Now, insstall then again but using npm and the current package.json as list of dependencies:

$ npm install package.json

Then you can follow the above instructions to run the server. Unfortunatelly, this will fail with a compilation error over the bootstrap css files, so you will have to disable the imports on bootstrap css declared within src/App.vue in order to build it.
 
