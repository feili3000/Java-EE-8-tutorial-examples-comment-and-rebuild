1. Let's go to the next example, which is ajaxguessnumber. In the project panel, Expend +Java EE Tutorial -> +Modules -> +web -> +jsf, right click <ajaxguessnumber > project,  choose <Open Project>.
2. Open menu File -> New Project... Select Java with Maven and Web Application. Click <Next>. Make Project name:  ajaxguessnumber_c, and Project Location: C:\Programming\javaee8\myexamples\web\jsf, click <Next>. Make sure Server: is GlassFish Server 5.1.0 and JavaEE Version: is Java EE 8 Web. Click <Finish>
3. If the project name is not  ajaxguessnumber_c, right click on it, and choose Rename... Make the Display name as  ajaxguessnumber_c. Click <OK>
4. If you find beans.xml under Web Pages\WEB-INF, delete it. If you find anything under Source Packages, delete them all. Delete anything under Other Sources/src/main/resources.
5. Right click on project  ajaxguessnumber_c, and choose Properties.
5.1 Click Sources in the left panel, confirm 1.8 is Source/Binary format in the right panel.
5.2 Click Frameworks in the left panel, click the <Add...> button in the right panel. Choose <JavaServer Faces> from the list.
5.3 Click Compile in the left panel, choose <JDK 1.8> as Java Platform:.
5.4 Click Run in the left panel, confirm <GlassFish 5.1.0> is the Server, and <Java EE 8 Web> is the Java EE Version. Click the <OK> button to close the properties window.
6. Double click web.xml under WebPages -> WEB-INF, change /faces/* to *.xhtml, and faces/index.xhtml to index.xhtml. This will make the request call url simpler. Also change 30 minutes to 300 for the session timeout, so the page will stay there long enough for us to talk about it in detail.
9. Copy ajaxguessnumber project to ajaxguessnumber_c.
Delete index.xhtml from ajaxguessnumber_c, copy resources folder and ajaxgreeting.xhtml over from ajaxguessnumber to ajaxguessnumber_c, under Web Pages. copy Java code package  from ajaxguessnumber to ajaxguessnumber_c, under Source Packages.
10. Open web.xml, change the welcome file from index.xhtml to ajaxgreeting.xhtml
11. <Save>, <Clean and Build> and <Run>. Play with it.
12. Open ajaxgreeting.xhtml, do some changes:
12.1 change stylesheet link as: <h:outputStylesheet name="css/default.css"/>. 
12.2 Change the image link as: <h:graphicImage value="resources/images/wave.med.gif"... 
12.6 Delete  id="AjaxGuess", id="errors1" and id="submit".
12.7 Menu Source -> Format, <Save>.
13. 3 new things on this page.
13.1. h:commandButton has no action, therefore, for executing <f:ajax> only. <f:ajax> submit input text to userNumberBean.userNumber.
13.2 h:outputText uses rendered field to determine render or not.
13.3 h:message is in the f:ajax render field. Can we take it out to put underneath h:inputText? No, because this is a f:ajax call only and the page will not be updated fully, so the error message will never be shown up. If you take it out to put underneath h:inputText, you should render it in f:ajax call like this: render="outputGroup, errors1".
14. UserNumberBean is a request scope bean. After the ajaxgreeting.xhtml delivered, the server deleted the bean. Afterwards, each <f:ajax> call will submit the input value to userNumberBean.userNumber, therefore, create a new request bean, after f:ajax render="outputGroup" finish, f:ajax request finished and the bean is deleted. So N times guesses will cause N times bean creation/destruction. Not good, change it to a view scop. See my comment about this issue in:
Java EE 8 Tutorial Examples detailed explanation 4 JSF html5
15. <Save>, <Clean and Build> and <Run>
