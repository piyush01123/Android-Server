# Android HTTP Server

This repository provides a basic implementation of HTTP server on Android using the [HttpServer](https://docs.oracle.com/javase/8/docs/jre/api/net/httpserver/spec/com/sun/net/httpserver/HttpServer.html) library.

**Step 1:** Download these 2 files and place in app/libs:
[http-2.2.1.jar](https://github.com/piyush-kgp/Android-Server/blob/master/app/libs/http-2.2.1.jar) and [sun-common-server.jar](https://github.com/piyush-kgp/Android-Server/blob/master/app/libs/sun-common-server.jar)

**Step 2:** Add these dependencies to your [build.gradle](https://github.com/piyush-kgp/Android-Server/blob/master/app/build.gradle):
```
dependencies{
...
implementation files('libs/sun-common-server.jar')
implementation files('libs/http-2.2.1.jar')
}
```
**Step 3:** Add this snippet to your `onCreate` method of `MainActivity` class:
```
try {
    HttpServer server = HttpServer.create(new InetSocketAddress("0.0.0.0", 8080), 0);
    server.createContext("/", new MyHandler());
    server.setExecutor(null); // creates a default executor
  server.start();
} catch (IOException e){
    Log.e("SUNServer", e.toString());
}
```

Separately add this class to the same file:
```
class MyHandler implements HttpHandler {
    public void handle(HttpExchange t) throws IOException {
        InputStream is = t.getRequestBody();
        is.read(); // .. read the request body
  String response = "Hello World";
        t.sendResponseHeaders(200, response.length());
        OutputStream os = t.getResponseBody();
        os.write(response.getBytes());
        os.close();
    }
```
Also add the required imports at the top:
```
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetSocketAddress;

import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;
import com.sun.net.httpserver.HttpExchange;
```
See [MainActivity.java](https://github.com/piyush-kgp/Android-Server/blob/master/app/src/main/java/com/piyush/staticserver/MainActivity.java) for example.

**Step 4:** Add this permission in your `AndroidManifest.xml`:
```
<uses-permission android:name="android.permission.INTERNET" />
```

After this build and run the application on your Android device. You should be able to see a running server at port 8080 on the device which is also accessible from the local network.

#### Where to go from here?
One may require to set up an Android Server for hundreds of reasons. The exact requirements may vary but this is a good starting point. To accept a POST request for an image (say to create a peer-to-peer image sharing app) for example, `MyHandler` class will need to change. Refer to the [documentation](https://docs.oracle.com/javase/8/docs/jre/api/net/httpserver/spec/com/sun/net/httpserver/HttpServer.html) for more.