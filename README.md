# Flyersremorse
A combination of the Fare Range API and InstaFlights Search API is used to show the user the lowest price on airfare and the range of prices that have recently been paid for that flight. That way the user can ensure they don’t have flyer’s remorse if they purchase a ticket below a certain price. 

Flyersremorse is a standard Single Page Application (SPA) created in AngularJS framework. It is a simple site with home/search view and search results view and as it is a single page app there is no page refresh on page change. The directory structure is divided to ‘views’ with all html files, ‘scripts’ which stores all angular code, ‘styles’ with the stylesheets created in Sass language. 

There are several dependencies app relies on apart from AngularJs like: moment.js, bootstrap, bootstrap datepicker and AmCharts for creating the chart. 
Flyersremorse is designed using fully automated developers workflow based in Grunt.js. There are multiple tasks created for code minification, uglifying, compilation, unit testing, sass compilation and finally building the standalone app. 

### 1. Working with app
In order to build source code  make sure that Node.js, Bower, Grunt, Grunt-CLI are installed on your development environment. Installation of build tools on platform what you are working on, please visit Node.js  (http://nodejs.org) website to find details concerning Node.js installation.
1.	Unpack source files and enter the unpacked directory.
2.	Execute following commands:

* npm install
* bower install
* grunt serve (in order to run the app on localhost in default browser)
* grunt serve:dist (in order to build app and run on localhost in default browser)

In case of any build issues please add –force parameter . 

### 2. Setting up FlyersRemorse application on a server. 
1.	Create a virtual host on any webserver.
2.	Transfer dist files to that directory.
3.	Edit scripts files and replace string  bridge.sabre.cometari.com to address of your Middleware. 

### 3. The Middleware requirements.
It might run on webserver with rewrite module (such as Nginx, Apache, Microsoft IIS) running with PHP module. It was successfully tested on Microsoft IIS 7.5 and with URL Rewrite with PHP 5.4.24. 
Url rewrite might downloaded from http://www.iis.net/downloads/ microsoft/url-rewrite and installed as an extension. PHP might be installed through the Web Platform Installer (Web Pi) http:// www.microsoft.com/web/downloads/platform.aspx 

### 4. The Middleware installation. 
1.	Make sure that requirements mentioned in the first paragraph have been met.  
2.	Run IIS Manager by typing inetmgr  
3.	Add a new website as it is presented on that screenshot  
4.	Unpack (middleware.zip) to the directory which the previously created Site is pointing to.  
5.	Edit web.config and following entry in <system.webServer> section  

```sh
<rewrite> 
             <rules>
                 <clear />
                 <rule name="Rule rewrite 1" enabled="false"stopProcessing="true">
                     <match url="^([^\.]+)$" />
                     <conditions logicalGrouping="MatchAll" trackAllCaptures="false">
                         <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                     </conditions>
                     <action type="Rewrite" url="{R:1}.php" />
                 </rule>
                 <rule name="Rule rewrite 2" stopProcessing="true">
                     <match url="^(.*)$" ignoreCase="false" />
                     <conditions logicalGrouping="MatchAll" trackAllCaptures="false">
                         <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                         <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
                         <add input="^/index\.php" matchType="IsFile" ignoreCase="false" negate="true" />
                     </conditions>
                     <action type="Rewrite" url="index.php" appendQueryString="true" />
                 </rule>
             </rules>
</rewrite> 
```
6.	Make sure that config file db/test_main.php contains the correct login credentials – more details described in paragraph 7.
7.	Reset IIS by typing iisreset.

### 5.	Updating login credentails to Sabre API.

Edit file db/main.db and replace login and password to valid login credentails.:
```sh
{"token":"Shared\/IDL:IceSess\\\/SessMgr:1\\.0.IDL\/Common\/!ICESMS\\\/RESH!ICESMSLB\\\/RES.LB!-3552884299647084251!398031!0!1!E2E-1","apiUrl":"https:\/\/api.dev.sabre.com\/v1","clientId":"V1:_YOUR_LOGIN_","clientSecret":"_YOUR_SECRET_PASS_"}
```
## Disclaimer of Warranty and Limitation of Liability

This software and any compiled programs created using this software are furnished “as is” without warranty of any kind, including but not limited to the implied warranties of merchantability and fitness for a particular purpose. No oral or written information or advice given by Sabre, its agents or employees shall create a warranty or in any way increase the scope of this warranty, and you may not rely on any such information or advice.
Sabre does not warrant, guarantee, or make any representations regarding the use, or the results of the use, of this software, compiled programs created using this software, or written materials in terms of correctness, accuracy, reliability, currentness, or otherwise. The entire risk as to the results and performance of this software and any compiled applications created using this software is assumed by you. Neither Sabre nor anyone else who has been involved in the creation, production or delivery of this software shall be liable for any direct, indirect, consequential, or incidental damages (including damages for loss of business profits, business interruption, loss of business information, and the like) arising out of the use of or inability to use such product even if Sabre has been advised of the possibility of such damages.
