# AI_Wildlife_App

This project is able to identify the animal in a picture taken or uploaded by the user. The app was created using Power Apps and Power Automate. It uses the Microsoft AI for Earth API to identify animals.

<h1>Power Automate Flow</h1>
Note: The first and last steps must be done in Power Apps
<h2>Overview:</h2>
![Powerapps](https://user-images.githubusercontent.com/44957401/145456603-fad9c1e8-bf19-45ba-af8f-76bc154e8188.PNG)


This flow:
1. Takes the blob ID of the image from Power Apps
2. Creates a variable for the ID, and empty variables for the identification and confidence
3. Gets the blob image from Azure Blob storage using the ID
4. Sends the blob image to Microsoft AI for Earth using an HTTP POST request
5. Parses the JSON respons
6. Sets the prediction and confidence variables
7. Responds to the flow with the prediction and confidence (as strings) 
8. Set variables in Power Apps

<h2>1: Call flow in Power Apps</h2>
(flow must be created to do this) <br>
Assign a variable to the result: <br> 
Set(result, NameOfYourFlow.Run(varRecentID));
<br> - pass the ID of your image in blob storage

<h2>2: Create a variable for ID, identification, and confidence in Power Automate</h2>

![powerapp2](https://user-images.githubusercontent.com/44957401/145456812-cbf3aaf4-e063-4984-9381-cd7448d73bbb.png)

<h2>3: Get the blob image from Azure Blob storage</h2>

![Screenshot 2021-12-09 124633](https://user-images.githubusercontent.com/44957401/145457081-086eb165-b842-4580-b646-46be774c0735.png)

<h2>4: Send image to API </h2>

![apishot](https://user-images.githubusercontent.com/44957401/145457549-12800892-4723-4afc-85e1-7ddb1a1f83a2.png)

<h2>5: Parse JSON </h2>

![json](https://user-images.githubusercontent.com/44957401/145457709-ee84ce6f-b624-4a0f-ac72-1916f9fd3324.png)

Schema:

{
    "properties": {
        "bboxes": {
            "items": {
                "properties": {
                    "confidence": {
                        "type": "number"
                    },
                    "x_max": {
                        "type": "number"
                    },
                    "x_min": {
                        "type": "number"
                    },
                    "y_max": {
                        "type": "number"
                    },
                    "y_min": {
                        "type": "number"
                    }
                },
                "required": [
                    "confidence",
                    "x_max",
                    "x_min",
                    "y_max",
                    "y_min"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "predictions": {
            "items": {
                "properties": {
                    "class": {
                        "type": "string"
                    },
                    "class_common": {
                        "type": "string"
                    },
                    "confidence": {
                        "type": "number"
                    },
                    "family": {
                        "type": "string"
                    },
                    "family_common": {
                        "type": "string"
                    },
                    "genus": {
                        "type": "string"
                    },
                    "genus_common": {
                        "type": "string"
                    },
                    "kingdom": {
                        "type": "string"
                    },
                    "kingdom_common": {
                        "type": "string"
                    },
                    "order": {
                        "type": "string"
                    },
                    "order_common": {
                        "type": "string"
                    },
                    "phylum": {
                        "type": "string"
                    },
                    "phylum_common": {
                        "type": "string"
                    },
                    "species": {
                        "type": "string"
                    },
                    "species_common": {
                        "type": "string"
                    },
                    "subphylum": {
                        "type": "string"
                    },
                    "subphylum_common": {
                        "type": "string"
                    },
                    "subspecies": {
                        "type": "string"
                    },
                    "subspecies_common": {
                        "type": "string"
                    }
                },
                "type": "object"
            },
            "type": "array"
        }
    },
    "type": "object"
}

<h2>6: Set prediction and confidence variables</h2>

![setVariables](https://user-images.githubusercontent.com/44957401/145458949-e10370ff-2084-4251-8a5f-dadc42c8c926.png)

(variables must be set in a loop, but we're only sending one image at a time, so the loop will always run once)

<h2>7: Respond to flow </h2>

![respond](https://user-images.githubusercontent.com/44957401/145459157-b051bb4d-3cf6-4026-b9e7-449bc82ef7d9.png)

<h2>8: Set variables in Power Apps </h2>

//stores identification <br>
Set(identification,result.identification); <br>

//stores confidence <br>
Set(confidence,result.confidence); <br>

You can now use the variables anywhere in the app.

