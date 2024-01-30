# Lab Report 2

![Image](LabReport2ss.png)
![Image](LabReport2ss1.png)

import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    ArrayList<String> messages = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("&");
            String[] fStrings = null;
            String[] sStrings = null;
            
            fStrings = (parameters[0].split("="));
            sStrings = (parameters[1].split("="));

            messages.add(fStrings[1] + ": " + sStrings[1]);

            String allMessages = "";
            for(int i = 0; i < messages.size(); i++){
                allMessages += ("\n" + messages.get(i));
            }
            return allMessages;
        }   
        return "404 Not Found!";
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
