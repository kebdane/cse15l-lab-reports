# Lab Report 2

## Part 1

ChatServer.java code
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    ArrayList<String> messages = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("&");
            
            String[] fStrings = (parameters[0].split("="));
            String[] sStrings = (parameters[1].split("="));

            messages.add(fStrings[1] + ": " + sStrings[1]);
        }  
        String allMessages = "";
            for(int i = 0; i < messages.size(); i++){
                allMessages += ("\n" + messages.get(i));
            }
        if(messages.size() > 0){
            return allMessages;
        }else{
            return "404 Not Found!";
        }
    }
}       

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
![Image](LabReport2ss.png)
* `handleRequest()` method was called
* The relevant argument to this method was the `URI url`, in this case, `0-0-0-0-4000-drhuco7qoasp3f16fmgqqu42oO.us.edusercontent.com/add-message?s=Kersten&user=Hello`. To which contains the a path request `/add-message`.
* The relevant fields would be the class variable `messages` which is an arrayList.
* Before the call, `messages` variable is empty conatinaing no String values. But after the call, `messages` will store the String that resulted from `handleRequest()` method call in which will be "Kersten: Hi".
* `messages` Arraylist changes by having more Strings to be added creating a list of Strings that are results of `handleRequest()` method with a `/add-message` path request.

![Image](LabReport2ss1.png)
* `handleRequest()` method was called
* The relevant argument to this method was the `URI url`, in this case, `0-0-0-0-4000-drhuco7qoasp3f16fmgqqu42oO.us.edusercontent.com/add-message?s=Kersten&user=Hello`. To which contains the a path request `/add-message`.
* The relevant fields would be the class variable `messages` which is an arrayList.
* Before the call, `messages` variable is conatinaing the resulted String value from the first `handleRequest()` method call request which is "Kersten: Hi". And after the call, `messages` will store the Strings that resulted from the second `handleRequest()` method call in which will be "Kersten: Hello".
* `messages` Arraylist changes by having more Strings to be added creating a list of Strings that are results of `handleRequest()` method with a `/add-message` path request.

## Part 2

### Private key
![Image](SS1.png)

### Public key
![Image](S2.png)

### Example Login
![Image](SS3.png)
* Here having a private key, made logging into ieng6 account much easier by not requiring to enter your password every time.

## Part 3
* During Lab 2 and 3, I learned a lot. Most of the activities were pretty much new to me such as building and running a server. I didn't know how to create a server before and so having a starter code that we could edit and observe its outputs was very helpful for me. I also learned few new commands such as `curl`, `mkdir`, and `scp`. I wasn't aware of these commands before and I think learning these commands will be very helpful with future activities of this class and other classes I'm taking/ will take. 

