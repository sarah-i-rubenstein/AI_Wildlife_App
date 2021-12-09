# AI_Wildlife_App

This project is able to identify the animal in a picture taken or uploaded by the user. The app was created using Power Apps and Power Automate. It uses the Microsoft AI for Earth API to identify animals.

<h1>Power Automate Flow</h1>

<h2>Overview:</h2>
![Powerapps](https://user-images.githubusercontent.com/44957401/145456392-fc5289d3-f50d-42d1-a621-e1ea3209b93c.PNG)


This flow:
1. Takes the blob ID of the image from Power Apps
2. Creates a variable for the ID, and empty variables for the identification and confidence
3. Gets the blob image from Azure Blob storage using the ID
4. Sends the blob image to Microsoft AI for Earth using an HTTP POST request
5. Parses the JSON respons
6. Sets the prediction and confidence variables
7. Responds to the flow with the prediction and confidence (as strings) 

<h2>1: Call flow in Power Apps</h2>
(flow must be created to do this) <br>
Assign a variable to the result: <br> 
Set(result, NameOfYourFlow.Run(varRecentID));
<br> - pass the ID of your image in blob storage


