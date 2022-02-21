1. Let's go to the next example, which is checkout-module. In the project panel, Expend +Java EE Tutorial -> +Modules -> +web -> +jsf, right click <checkout-module> project, �choose <Open Project>.
2. Open menu File -> New Project... Select Java with Maven and Web Application. Click <Next>. Make Project name: checkout-module_c, and Project Location: C:\Programming\javaee8\myexamples\web\jsf, click <Next>. Make sure Server: is GlassFish Server 5.1.0 and JavaEE Version: is Java EE 8 Web. Click <Finish>
3. If the project name is not checkout-module_c, right click on it, and choose Rename... Make the Display name as checkout-module_c. Click <OK>
4. If you find anything under Source Packages, delete them all. Delete anything under Other Sources/src/main/resources.
5. Right click on project checkout-module_c, and choose Properties.
5.1 Click Sources in the left panel, confirm 1.8 is Source/Binary format in the right panel.
5.2 Click Frameworks in the left panel, click the <Add...> button in the right panel. Choose <JavaServer Faces> from the list.
5.3 Click Compile in the left panel, choose <JDK 1.8> as Java Platform:.
5.4 Click Run in the left panel, confirm <GlassFish 5.1.0> is the Server, and <Java EE 8 Web> is the Java EE Version. Click the <OK> button to close the properties window.
9. Copy checkout-module project to checkout-module_c.
9.1 Delete all pages from WebPages of checkout-module_c project.
9.2 Copy all pages including the resources folder from Web Pages of �checkout-module project to checkout-module_c.
9.3 Copy all the Java source code from Source Packages of checkout-module project to checkout-module_c.
9.4 copy � � 
<context-param>
� � � � <param-name>javax.faces.CLIENT_WINDOW_MODE</param-name>
� � � � <param-value>url</param-value>
</context-param>
from web.xml under Web Pages/WEB-INF of checkout-module to checkout-module_c. Change url-pattern to *.xhtml, Change welcome-file to index.xhtml
10. This example is about Flow scope. We know request scope, it survives only �for one request. We know the session scope, it keeps running for the whole user session. People think request scope is too short and session scope is too long in many cases, they try to think some scope is in the between. The flow scope is one of them, and major one of them. The flow scope organizes pages into different folders. Inside of each folder, all pages share one flow scope bean, the different folder is a different flow scope. You most likely have other pages that do not belong to any flow scope, feel free to organize them in your way. In this example, we have 2 flow scopes: joinFlow and checkoutFlow and 2 pages: index.xhtml and exithome.xhtml. People used to provide 2 non flow scope pages, one for enter flows and another for exit flow scope, but this is not required. You can have as many non flow scope pages as you want, but 0 is not good practice. Once your application does not use any flow scope any more, you can put them on a non flow scope page, so the server can destroy the flow scope bean to reduce the server load. The flow scope bean size is big in general because they serve multiple pages.
11. Each flow scope is defined by a configuration, the configuration can be done by a xml file or a Java code. Expand +joinFlow, you see a file has the same name as the folder plus"-flow". This is its configuration file. Open it, you can see it defines one flow-return and one flow-call. The flow-return page is exithome.xhtml and flow-call is going to checkout flow. �Expend +checkoutFlow you will not find the configuration file, so somewhere in Java code there will be a tag @Produces @FlowDefinition, it tells the server this is your configuration for the flow with a id="checkoutFlow". The server will keep the returned Flow object, when you do return or call, the server will check this object to see how to do that for you. Open CheckoutFlow.java, it is there. The checkoutFlow defined one flow-start, one flow-return and one flow-call. 
12. I want to add some code to monitor the life cycle of flow scope beans. 
12.1 Open JoinFlowBean.java, add the line:
� � � � System.out.println("JoinFlowBean Start.###################################################################");
to the constructor.
add the method:
� � @Override
� � public void finalize() {
� � � � System.out.println("JoinFlowBean end.###################################################################");
� � }
to JoinFlowBean.java.
12.2 Open CheckoutFlowBean.java, add these 2 methods to it:
� � public CheckoutFlowBean() {
� � � � System.out.println("CheckoutFlowBean Start.###################################################################");
� � }
� � @Override
� � public void finalize() {
� � � � System.out.println("checkoutFlowBean end.###################################################################");
� � }
13. <Save>, <Clean and Build> and <Run>. 
14. If the project failed to run, sometimes because of other broken projects. In such a case, Click <Services> in the left Projects panel, expend +GlassFish Server 5.1.0, expend +Applications, select all projects, right click and choose <Undeploy>. Click the <GlassFish server 5.1.0> tab in the left lower panel <output>. �Stop the server by clicking the red square icon on the left edge of the window. Right click on the window area of <GlassFish server 5.1.0>, choose <clear>. <Run> again.
15. Right click on the content area of the GlassFish Server 5.1.0 tab of the Output tab window, choose <Clear>.
15.1 Let's run it and see how it works. Click the <Check out> button to enter the checkoutFlow. You can see the "CheckoutFlowBean Start" message in the GlassFish Server 5.1.0 window. Fill all * fields and click <Continue>, click <Exit Flow>, we exit to the index.xhtml page. Wait for 1-2 minutes, you will see the "CheckoutFlowBean end" message in the GlassFish Server 5.1.0 window.
15.2 Click the <Join> button to enter the jointFlow. You can see the "JoinFlowBean Start" message in the GlassFish Server 5.1.0 window. Click <Exit Flow>, it exits to the exithome.xhtml page. Wait for 1-2 minutes, and you will see the " JoinFlowBean end" message in the GlassFish Server 5.1.0 window. Pherfect! right? But the big trouble is coming.
16. This time we click <Join> to enter JoinFlow. Click <Submit> we get joinFlow page two. Now we click the <Check out> button to call checkoutFlow from joinFlow. We enter page one of the checkoutFlow. fill all * fields and <Continue>, we are on the checkoutFlow page two. Click <Exit Flow>, we got an error message, saying "unable to find a matching navigation case...", more surprise! we are put on the joinFlow page one. Remember, last time, we clicked the same button, no errors and got an index.xhtml page. Obviously this is a bug of the example, I am wondering why they leave this bug unfixed. It is not hard to find it.
17. What is wrong, we do not know. Let's dig. Because it complains of missing a navigation case, let's make one for it. Insert 
� � � � flowBuilder.navigationCase().fromViewId("/checkoutFlow/checkoutFlow2.xhtml")
� � � � � � � � � � � � � � � � � � .fromOutcome("returnFromCheckoutFlow")
� � � � � � � � � � � � � � � � � � .toViewId("/index.xhtml").redirect(); � � � �
this into CheckoutFlow.java. 
18. <Save>, stop server by clicking the red square icon on the left side of <GlassFish Server 5.1.0> window. <Clean and Rebuild> and <Run>, follow the last time sequence: �click <Join> to enter JoinFlow. Click <Submit> we get joinFlow page two. Click the <Check out> button to call checkoutFlow from joinFlow. We are on page one of the checkoutFlow. fill all * fields and <Continue>, we are on the checkoutFlow page two. Click <Exit Flow>, no error any more, and we exit to the index.xhtml page. Nice! The bug is fixed, right? No, if you pay attention to the message of GlassFish Server 5.1.0 window, you can see the checkoutFlowBean start and end correctly. The joinFlowBean starts but does not end forever. It is a system bug which causes joinFlowBean to stay in the server memory forever, it is the server memory leak.
19. I think the flow design has a problem. In the specification, they say that the caller Flow bean can keep alive after call another flow, but if you open joinFlow-flow.xml, you find that when joinFlow call checkoutFlow, it take the property values out of the joinFlowBean and pass them to checkoutFlowBean as the page request parameters. Why do this if �joinFlowBean is still alive and assicible? � 
20. Insert #{joinFlowBean.returnValue} into checkoutFlow.xhtml, just under h:body tag. �<Save>, stop server by clicking the red square icon on the left side of <GlassFish Server 5.1.0> window. <Clean and Rebuild> and <Run>. Click <Join>, <Submit>, and <Check out>, look at <glassFish Server 5.1.0> window messages, the server did not use the existing joinFlowBean, and create a brand new one, the �existing joinFlowBean is dead in the server memory.
21. Delete #{joinFlowBean.returnValue} from checkoutFlow.xhtml; delete 
� � � flowBuilder.navigationCase().fromViewId("/checkoutFlow/checkoutFlow2.xhtml")
� � � � � � � � � � � � � � � � � � .fromOutcome("returnFromCheckoutFlow")
� � � � � � � � � � � � � � � � � � .toViewId("/index.xhtml").redirect(); 
from CheckoutFlow.java. 
22. The bug forces us to exit back to joinFlow when exit from checkoutFlow if we come from joinFlow. In CheckoutFlowBean.java, add:
� � String returnValue = "/index";
add:
� � � � FacesContext context = FacesContext.getCurrentInstance();
� � � � FlowHandler handler = context.getApplication().getFlowHandler();
� � � � boolean alive = handler.isActive(context, "", "joinFlow");
� � � � System.out.println("joinFlow active: " + alive + "##################################################################");

� � � � if (alive) {
� � � � � � returnValue = "/joinFlow/joinFlow.xhtml";
� � � � }
� to the constructor.
Change getReturnValue method as:
� � � � return returnValue;
23. Right click on the code area, choose <Fix Imports>, Menu <Source> -> <Format>, <Save>, stop server by clicking the red square icon on the left side of <GlassFish Server 5.1.0> window. <Clean and Rebuild> and <Run>,
�Follow the last time sequence: �click <Join> to enter JoinFlow. Click <Submit> we get joinFlow page two. Click the <Check out> button to call checkoutFlow from joinFlow. We are on page one of the checkoutFlow. fill all * fields and <Continue>, we are on the checkoutFlow page two. Click <Exit Flow>, this time we exit to the joinFlow page one. <Exit Flow> again, now we exit to exithome.xhtml page. Look at the message of GlassFish Server 5.1.0 window, you can see the checkoutFlowBean and joinFlowBean both ended. Nice!
24. We can improve more. We can exit to the page of joinFlow from which checkoutFlow is called. 
24.1 Open joinFlow2.xhtml, change action="callcheckoutFlow" to:
action="#{joinFlowBean.callcheckoutFlow()}"
24.2 Open JoinFlowBean.java, add the following:
� � String currentPageId;

� � public String callcheckoutFlow() {
� � � � FacesContext context = FacesContext.getCurrentInstance();
� � � � currentPageId = context.getViewRoot().getViewId();
� � � � return "callcheckoutFlow";
� � }

� � public String getCurrentPageId() {
� � � � return currentPageId;
� � }
Right click menu <Fix Imports>.
It gets the current pageId when joinFlow calls checkoutFlow.Open joinFlow-flow.xml, change flow-call outbound-parameter value from:
param1 joinFlow value
to:
#{joinFlowBean.currentPageId}
so we pass the currentPageId as a calling parameter. Open CheckoutFlowBean.java, change the constructor �if (alive) �part, (as:

� � � � if (alive) {
� � � � � � Map<Object, Object> paramMap = handler.getCurrentFlowScope();
� � � � � � returnValue = (String)paramMap.get("param1Value");
� � � � }
Right click, <Fix Imports>, It retrieves the calling parameter page id. Open CheckoutFlow.java, the returnNode will retrieve that page id and return to there.
25. <Save>, stop server by clicking the red square icon on the left side of <GlassFish Server 5.1.0> window. <Clean and Rebuild> and <Run>,
�Follow the last time sequence, <Join> --> <Submit> --> <Check out> --> <Continue> --> <Exit Flow>, now you exit to page two of joinFlow.

26. We fixed the call from joinFlow to checkoutFlow. We need to do the same thing for the call from checkoutFlow to joinFlow, as the following:
26.1 Open checkoutFlow4.xhtml, change action="calljoin" to:
action="#{checkoutFlowBean.calljoinFlow()}"
26.2 Open CheckoutFlowBean.java, add the following:
� � String currentPageId;

� � public String calljoinFlow() {
� � � � FacesContext context = FacesContext.getCurrentInstance();
� � � � currentPageId = context.getViewRoot().getViewId();
� � � � return "calljoinFlow";
� � }

� � public String getCurrentPageId() {
� � � � return currentPageId;
� � }
Here we record the leaving page when checkoutFlow calls joinFlow.
26.3 Open CheckoutFlow.java, change flowCallNode("calljoin") to:
flowCallNode("calljoinFlow")
add one more parameter below "#{checkoutFlowBean.city}") :
� � � � � � � � .outboundParameter("param3FromCheckoutFlow",
� � � � � � � � "#{checkoutFlowBean.currentPageId}")
Here we pass the leaving page as the outbound parameter when checkoutFlow calls joinFlow.
26.4 Open joinFlow-flow.xml, add third inbound-parameter like:
� � � � <inbound-parameter>
� � � � � � <name>param3FromCheckoutFlow</name>
� � � � � � <value>#{flowScope.param3Value}</value>
� � � � </inbound-parameter>
Here we config joinFlow to retrieve the third parameter whching is the leaving page of checkoutFlow.
26.5 Open JoinFlowBean.java, add:

� � String returnValue = "/exithome";

change getReturnValue() function as:
return returnValue;

add the following to the constructor:
� � � � FacesContext context = FacesContext.getCurrentInstance();
� � � � FlowHandler handler = context.getApplication().getFlowHandler();
� � � � boolean alive = handler.isActive(context, "", "checkoutFlow");
� � � � System.out.println("checkoutFlowactive: " + alive + "###############################################################");

� � � � if (alive) {
� � � � � � Map<Object, Object> paramMap = handler.getCurrentFlowScope();
� � � � � � returnValue = (String)paramMap.get("param3Value");
� � � � }
�<Fix Imports>, <Format> and <Save>, when joinFlowBean starts, we detect if checkFlow is alive, if so, we change return from exithome.xhtml to checkoutFlowBean leaving page.
27. �Stop the server by clicking the red square icon on the left side of <GlassFish Server 5.1.0> window. <Clean and Rebuild> and <Run>,
Click <Check Out>, fill all * fields, <Continue>, <Continue>, fill all * fields, <Submit>, click <Join> button. we are jumping from checkoutFlow to joinFlow. Select a box, <submit>, <Exit Flow>. See, we come back to page Four from joinFlow. <Exit Flow>, we are out of checkoutFlow. Check the message in <<GlassFish Server 5.1.0> window, you will see both joinFlowwBean and checkoutFlowBean ended correctly.
28. mission accomplished.