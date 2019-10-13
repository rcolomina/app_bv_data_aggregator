# Example of Web Application using Bootstrap Vue JS

This is an example of web application developed with Bootstrap plus VueJs framework. This app does local data aggregation from a json local file. Also it allows to the user input some parameters to make calculations on data aggregations. This has been developed in Ubuntu 18.04, however it should work on Windows or Mac as well. 

## Running the App

To run the app this may download the source code using git clone as follows:

$ git clone https://github.com/rcolomina/app-bv-data-aggreg

Navigate to the downloaded folder:

$ cd app-bv-data-aggreg

Install npm package manager to run the app:

$ sudo apt-get install npm

Type in the following command to run the server:

$ npm run serve

The command should show the following text on the terminal window. Now this app its available on the localhost address at 8080 port.


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
 
