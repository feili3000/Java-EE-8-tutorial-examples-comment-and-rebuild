1. Let's go to the next example, which is hello1-rlc. In the project panel, Expend +Java EE Tutorial -> +Modules -> +web -> +jsf, right click < hello1-rlc> project, �choose <Open Project>.
2. Open menu File -> New Project... Select Java with Maven and Web Application. Click <Next>. Make Project name: �hello1-rlc_c, and Project Location: C:\Programming\javaee8\myexamples\web\jsf, click <Next>. Make sure Server: is GlassFish Server 5.1.0 and JavaEE Version: is Java EE 8 Web. Click <Finish>
3. If the project name is not hello1-rlc_c, right click on it, and choose Rename... Make the Display name as hello1-rlc_c. Click <OK>
4. If you find beans.xml under Web Pages\WEB-INF, delete it. If you find anything under Source Packages, delete them all. Delete anything under Other Sources/src/main/resources.
5. Right click on project hello1-rlc_c, and choose Properties.
5.1 Click Sources in the left panel, confirm 1.8 is Source/Binary format in the right panel.
5.2 Click Frameworks in the left panel, click the <Add...> button in the right panel. Choose <JavaServer Faces> from the list.
5.3 Click Compile in the left panel, choose <JDK 1.8> as Java Platform:.
5.4 Click Run in the left panel, confirm <GlassFish 5.1.0> is the Server, and <Java EE 8 Web> is the Java EE Version. Click the <OK> button to close the properties window.
6. Double click web.xml under WebPages -> WEB-INF, change /faces/* to *.xhtml, and faces/index.xhtml to index.xhtml. This will make the request call url simpler. Also change 30 minutes to 300 for the session timeout, so the page will stay there long enough for us to talk about it in detail.
9. Copy hello1-rlc project to �hello1-rlc_c.
9.1 delete all web pages from hello1-rlc_c, copy contracts, reply folders and greeting.xhtml from hello1-rlc Web Pages to hello1-rlc_c �Web Pages. copy javaeetutorial.hello1rlc folder from Source Packages of �hello1-rlc to �hello1-rlc_c.
9.2 Open web.xml again and change the welcome page to greeting.xhtml.
9.3 Right click project hello1-rlc_c, �Go to <new> -> <Other...>, Select <JavaServer Faces> and <JSF Faces Configuration>. <Next> and <Finish>.
9.4 copy whole section <application>... <application> from faces-config.xml of hello1-rlc to faces-config.xml of hello1-rlc_c.
9.5 Do <Save>, <Clean and Build> and <Run> for project � hello1-rlc_c. You will see a web page showing up. If you get the error: Failed to clean project, just go to Services tab to stop GlassFish server.
9. This application has 2 pages: greeting.xhtml and response.xhtml, and 2 sets of resources: contracts/hello and contracts/reply. The greeting page uses hello resource set and response uses reply resource set. Let's run it, please pay attention to the different color and image of the 2 pages.
10. How do they deal with multi-resource sets? We are told to organize them to different folders, but the folders must be under a root folder called contracts. The name and location of contracts' folder is implied and is hard code. Remember how they regulate one set resource? They say you must put everything under a root folder called resources. The name and location of �resources' folder �is also implied and is hard code.
11. How does a page use a resources' set? Through a url pattern. Open the file: Web Pages\WEB-INF\faces-config.xml. You can see the resources set configuration. If you call a page with a url pattern /reply/*, the page will use the resources "repla", all other call patterns will use resources "hello". By the way, you can create a JSF configuration file faces-config.xml through right click <New> -> <other...> -> <JavaServer Faces>&<JSF Faces Configuration>.
12. This resources organization design is a failed design to me because of the following reasons: 1. Single resources' set and multi-resources' sets have no common structure. The former has the root resources and the latter has the root contracts. 2. The resources and the contracts
are both implied and hard coded. 3. The url pattern is an important contract between the user and the application. It should be used for organizing application functions, not for organizing internal resources' sets.
13. For the above reasons, I want to rewrite this application. Right click <Web Pages> -> <New> -> <Other...>, Select <Other> and <Folder>, then <Next>, Folder Name: is resources. <Finish>. Under the resources folder, create hello and reply folders. Under the both folders, create 3 folders: css, images and templates.
15. copy contracts/hello/*.css to resources\hello\css, �copy contracts/hello/*.gif to resources\hello\images, and copy contracts/hello/template.xhtml to resources\hello\templates.
16. copy contracts/reply/*.css to resources\reply\css, �copy contracts/reply/*.gif to resources\reply\images, and copy contracts/reply/template.xhtml to resources\reply\templates. 
17. delete contracts folder and faces-config.xml.
18. Open greeting.xhtml. make the following change: template="/resources/hello/templates/template.xhtml"
19. Open resources/hello/templates/template.xhtml, make the following changes: h:outputStylesheet name="hello/css/default.css"
h:graphicImage url="/resources/hello/images/duke.handsOnHips.gif"
20. Open response.xhtml. make the following change: template="/resources/reply/templates/template.xhtml"
21. Open resources/reply/templates/template.xhtml, make the following changes: <h:outputStylesheet name="reply/css/default.css"/> and h:graphicImage url="/resources/reply/images/duke.thumbsup.gif".
22. <Clean and Build> and <Run>, <Submit>, <Back>.
23. final clean. Open Hello.java from Source Packages\javaeetutorial.hello1rlc. @Model is a shortcut of @Named
�@RequestScoped.
24. Open greeting.xhtml, delete all 3 id="xxx", delete class="messagecolor", no use to them.
25. Open resources/hello/templates/template.xhtml, delete all 4 id="xxx". no use to them.
26. Open response.xhtml, delete id="back", it is no use.
27. Open resources/reply/templates/template.xhtml, delete all 3 id="xxx". no use to them.
28. <Clean and Build> and <Run>, <Submit>, <Back>.