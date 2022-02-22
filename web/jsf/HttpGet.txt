1. Let's go to the next example, which is bookmarks. In the project panel, Expend +Java EE Tutorial -> +Modules -> +web -> +jsf, right click <bookmarks> project,  choose <Open Project>.
2. Open menu File -> New Project... Select Java with Maven and Web Application. Click <Next>. Make Project name: bookmarks_c, and Project Location: C:\Programming\javaee8\myexamples\web\jsf, click <Next>. Make sure Server: is GlassFish Server 5.1.0 and JavaEE Version: is Java EE 8 Web. Click <Finish>
3. If the project name is not bookmarks_c, right click on it, and choose Rename... Make the Display name as bookmarks_c. Click <OK>
4. If you find beans.xml under Web Pages\WEB-INF, delete it. If you find anything under Source Packages, delete them all. Delete anything under Other Sources/src/main/resources.
5. Right click on project bookmarks_c, and choose Properties.
5.1 Click Sources in the left panel, confirm 1.8 is Source/Binary format in the right panel.
5.2 Click Frameworks in the left panel, click the <Add...> button in the right panel. Choose <JavaServer Faces> from the list.
5.3 Click Compile in the left panel, choose <JDK 1.8> as Java Platform:.
5.4 Click Run in the left panel, confirm <GlassFish 5.1.0> is the Server, and <Java EE 8 Web> is the Java EE Version. Click the <OK> button to close the properties window.
6. Double click web.xml under WebPages -> WEB-INF, change /faces/* to *.xhtml, and faces/index.xhtml to index.xhtml. This will make the request call url simpler. Also change 30 minutes to 300 for the session timeout, so the page will stay there long enough for us to talk about it in detail.
9. Copy bookmarks project to bookmarks_c.
9.1 Delete all pages from WebPages of bookmarks_c project.
9.2 Copy all pages including the resources folder from Web Pages of bookmarks project to bookmarks_c.
9.3 Copy all the Java source code from Source Packages of bookmarks project to bookmarks_c.

10. Open index.xhtml, change h:graphicImage url to "resources/images/duke.waving.gif". Delete id="username", id="submit", id="reset" and  class="messagecolor". They are useless.
11. Open  response.xhtml, change h:graphicImage url to "resources/images/duke.waving.gif". Delete id="back". It is useless.
12. Open personal.xhtml, change h:graphicImage url to "resources/images/duke.waving.gif".
13. <Save>, <Clean and Build> and <Run>. 
14. Enter Fei as the name, <Submit>, click <personal greeting page!> link. 
15. <Ctrl>+c key to copy the url of the browser. Open a new tab of the browser, <Ctrl>+v key to paste the url, <Enter> key. You see another personal greeting page, which means you can bookmark the personal greeting page. This is all the purpose of this project.
16. I am already explain this topic in previous example, I repeat it here. HTTP request has 2 types, get and post. It typically code like: <form mothod="get or post", action="next page">, if it is a get call, the browser not only show the next page, but also update page url. The url includs next page name and all parameters, like this:  http://localhost:8080/bookmarks_c/personal.xhtml?Result=Fei+Li. You can bookmark it for your later use. If it is a post call, the browser still shows up the next page, but not update the url, the url become useless and misleading. So it is not bookmarkable. The call page and parameters is in the request header and body, you cannot see it. 
17. JSF default call type is post. when your project comes to a point that you want to offer the user a bookmarkable url, you need to use h:link, see response.xhtml. 
18. You can take h:link out of <h:form> if you want. It works both inside or outside of a form. Delete includeViewParams="true" because it is useless here, later on I will show you how to use it.
19. You can construct the url by your own without f:param, like this.
20. If you have special html letters in your value of a param, you have to escape them, like this.
21. Is there another way to make a bookmarkable url in JSF, yes, for example, h:commandButton action="nectpage?faces-redirect=true". faces-redirect=true tells the browser to use HTTP GET call.
22. Open personal.xhtml, f:viewParam will find a page param Result, and put its value to hello.name.
add a new h:link like this. Now includeViewParams="true" works. The link will not only have its own param Reason, but also includes the page param, in this case Result.

