# AI_Wildlife_App

This project is able to identify the animal in a picture taken or uploaded by the user. The app was created using Power Apps and Power Automate. It uses the Microsoft AI for Earth API to identify animals.

Resources you will need to recreate:
1. Microsoft Power Apps
2. Microsoft Power Automate
3. A Microsoft AI for Earth API Key
4. (optional) the Microsoft Azure Storage Explorer (<a href="https://azure.microsoft.com/en-us/features/storage-explorer/#overview">free download</a>)

The logic flow of this project:
1. Send Photo taken/uploaded in Power Apps to Azure blob storage
2. Send ID of blob to Power Automate flow (which calls the Microsoft AI for Earth API)
3. Recieve the identification and confidence in Power App and display

<h1>Add Photo to Blob Storage</h1>
We did this using a premuim Power App connector
<img src="https://user-images.githubusercontent.com/44957401/145462733-10178a22-ab93-4fd8-bb15-0d8eaf227d0c.png">
To create this connection...
<ol>
    <li>Select "Add Data" in the Power Apps "Data" section</li>
<li>Enter the specified values, such as storage account name and primary key (you can get both of these values in the Microsoft Azure Storage Explorer)</li>
    <li>Add photo from camera/upload to blob storage:</li>
</ol>
Set(varRecentID, Text(Round(Rand() *1000000,0), "[$-en]000#")); <br>
AzureBlobStorage.CreateFile("nameOfContainer", varRecentID, Camera1.Stream);  
<br> <br>
...this ID will be sent to a Power Automate Flow

<h1>Power Automate Flow</h1>
Note: The first and last steps must be done in Power Apps
<h2>Overview:</h2>
<img src="https://user-images.githubusercontent.com/44957401/145456603-fad9c1e8-bf19-45ba-af8f-76bc154e8188.PNG">

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

<ul>
    <li>pass the ID of your image in blob storage</li>
</ul>

<h2>2: Create a variable for ID, identification, and confidence in Power Automate</h2>

![powerapp2](https://user-images.githubusercontent.com/44957401/145456812-cbf3aaf4-e063-4984-9381-cd7448d73bbb.png)

<h2>3: Get the blob image from Azure Blob storage</h2>

![Screenshot 2021-12-09 124633](https://user-images.githubusercontent.com/44957401/145457081-086eb165-b842-4580-b646-46be774c0735.png)

<h2>4: Send image to API </h2>

![apishot](https://user-images.githubusercontent.com/44957401/145457549-12800892-4723-4afc-85e1-7ddb1a1f83a2.png)

<h2>5: Parse JSON </h2>

![json](https://user-images.githubusercontent.com/44957401/145457709-ee84ce6f-b624-4a0f-ac72-1916f9fd3324.png)

Schema:
```
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
```
<h2>6: Set prediction and confidence variables</h2>

![setVariables](https://user-images.githubusercontent.com/44957401/145458949-e10370ff-2084-4251-8a5f-dadc42c8c926.png)

(variables must be set in a loop, but we're only sending one image at a time, so the loop will always run once)

<h2>7: Respond to flow </h2>

![respond](https://user-images.githubusercontent.com/44957401/145459157-b051bb4d-3cf6-4026-b9e7-449bc82ef7d9.png)

<h2>8: Set variables in Power Apps </h2>

```
//stores identification
Set(identification,result.identification);

//stores confidence
Set(confidence,result.confidence);
```

<h2>Contact Information</h2>
If you have any questions please contact me at <a href = "mailto: sarah.i.rubenstein@gmail.com">sarah.i.rubenstein@gmail.com</a>.
