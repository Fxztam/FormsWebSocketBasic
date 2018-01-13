# FormsWebSocketBasic

Websockets is a two-way extremely powerful communication protocol,
you can find here a very good summary from Oracle: http://www.oracle.com/technetwork/cn/community/developer-day/6-java-html5websocket-app-2196804-zhs.pdf

The Oracle Forms 12.2.1.3 new feature 'Websocket JS Interface' - WJSI - allows the connection to a Web Browser session for sending events/data or synchronous receiving data per JS function call,  
this is a one-way peer to peer implementation: Forms calls exactly one JS client point.

The Demo chk_websocket.fmb/chk-websocket.html tries to explain the basic functions with good transparency.

### Forms test module started with Forms Stand-alone Application Launcher:

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

    1. Download : jetty.jarhttp://central.maven.org/maven2/org/eclipse/jetty/aggregate/jetty-all/9.4.5.v20170502/jetty-all-9.4.5.v20170502-uber.jar  (from the Deployment Guide).
    2. Properly sign the jar.
    3. Copy the jar to the /forms/java directory.
    4. Add the jar file to ARCHIVE (or extensions.jnlp if using Web Start).
    5. Add frmwebsocketjsi.jar to ARCHIVE.
    6. Config in the formsweb.cfg:
       * enableJavascriptEvent=true
       * JavaScriptBlocksHeartBeat=true

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
```js
    <script type="text/javascript" src="/forms/java/frmwebsocketjsi.js"></script>
```
2. Add JS code to connect to the websocket server from the HTML page.  
```js
    frmwebsocketjsi.connectToServer(8888);
```
3. Add JS code to connect to the desired session from the HTML page.  Example:
```js
    frmwebsocketjsi.beginSession('MyFxSession');
```
See the functions behind the buttons in chk_websocket.fmb and chk-websocket.html

## Running the tests

The WJSI fetures in this demo were tested on the IE11 Java plugin and the Forms Application Launcher, based on Java Version 1.8_151.

### Start with Forms Stand-alone Application Launcher (FSAL) 

The cmd file for starting with FSAL: https://github.com/Fxztam/FormsWebSocketBasic/blob/master/start_websocket-demo.cmd

Following flow was succesfully tested:

1. Start the chk_websocket.fmx:
   * press 'Start Server'
   * press 'Server State?'
   * press 'Start Session'
   * press ' Session State?'
   * press 'Session ID?'
2. If all ready here, then next:
3. Open the chk-websocket.html:
   * open in the Browser 'Development Tools' 'Console'
   * press 'Connect Server' in the chk-websocket.html
   * press 'Begin Session'
4. The web page session is started.
5. Interaction via JS can begin:
6. In the Form select in ComboBox:
   * 'Alert' press 'Send JavaScript'
   * 'Check Console' press 'Send JavaScript'
   * 'Get Item' press 'Send JavaScript'
   * 'Get Date> press 'Send JavaScript'
   * 'Prompt' press 'Send JavaScript'
   * 'Set Item' press 'Send JavaScript' 
7. Finishing the Websocket Service from chk_websocket.fmx
   * press 'Stop Session'
   * press 'Stop Server'.

You can check the availability of a Websocket Port: press 'Port Check'.

Here the demo: 

<img src="http://www.fmatz.com/WS-Forms-final.gif" />

## Known issues

The flow above is important, errors due to deviations from the flow must be remedied by restarting.

## Not implemented

The interaction is a one-way from Forms to JS, listening on JS events/data on the Forms side is not implemented.
