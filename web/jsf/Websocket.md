1. In the Java EE 8 tutorial examples, there are 2 examples about Websocket, but there are old JSF 2.2 examples. The  Java EE 8 tutorial document does describe JSF 2.3 Websocket, but no example is provided.
2. Let's make one for them.
2.1 Open menu <File> -> <New Project>, select <Java with Maven> and <Web Application>, <Next>, Make the project name: websocket_c, <Next>, select <Java EE 8 Web>, make sure Server is GlassFish Server 5.1.0, <Finish>
2.2 Change the project name if it is not websocket_c.
2.3 Right click the project and choose <Properties>, click <Sources>, make sure <Source/Binary Format> is 1.8. click Frameworks, add <JavaServer Faces>, click <Compile>, Java Platform is JDK 1.8, click <Run>, Server is GlassFish 5.1.0 and Java EE Version is Java EE 8 Web. <OK>
2.3.1 Open pom.xml, add or modify javax.faces dependency like this:
        <dependency>
            <groupId>org.glassfish</groupId>
            <artifactId>javax.faces</artifactId>
            <version>2.3.9</version>
            <scope>provided</scope>
        </dependency>
the <scope>provided</scope> tells Netbeans, use Netbeans' JSF 2.3.9 library to compile, but do not pack it to the .war file, GlassFish 5.1.0 server will use its own JSF 2.3.9 to run this project. The Netbeans' JSF 2.3.9 library is not working well at run time for me.
2.5 open web.xml, change url-pattern to *.xhtml, change welcome file to index.xhtml, change session-timeout to 300, so we have enough time to talk about it. welcome file is index.xhtml, add the following to it:
     <context-param>
        <param-name>javax.faces.ENABLE_WEBSOCKET_ENDPOINT</param-name>
        <param-value>true</param-value>
    </context-param>
3. <Save>.
4. Replace whole index.xhtml by this:

<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:f="http://xmlns.jcp.org/jsf/core">
    <h:head>
        <title>Facelet Title</title>
        <script type="text/javascript">
            function onMessage(message, channel, event) {
                //alert(message);
                var arraypv = message.split("/");
                document.getElementById("form1:price").innerHTML = arraypv[0];
                document.getElementById("form1:volume").innerHTML = arraypv[1];
            }
            function  onOpen(event) {
                //alert("Client try to call the server");
            }
        </script>        
    </h:head>
    <h:body><h3>JSF 2.3 WebSocket</h3>
        <h4>Server Push: Deliver everything to your door.</h4>
        <h:form id="form1">
            <h:outputLabel value="Price: " for="price"/> <h:outputText id="price" />
            <p/>
            <h:outputLabel value="Volume: " for="volume"/> <h:outputText id="volume"/>
            <p/>
            <h:commandButton onclick="jsf.push.open('form1:c1');" action="#{helloBean.start()}" value="Start push">
                <f:ajax/>
            </h:commandButton>
            <h:commandButton onclick="jsf.push.close('form1:c1');" action="#{helloBean.pause()}" value="Pause push">
                <f:ajax/>
            </h:commandButton>
            <p/>
            <f:websocket id="c1" channel="helloChannel" connected="false"
                         onmessage="onMessage" onopen="onOpen" scope="session">
                <f:ajax event="newMsg" render="price"/>
            </f:websocket>
        </h:form>        
    </h:body>
</html>

5. Menu <Source> -> <Format>, <Save>.
6. Delete all Java code from Source Packages, delete META-INF from src/main/resources under Other Source.
7 Right click Source Package, choose <New> -> <Others...>, choose <JavaServer Faces> and <JSF CDI Bean>, class name is: PushSessionBean, package: is view, scope is session. Make it like this:

package view;

import javax.inject.Named;
import java.io.Serializable;
import javax.enterprise.context.SessionScoped;
import javax.faces.push.Push;
import javax.faces.push.PushContext;
import javax.inject.Inject;

@Named(value = "pushSessionBean")
@SessionScoped
public class PushSessionBean implements Serializable {

    @Inject
    @Push(channel = "helloChannel")
    PushContext helloChannel;

    public PushContext getHelloChannel() {
        return helloChannel;
    }

    public void setHelloChannel(PushContext helloChannel) {
        this.helloChannel = helloChannel;
    }
}

8.  Right click Source Package, choose <New> -> <Others...>, choose <JavaServer Faces> and <JSF CDI Bean>, class name is: HelloBean, package: is view, scope is view. make it like this:

package view;

import javax.inject.Named;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ThreadLocalRandom;
import static java.util.concurrent.TimeUnit.SECONDS;
import javax.annotation.PostConstruct;
import javax.faces.view.ViewScoped;
import javax.faces.push.PushContext;
import javax.inject.Inject;

@Named(value = "helloBean")
@ViewScoped
public class HelloBean implements Serializable  {

    @Inject
    PushSessionBean pushSessionBean;
   
    PushContext helloChannel;
   
    public HelloBean() {
    }

    @PostConstruct
    public void init() {
        helloChannel = pushSessionBean.getHelloChannel();
    }

    private ScheduledExecutorService scheduler = null;
    private volatile double price = 100.0;
    private volatile int volume = 300000;
    private volatile String msg = null;

    volatile boolean push = false;


    public void pause() {
        System.out.println("pause push ###################################################################");
        push = false;
    }

    public void start() {
        System.out.println("start push ###################################################################");
        push = true;
        if (scheduler != null) {
            return;
        }
        scheduler = Executors.newScheduledThreadPool(1);
        Runnable beeper = new Runnable() {
            @Override
            public void run() {
                if (!push) {
                    return;
                }
                try {
                    if (helloChannel != null) {
                    price += (ThreadLocalRandom.current().nextInt(0, 101) - 50) / 100.0;
                    volume += ThreadLocalRandom.current().nextInt(5000) - 2500;
                    msg = String.format("%.2f / %d", price, volume);
                    helloChannel.send(msg);
                } else {
                    System.out.println("helloChannel null............................................................");
                }
            }
            catch (Exception e) {
                System.out.println("e=" + e);
            }
        }

    };
        scheduler.scheduleAtFixedRate (beeper, 0, 1, SECONDS);
    }
}

9. <Save>, <Clean and Build> and <Run>.
10. In the index.xhtml, we declare a websocket, the id is used by javascript. The channel name is used by the push server side too. The initial socket status is not connected. Call the javascript function onMessage() when the message comes. Call the javascript function onOpen() when the socket connection opens. scope="session" means each session should have its own socket connection. the f:ajax is useless here, without it, the push server is not happy.
11. Javascript function onMessage gets called when a message comes in. It puts the message into 2 html elements. We have 2 commandButton to manage connections. When the <start push> button is clicked, it calls a Javascript function to open a connection, and calls the bean method to start push.   <Pause push> button calls a Javascript function to close the connection, and calls the bean method to stop push.
12.  In Java code side, we use a session bean to inject a push context and we use a view scope bean to inject the session bean to retrieve the push context. The view scope bean uses a timer to generate messages and push to the client side continually, the client side uses start and pause to control the server message pushing.
This is one mode to use websocket, the push side will deliver everything to the client. Another way to use websocket is only notifying the client that a new message is ready on the push server side to pick up. Let's see what it looks like.
13. Insert the following below </f:websocket> in index.xhtml
            <hr/>
            <p/>
            <h4>Server Push: Push a notification message only, you pick up everything in store.</h4>
            <f:websocket id="c2" channel="pickupInStoreChannel" connected="false" scope="session">
                <f:ajax event="newArrival" render="orderItemList"/>
            </f:websocket>

            <h:selectOneListbox id="orderItemList" size="10" style="width:150px;">
                <f:selectItems value="#{helloBean.orderItemList}"/>
            </h:selectOneListbox>
This is our second websocket with a different name. It does not have an onMessage method, instead, it waits for a message "newArrival" from the pushing side, when the message comes, the client knows the new message is ready for pickup.  It retrieve the orderItemList from helloBean,
14. Change onclick="jsf.push.open('form1:c1')" to:
onclick="jsf.push.open('form1:c1'); jsf.push.open('form1:c2');"
15. Change onclick="jsf.push.close('form1:c1')" to:
onclick="jsf.push.close('form1:c1'); jsf.push.close('form1:c2');"

16. Insert the second push context into PushSessionBean.java:
    @Inject
    @Push(channel="pickupInStoreChannel")
    PushContext pickupInStoreChannel;
and right click, choose <Insert Code...>, <Getter and Setter...>, check <pickupInStoreChannel>, <Generate>.
17. Add the following to HelloBean.java:

    PushContext pickupInStoreChannel;

    String notification = "newArrival";
    List<String> orderItemList;
    volatile int j = 0;
    volatile int i = 0;
    List<String> typeList;

    public List<String> getOrderItemList() {
        return orderItemList;
    }

    public void setOrderItemList(List<String> orderItemList) {
        this.orderItemList = orderItemList;
    }

18. add the following to @PostConstruct init() method:
        pickupInStoreChannel = pushSessionBean.getPickupInStoreChannel();
        orderItemList=new ArrayList<String>();
        typeList=new ArrayList<String>();
        typeList.add("Food");
        typeList.add("iPhone");
        typeList.add("Cloths");
19. Add the following to the method run() below helloChannel.send(msg):

        while ( true ) {
                            if ( i < 4 ) {
                                i++;
                                break;
                            }
                            orderItemList.clear();
                            String type = typeList.get(j);
                            orderItemList.add(type + " 1");
                            orderItemList.add(type + " 2");
                            orderItemList.add(type + " 3");
                            i++;
                            if ( i >= 5 )
                                i = 0;
                            j++;
                            if (j >= 3)
                                j = 0;
                            pickupInStoreChannel.send(notification);
                }
20. Do <Fix Imports> by right clicking the code area. Do <format> by menu Source> -> <Format>. The code generates the new messages and then sends "newArrival" to the client.
21.  <Save>, <Clean and Build> and <Run>.