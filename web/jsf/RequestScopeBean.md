Most people learn Java EE starting from looking at these examples. They are very important. I want to make my comments on those examples. Do some modifications, fix some bugs, and rebuild them in my way.
1. download Java JDK 17.0.2 for windows from:
https://www.oracle.com/java/technologies/downloads/
install

2. download Java JDK 8u311 for windows from:
https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html
install

3. download Java EE 8 SDK from:
https://www.oracle.com/java/technologies/java-ee-sdk-download.html
3.1 unzip it, copy glassfish5 to c:\Programming.
3.2 make a new folder c:\Programming\javaee8
3.3 copy examples folder from: C:\Programming\glassfish5\docs\javaee-tutorial to c:\Programming\javaee8
3.4 make a new folder c:\Programming\javaee8\myexaples

3.5 download GlassFish Server 5.1.0 from:
https://www.eclipse.org/downloads/download.php?file=/glassfish/glassfish-5.1.0.zip
3.6 In Windows file explorer, right click the file, choose <Extract All...>
3.7 Rename glassfish5 to glassfish-5.1.0 and copy it to C:\Programming

4. download apache netbeans 12.6 from:
https://netbeans.apache.org/
install, must choose JDK 17.0.2

5. setting up ide environment
5.1 open netbeans
5.2 open menu Tools -> Options, select General tab, Choose your favorite web browser for web Browsers: click <Apply> button, wait a while for it settle down, click <OK> button.
5.3 open menu Windows -> services, it will open a panel on the left and Services tab on the panel top. From now on, we call the panel Projects panel.
5.4 Click on the Services tab on the Projects panel. Right click on the Servers from the list. Choose <Add Server>. It pops up a window, changes the name to GlassFish Server 5.1.0, and chooses GlassFish Server. Click the <Browse...> button, make sure the file name: field is: C:\Programming\glassfish-5.1.0, and click on the <Choose> button. Click the <Next> button, then the <Finish> button. Now you can see GlassFish Server 5.1.0 on the Servers list.
5.5 Right click on GlassFish Server 5.1.0, and choose <Start>. It pops up a window asking you to choose another JDK other than JDK 17, GlassFish 5.1.0 runs on JDK 1.8, so you choose JDK 1.8 and click <OK>. Wait for a while till you see a green triangle show up besides GlassFish head. It means the server started successfully. Right click on <GlassFish Server 5.1.0> and choose <View Domain Admin Console>, the GlassFish 5.1.0 admin page page shows up in your browser. Remember the URL for the page is: http://localhost:4848. Go to http://localhost:8080, you will see the server welcome page.
5.5 test Java DB. GlassFish server will install Java DB for you automatically. Expan <+ Java DB>, right click on sample database and choose <connect...>.
5.6 GlassFish server 5.0 and Apache Netbeans 12.6 have a problem in the JSF part, so I choose GlassFish 5.1.0.
5.7 If you choose Chrome as the browser, install Netbeans Connector for Chrome from:
https://chrome.google.com/webstore/detail/netbeans-connector/hafdlehgocfcodbgjnpecfajgkeejnaa?hl=en
Click <Add to Chrome>, Click <Add extension>
Later on when we do HTML5 and Javascript examples, the connector will be very helpful when we do debugging.
6. open Java EE Tutorial Examples project.
6.1 open menu File --> Open Projects... Go to c:\Programming\javaee8, click on the project <examples>, click on the <Open Project> button.
7. hello1
7.1 In the project panel, under project <Java EE Tutorial Examples>, expand +Modules, then expand +web, then extend jsf, right click on <holle1>, then choose <Open Project>. You got Project <hello1> opened.
7.2 use the file manager to create a new folder C:\Programming\javaee8\myexamples\web\jsf
7.3 open menu <File> -> <Create...> Choose <Java with Maven> from the left list, and choose <Web Application> from the right list, and click on the <Next> button. Project Location: will be: C:\Programming\javaee8\myexamples\web\jsf, and project name: will be hello1_c. Click <Next> button, Server: should be <Glassfish 5.1.0> and Java EE version: should be <Java EE 8 Web>. Click <Finish>
7.4 If the project name is not hello1_c, right click on it, and choose Rename... Make the Display name as hello1_c. Click <OK>
7.5 If you find beans.xml under Web Pages\WEB-INF, delete it. If you find anything under Source Packages, delete them all. �Delete anything under <Other Sources>/src/main/resources.
7.5 right click on project hello1_c, and choose Properties.
7.6 Click Sources in the left panel, confirm 1.8 is Source/Binary format in the right panel.
7.7 Click Frameworks in the left panel, click the <Add...> button in the right panel. Choose <JavaServer Faces> from the list.
7.8 Click Compile in the left panel, choose <JDK 1.8> as Java Platform:.
7.9 Click Run in the left panel, confirm <GlassFish 5.1.0> is the Server, and <Java EE 8 Web> is the Java EE Version. Click the <OK> button to close the properties window.
7.9.1 Double click web.xml under WebPages -> WEB-INF, change /faces/* to *.xhtml, and faces/index.xhtml to index.xhtml. This will make the request call url simpler. Also change 30 minutes to 300 for the session timeout, so the page will stay there long enough for us to talk about it in detail.
8. Copy whole project from hello1 to hello1_c.
8.1 delete all pages under Web Pages from hello1_c, copy Web Pages hello1 project to hello1_c project, including resources folder, index.xhtml and response.xhtml. copy javaeetutorial.hello1 folder from Source Packages of hello1 to hello1_c.
8.2 Do <Clean and Build> and <Run> for project hello1_c. You will see a web page showing up. If you meet the error: Failed to clean project, just go to Services tab to stop GlassFish server.
8.3 double click on web.xml and look at <welcome-file>, it is index.xhtml. So we know the page showing up is index.xhtml.
8.4 Double click index.xhtml. The first thing I want to change is h:graphicImage url, Java EE has a design problem for how to use resources, so they come up with this terrible format, nobody can remember how to use it, so let's change it back to the old way. In the project, find the image location from <Web Pages>, so, change to url="resources/images/duke.waving.gif" �
8.5 delete 3 id="xxx", they are useless. id is only used by javascript and stylesheet. You can see in the project there is no javascript and stylesheet. delete class="messagecolor" from <div>, it targets a stylesheet but the project does not have any stylesheet.
8.5 Notice h:inputText has 2 fields: required="true", requiredMessage="Error: A name is required.", which means submit an empty input will cause an error, the error message is "Error: A name is required.", the message will be displayed on the h:messages location and has the style defined by the h:messages. In the web browser, leave the input box empty, click the <Submit> button to test it. �
8.6 maxlength="25" tells the browser not to create an input box longer than 25, it can be less than 25 if the page is too crowded.
8.7 value="#{hello.name}" what is this? double click Hello.java. You see @Named and @RequestScoped. We call it a managed bean. The managed bean has a name "hello", the same as class name except the lower case of the first letter. When we click <run> for the project <hello_c>, the GlassFish server actually will deploy the project first before running it. At the deploy procedure, the server will find all managed bean and register them in the server JNDI database. In this project, the server will register something like name="hello_c/hello", class="javaeetutorial.hello1.Hello, scope="request. 
The difference between Java ordinary instance and the managed bean is that the Java ordinary instance's life cycle is controlled by you and the managed bean's life cycle is controlled by the server. For example, in ordinary Java applications, we would like to write some code like this: methd(), when method gets called, hello instance created, when the method call finishes, the hello instance will be labeled as garbage collectable. Java garbage collector thread runs periodically. When it runs, it dumps all �garbage collectable instances out of the memory. �In a Java EE Web application like this hello1, we never new an instance, instead the server will do that for us, after we click <Run>, Netbeans will make a request to the server GlassFish 5.1.0 by putting a url "http://localhost:8080/hello1_c/" in the browser address bar on our behalf. From web.xml, the server knows the request is for page index.xhtml, for the moment, the index.xhtml is not a pure html format yet, so the server loads it to process. �When server meet the term #{hello.xxx}, the server knows it is about a managed bean, so it look at its JNDI database and find its class and scope, the server will new an instance from the class, if index.xhtml has more #{hello.xxx}, the server will use the same instance to deal with it. When the whole index.xhtml translates to pure html format, the server will send it back to the browser. Now the request is finished, because the hello instance's life cycle is "request", so the server will label it to garbage collectable and wait for Java Garbage Thread to come to pick it up. �
The value="#{hello.name}" in the �h:inputText has 2 purposes, when the server construct html format it calls hello.getName() to retrieve the value, if the value is not null or empty it will be put into the input box; when you enter something in the box, say "Fei Li", the server will call hello.setName("Fei Li") to store the value for you.
9. Let's put a name into the input box and click the <Submit> button. From the action="response" of the h:commandButton in the index.xhtml, we know this is a brand new request to the GlassFish server for the page response.xhtml. HTML has 2 kinds of requests: get or post. The get request will put all call stuff into a url, and post call will put target "response" in the html request header and page parameters into the html request body. You can see every detail from the url if the call is "get", but you can see nothing if the call is "post". Java EE Web default request format is post, so you can see nothing. When we do <Submit>, we are not only request response.xhtml page, we also send request parameter hello.name="Fei Li" along the request, so if we translate this post call to get call, the url will be: http://localhost:8080/hello1_c/response?hello.name="Fei Li".
10. After the server gets a new request, it loades response.xhtml to process the html response. Let's first modify the response.xhtml a little bit. Double click response.xhtml, change image url to "resources/images/duke.waving.gif", delete id="back". Click the save button in the toolbar under the menu bar. Because the response.xhtml has a tag #{hello.name}, it goes to its JNDI database to find the hello bean definition and creates a brand new managed bean instance with the name as "hello". This instance does not have anything with the previous one. The previous one is for a different request scope and dead already, this one is for the new undergoing request "response". �After the server creates a new hello instance, the server will go to a procedure called restore. In the restore procedure, the server will find all hello.xxx from the parameter list of the request call, and call hello.setxxx() to restore the bean values. In this case, because we have the parameter hello.name="Fei Li", the server calls hello.setName("Fei Li"). �After the restore procedure finishes, the server will retrieve its value from the bean and put it in the #{hello.name}'s location. �After the whole html was constructed, the server sent the response out, and destroyed the hello bean, again. Now we see what the response page looks like. The page has "Fei Li" displayed, and has a <Back> button with the action="index".
11. Both pages of index.xhtml and response.xhtml have a form. But, index.xhtml form has an input and it will create a parameter on the request; the response.xhtml has no input so the request will have an empty request parameters. 
12. We click the <Back> button and get back to the index.html page. Of course, the server will create a new hello bean instance again, because the request has no request parameters, so the restore procedure is skipped and hello.name is null. That is why we see an empty input text box here.
13. Do we have other requests that cause a new hello bean instance creation? Yes, we have, if we leave the input box empty and click the <Submit> button, it will create a new request scenario. If we translate the request post call to get call, it will look like: http://localhost:8080/hello1_c/response?hello.name="". When the GlassFish server receives such a call, it will go for a validation procedure before loading response.xhtml. If the validation fails (we do fail now) , according to the rule, the server will not load the requested page, instead it will load the original sending page, which is index.xhtml for our case. When it processes the index.xhtml, it has the error message to put into the page. And again it will create a new hello bean instance because it meets #{hello.name} again.
14. let's put some Java code to show when the server creates and destroys hello bean instances.
....
15. Click <Save>, do <Clean and Build>, then <Run>. You will see the index.xhtml page shows up in the browser. 
15.1 Click the <GlassFish 5.1.0>tab in the <output> window. You will see "hello bean 1 created.|#]" and "hello bean 2 created.|#]". We only expected one but the server did 2. I have no idea why the server did this, you tell me if you know. Anyway, we can get rid of this annoyance by clicking <Run> again, �see, this time, only hello bean 1 created.
15.2 If you leave the page alone for a while, you will see these: "hello bean 2 destroyed.|#]", "hello bean 1 destroyed.|#]". This is exactly what we are looking for, the index.xhtml request is done, the page sent out to the browser, and request scope hello bean destroyed.
15.3 Enter Fei Li into the input box and click the <Submit> button. The response.xhtml page shows up in the browser. The <GlassFish 5.1.0> tab shows "hello bean 3 created.|#]". wait for a while, you will see "hello bean 3 destroyed.|#]", as good as we want, processing response.xhtml needs to create a new bean instance, and destroy it after the page is sent out to the browser.
15.4 Click the <Back> button, you will see the index.html page shows up again, and the <GlassFish 5.1.0> tab window shows "hello bean 4 created.|#]", a while later, you will see "hello bean 4 destroyed.|#]", Good job, GlassFish, what is your favorite food? Let's continue.
15.5 Leave the input box empty, and click the <Submit> button. Index.xhtml, instead response.xhtml shows up with the error message in it. <GlassFish 5.1.0>Tab window shows "hello bean 5 created.|#]", and "hello bean 5 destroyed.|#] a little while later. 
15.6 If you click very quickly, a bunch of bean creations come first, and later on a bunch of bean destroyed messages come up together. It gives you an idea how Java garbage collector works asynchronously with the objects creations.










�