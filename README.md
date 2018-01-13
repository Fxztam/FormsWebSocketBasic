# FormsWebSocketBasic

Websockets is a two-way extremely powerful communication protocol,
you can find here a very good summary from Oracle: http://www.oracle.com/technetwork/cn/community/developer-day/6-java-html5websocket-app-2196804-zhs.pdf

The Oracle Forms 12.2.1.3 new feature 'Websocket JS Interface' - WJSI - allows the connection to a Web Browser session for sending events/data or synchronous receiving data per JS function call,  
this is a one-way peer to peer implementation: Forms calls exactly one JS client point.

The Demo chk_websocket.fmb/chk-websocket.html tries to explain the basic functions with good transparency.

### Forms test module started with fsal:

<img src="http://www.fmatz.com/WS-final-13-01-_2018_11-28-47.png"/>

## Getting Started

This Oracle Forms Demo was inspired from: 
- https://docs.oracle.com/middleware/12213/formsandreports/deploy-forms/oracle-forms-and-javascript-integration.htm#FSDEP242
- https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=535584723136940&parent=DOCUMENT&sourceId=854157.1&id=853911.1&_afrWindowMode=0&_adf.ctrl-state=p5qvbw888_102
- https://community.oracle.com/message/14655253#14655253 

### Prerequisites

- Oracle Forms 12.2.1.3
- Jetty WebSocket server 
- check enabled WebSocket in your Web Browser : https://websocket.org/echo.html 

### Setup and Deployment

Following subsequent setups are from: https://community.oracle.com/message/14655456#14655456

#### Environment setup goes something like this:

    1. [Download jetty.jar](http://central.maven.org/maven2/org/eclipse/jetty/aggregate/jetty-all/9.4.5.v20170502/jetty-all-9.4.5.v20170502-uber.jar)  (from the Deployment Guide).
    2. Properly sign the jar.
    3. Copy the jar to the /forms/java directory.
    4. Add the jar file to ARCHIVE (or extensions.jnlp if using Web Start).
    5. Add frmwebsocketjsi.jar to ARCHIVE.

#### Application setup:

    1.  Compile websocketJSI.pll in /forms directory and save websocketJSI.plx there
    2.  Create a new form and attach websocketJSI.pll to your form.
    3.  Add object group from websocketJSI.olb per drag and copy


### Programming

In the chk_websocket.fmb:

1. Start server : websocketJSI.StartServer(8888);
2. Start session: websocketJSI.BeginSession(8888, 'MyFxSession');
3. Send/receive events/data in WHEN-BUTTON-PRESSED (BT_SEND_JS):
<img src="http://www.fmatz.com/WS-PL-13-01-_2018_13-30-06.png" />

In the chk-websocket.html:

1. In the HTML page, add a reference to the needed JS file, the the file location is in /forms/java
´´´javascript
    <script type="text/javascript" src="/forms/java/frmwebsocketjsi.js"></script>
´´´
2. Add JS code to connect to the websocket server from the HTML page.  
´´´javascript
    frmwebsocketjsi.connectToServer(8888);
´´´
3. Add JS code to connect to the desired session from the HTML page.  Example:
´´´javascript
    frmwebsocketjsi.beginSession('MyFxSession');
´´´
See the functions behind the buttons in chk_websocket.fmb and chk-websocket.html

## Running the tests

The WJSI fetures in this demo were tested on the IE11 Java plugin and the Forms Application Launcher, based on Java Version 1.8_151.

### Start with Forms Stand-alone Application Launcher (FSAL) 

The cmd file for starting with FSAL: https://github.com/Fxztam/FormsWebSocketBasic/blob/master/start_websocket-demo.cmd

Here a demo movie: http://www.fmatz.com/Forms-WebSocket-Demo.gif

In order for this to work, the web page must be connected to the corresponding session.  
This means you should not attempt to start the server and the session (in the form) then immediately attempt to do something.  

In an ideal situation the expected flow would be something like this:

>1.  Start the chk_websocket.fmx:
    - press 'Start Server'
    - press 'Server State?'
    - press 'Start Session'
    - press 'Session State?'
    - press 'Session ID?'
    - If all ready here, then next: 
>3.  Open the chk-websocket.html:
    - open in the Browser 'Development Tools' 'Console'
    - press 'Connect Server' in the chk-websocket.html
    - press 'Begin Session'
>4.  The web page session is started.
>5.  Interaction via JS can begin:
>6.  In the Form select in ComboBox:
    - 'Alert' press 'Send JavaScript'
    - 'Check Console' press 'Send JavaScript'
    - 'Get Item' press 'Send JavaScript'
    - 'Get Date> press 'Send JavaScript'
    - 'Prompt' press 'Send JavaScript'
    - 'Set Item' press 'Send JavaScript' 
>7.  Finishing the Websocket Service from chk_websocket.fmx:
    - press 'Stop Session'
    - press 'Stop Server'.

You can check the availability of a Websocket Port: press 'Port Check' .

If the web page session has been successfully established, you will see a message in the JS console (or shell) that says something like "Connected with peer" or "Connection with peer successful."  If you do not see a similar message then the web page is not talking to the websocket server and your JS calls from/to Forms won't work.

There are many other useful functions available.  Review the comments in the PLL and JS files for suggests.  Some of those functions offers way to check is a connection has been establish, if the server is running, etc.  This are helpful in order to make proper programmatic decisions about what to do next.

Here the demo movie: http://www.fmatz.com/Forms-WebSocket-Demo.gif

## Known issues

## Not implemeted

The interaction must begin from Forms to JS, listening on JS events/data on the Forms side is not inmplemented yet.
