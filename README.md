# SingalR-FunctionApp-Demo

This Azure Function App powers the real-time messaging backend used by clients to connect, join groups, and exchange messages.
It exposes a set of HTTP-triggered APIs that authenticate users, manage group membership, and send messages through Azure SignalR Service.

## Endpoints

### 1. **Negotiate**
```GET /api/negotiate?userId=<UserId>```

Returns the SignalR **Hub URL** and **Access Token** used by clients to establish HubConnection.

### 2. **JoinGroup**
```POST /api/JoinGroup?meetingId=<MeetingId>&userId=<UserId>```

Adds a user to a SignalR group.  
Each meeting corresponds to one group.

### 3. **LeaveGroup**
```POST /api/LeaveGroup?meetingId=<MeetingId>&userId=<UserId>```

Removes the user from the specified SignalR group.

### 4. **MessageSignalR**
```GET /api/MessageSignalR?meetingId=<MeetingId>&userId=<UserId>&message=<Message>```

Broadcasts the message to all users in the specified meeting.

The backend emits the message using: 
ReceiveDoubt(senderId, message)


## Workflow
```
Client (WPF UI)
|
|-- /negotiate ---------------------> Returns URL + Access Token
|
|-- /JoinGroup ----------------------> Adds user to a meeting group
|
|-- /LeaveGroup ---------------------> Removes user from the meeting group
|
|-- /MessageSignalR -----------------> Broadcasts message to SignalR Service
|
Azure Function App
|
Azure SignalR Service <------------------> All connected clients
```

##  Technologies Used

- Azure Function App (Isolated Worker Model)
- Azure SignalR Service
- C# (.NET 8)
- HTTP-triggered Azure Functions
- SignalR output bindings

## Minimal UI

A minimal UI that implements this Function App can be found in the following repository:

[SignalR-UI-Demo](https://github.com/112201047/SignalR-UI-Demo)

## Running Locally (Optional)
If you choose to run the Function App locally:

- Install Azure Functions Core Tools
- Add with the SignalR Connection String in local.settings.json:
    ```json
    {
    "IsEncrypted": false,
    "Values": {
      "AzureWebJobsStorage": "UseDevelopmentStorage=true",
      "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated",
      "AzureSignalRConnectionString": "<your-connection-string>"
      }
    }
    ```
- Start the function app:
  ```bash
  func start
  ```
  
## Notes
This Function App is already published, and the API is live.
The repository serves as reference for the implementation.
