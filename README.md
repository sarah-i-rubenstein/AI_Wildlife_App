# AI_Wildlife_App

This project is able to identify the animal in a picture taken or uploaded by the user. The app was created using Power Apps and Power Automate. It uses the Microsoft AI for Earth API to identify animals.

<h1>Power Automate Flow</h1>

Overview:
![IMG-5703](https://user-images.githubusercontent.com/44957401/145454457-1e1be652-d33b-403e-bc47-d735b69f3416.jpg)

This flow:
1. Takes the blob ID of the image from Power Apps
2. Creates a variable for the ID, and empty variables for the identification and confidence
3. Gets the blob image from Azure Blob storage using the ID
4. Sends the blob image to Microsoft AI for Earth using an HTTP POST request
5. Parses the JSON respons
6. Sets the prediction and confidence variables
7. Responds to the flow with the prediction and confidence (as strings) 

<h2>1: Call flow in Power Apps</h2>
(flow must be created to do this)
Assign a variable to the result: <br> 
Set(result,NameOfYourFlow.Run(varRecentID));
<br> - pass the ID of your image in blob storage


