# Session handling
## Use macro to login before each requests
1.  With Burp running, log in as `carlos` and investigate the 2FA verification process. Notice that if you enter the wrong code twice, you will be logged out again. You need to use Burp's session handling features to log back in automatically before sending each request.
2.  In Burp, go to "Project options" > "Sessions". In the "Session Handling Rules" panel, click "Add". The "Session handling rule editor" dialog opens.
3.  In the dialog, go to the "Scope" tab. Under "URL Scope", select the option "Include all URLs".
4.  Go back to the "Details" tab and under "Rule Actions", click "Add" > "Run a macro".
5.  Under "Select macro" click "Add" to open the "Macro Recorder". Select the following 3 requests:  
     `GET /login  
        POST /login  
        GET /login2`  
    Then click "OK". The "Macro Editor" dialog opens.
6.  Click "Test macro" and check that the final response contains the page asking you to provide the 4-digit security code. This confirms that the macro is working correctly.
7.  Keep clicking "OK" to close the various dialogs until you get back to the main Burp window. The macro will now automatically log you back in as Carlos before each request is sent by Burp Intruder.
8.  Send the `POST /login2` request to Burp Intruder.
9.  In Burp Intruder, add a payload position to the `mfa-code` parameter.
10.  On the "Payloads" tab, select the "Numbers" payload type. Enter the range 0 - 9999 and set the step to 1. Set the min/max integer digits to 4 and max fraction digits to 0. This will create a payload for every possible 4-digit integer.
11.  Go to the "Resource pool" tab and add the attack to a resource pool with the "Maximum concurrent requests" set to `1`.
12.  Start the attack. Eventually, one of the requests will return a 302 status code. Right-click on this request and select "Show response in browser". Copy the URL and load it in your browser.
13.  Click "My account" to solve the lab.