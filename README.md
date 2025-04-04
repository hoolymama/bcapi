# Basecamp API

This package allows interaction with the [Basecamp API](https://github.com/basecamp/bc3-api) using Python.

## Installation
The package can be installed from your terminal by typing:

    pip install bcapi

You need to have python 3.11 or higher installed.

## Set up your environment

Typically

You need to create a .env file in the root of the project and add your Basecamp account ID, client ID, client secret, and redirect URI. Copy the .env.example file to get started. You'll find them on [your OAuth app page](https://launchpad.37signals.com/integrations/<your-app-id>). You'll need to login with your Basecamp account of course.

### Get a refresh token

You also need a refresh token. To interact with Basecamp's API, you must provide an access token for every API request. Access tokens are set to expire after two weeks. 

A refresh token allows you to automatically regenerate new access tokens. You only have to generate the refresh token once and after that you can use it to gain access to Basecamp each time you run your script. You must generate it and add it to your `.env` file. If you already have a Refresh token, skip this step.

To begin the authentication process, use the `auth` script:

```bash 
bcapi
```

Since your `.env` file does not contain a refresh_token, an error will be raised which contains a link for the authorization of your app. Open that link in the browser and click on "Yes, I'll allow access".

After allowing access, you'll be redirected to your redirect URI where you'll find a verification code in the URL. Use this code to complete the authentication. Run the `auth` script again with the verification code as an argument:

```bash
ciobc 17beb4cd
```

This will generate your Refresh token and use that token right away to generate the Access token for your current session. You should now save the refresh token in your `.env` file.


## 3. Using the API

Once you have authentication set up, you can start interacting with Basecamp. Here are some examples:

#### Initialize MessageBoard
```python
from ciobc.client import Client
from ciobc.messageboard import MessageBoard
```


#### Use the client as a context manager
```python
with Client() as client:
    message_board = MessageBoard(client=client)
```

#### List messages
```python
with Client() as client:
    message_board = MessageBoard(client=client)
    messages = message_board.list(
        project_id=123456,
        message_board_id=123456
    )
```

#### Create a message
```python
with Client() as client:
    message_board = MessageBoard(client=client)
	message_board.create(
		project_id=123456,
		message_board_id=123456,
		subject="Important Update",
		content="Here's the latest project update...",
		publish=True # Set to False for draft
	)
```

#### Get a message
```python
with Client() as client:
    message_board = MessageBoard(client=client)
	message = message_board.get(
		project_id=123456,
		message_id=789012
	)
```

#### Update a message
```python
with Client() as client:
    message_board = MessageBoard(client=client)
    message_board.update(	
        project_id=123456,
        message_id=789012,
        subject="Updated: Important Update",
        content="Here's the revised project update..."
    )
```



## Currently available endpoints

- MessageBoard
- People
- Projects
- Schedule Entries
- Schedules
- TodoList groups
- TodoLists
- Todos
- Todosets
