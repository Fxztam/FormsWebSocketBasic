# FormsWebSocketBasic

The Oracle Forms 12.2.1.3 new feature 'WebSocket JS Interface' - WSJSI - allows the connection to a Web Browser WebSocket session for sending events/data or synchronous receiving data per function call.   

## Getting Started

This Oracle Forms Demo was inspired from: 
- http://www.oracle.com/technetwork/developer-tools/forms/documentation/oracleforms-12210-newfeatures-2906037.pdf
- https://docs.oracle.com/middleware/12213/formsandreports/deploy-forms/oracle-forms-and-javascript-integration.htm#FSDEP242
- https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=535584723136940&parent=DOCUMENT&sourceId=854157.1&id=853911.1&_afrWindowMode=0&_adf.ctrl-state=p5qvbw888_102
- https://community.oracle.com/message/14655253#14655253 

### Prerequisites

- Oracle Forms 12.2.1.3
- Jetty WebSocket server 
- check enabled WebSocket in your Web Browser : https://websocket.org/echo.html 

### Setup and Deployment

Subsequent setups are from: https://community.oracle.com/message/14655456#14655456


#### Environment setup goes something like this:

    1. Download jetty.jar (refer to Deployment Guide for details)
    2. Properly sign the jar.
    3. Copy the jar to the /forms/java directory.
    4. Add the jar file to ARCHIVE (or extensions.jnlp if using Web Start).
    5. Add frmwebsocketjsi.jar to ARCHIVE.

#### Application setup:

    1.  Compile websocketJSI.pll in /forms directory and save websocketJSI.plx there
    2.  Create a new form and attach websocketJSI.pll to your form.
    3.  Add object group from websocketJSI.olb per drag and copy


### Programming

    4.  Add PL/SQL code to start the websocket server from the form. Example:
    



```sql
select name, dborn
from personals;
```
```sql
BEGIN

EXCEPTION WHEN OTHERS THEN
  
END;
```

## Running the tests

In short, your application code will be mostly the same.  
However the significant difference is in the need to start the websocket server and a session.

```js
websocketJSI.StartServer(<PortNunmer>);
```

5.  Add pl/sql code to begin a websocket session from the form.  Example:

 

        websocketJSI.BeginSession(32767, 'mySession123');

 

6.  In the HTML page, add a reference to the needed JS file.  The src location may need to be adjusted depending on the server where the HTML page lives. Example:

 

          <script type="text/javascript" src="/forms/java/frmwebsocketjsi.js"></script>

 

7.  Add JS code to connect to the websocket server from the HTML page.  Example:

 

          frmwebsocketjsi.connectToServer(32767);

 

8.  Add JS code to connect to the desired session from the HTML page.  Example:

 

          frmwebsocketjsi.beginSession('mySession123');

 

9.  Add the desired JS code to your HTML page based on your needs.  Refer to the Deployment Guide for examples.


In order for this to work, the web page must be connected to the corresponding session.  This means you should not attempt to start the server and the session (in the form) then immediately attempt to do something.   In an ideal situation the expected flow would be something like this:

 

1.  The form is start.

2.  From the form, the websocket server and session are started.

3.  The web page is opening if not already.

4.  The web page session is started.

5.  Interaction via JS begins.

 

If the web page session has been successfully established, you will see a message in the Java console (or shell) that says something like "Connected with peer" or "Connection with peer successful."  If you do not see a similar message then the web page is not talking to the websocket server and your JS calls from/to Forms won't work.

 

There are many other useful functions available.  Review the comments in the PLL and JS files for suggests.  Some of those functions offers way to check is a connection has been establish, if the server is running, etc.  This are helpful in order to make proper programmatic decisions about what to do next.

## Known issues

## Not implemeted


