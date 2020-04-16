# node-red-node-discord
Inspired by [node-red-contrib-discord](https://github.com/jorisvddonk/node-red-contrib-discord)

Node-red nodes that allow you to interact with Discord, via [Discord.js](https://discord.js.org).
Can be used to implement simple write/read Bots

# Installation

Run the following command in `~/.node-red`:

    npm install node-red-node-discord

# Nodes

node-red-node-discord gives you access to following nodes:

* discord-get-messages is a node with no inputs and one output allowing you to receive notifications of incoming messages.
* discord-get-emoji-reactions is a node with no inputs and one output allowing you to receive notifications when user reacts on message with emoji.
* discord-send-messages is a node with one input and no outputs allowing you to send messages to a Discord channel.
* discord-members-monitoring is a node with one input and output, designed to get all channels and their's members for metrics.

## discord-get-messages
- Triggers whenever a message was received on Discord
- You can pass list of channels to listen to
	**Note :** valid channel list example `#1245#general#1234567#another-channel`
-	`msg.payload` will be set to the textual content of the message
- `msg.channel` will be set to an Object containing info on the [channel](https://discord.js.org/#/docs/main/stable/class/Channel) the message was received from (does not contain any discord.js functions)
-	`msg.author` will be set to an Object containing info on the [user](https://discord.js.org/#/docs/main/stable/class/User) that sent the message (does not contain any discord.js functions)
-	`msg.attachments` will be set to `Array` containing attachments info in format 
  ```typescript
{
	filename: string, // Filename
	href: string // File Url generated by Discord
}
  ```
- `msg.rawData` will be set to an Object containing info on the [message](https://discord.js.org/#/docs/main/stable/class/Message) that was received, but again without any of the discord.js functions
- To reply to a message, use the `discord-send-messages` node.

## discord-send-messages
- Sends `msg.payload` on Discord channel with id `msg.channel`
  You can pass channel name in `msg.channel` as well as id, i.e. `general` <br>
  **Note:** this feature possibly can cause sending message to wrong channel if bot connected to multiple servers and there are same named channels
- Feel free to **@mention** people in `message.payload` <br>
  **Example:** `Hello @Gago, nice module :)` , also you can use `@here, @everyone` mentions
- To use discord's rich text embed specify `msg.rich` with following content (props marked with ? are not required)
     ```typescript
  {
        title?: string;
        description?: string;
  	    url?: string;
  	    color?: ColorResolvable;
  	    timestamp?: number | Date;
  	    footer?: {
    	            icon?: string;
    	            text: string;
 	              };
  	    thumbnail?: string;
  	    author: {
   	              name: string;
                  icon?: string;
                  url?: string;
                };

  	    attachments?: Attachment[];
  	    field?: {
                  name: string;
                  value: string;
                  inline?: boolean;
	              };
  	    fields?: [
                    {
                      name: string;
                      value: string;
                      inline?: boolean;
                    }
	              ];
  }
    ```
- `msg.attachments` contains attachments to send, it must be array containing objects in format
  ```typescript
  {
     name: string;
     file: string | Buffer | Stream;
  }
  ```
  
  ## discord-members-monitoring
  - Triggered from outside, for now doesn't provide any configuration options
  - Set's monitored data to `msg.monitoringData` with following content
  ```typescript
  {
  [category: string]: [
  	{
  	id: string;
  	channelName: string;
  	members: [
  		{
  		id: string;
  		username: string;
  		joinedDate: Date;
  		permissions: PermissionString[];
  		roles: [
  			{
   			id: string;
  			name: string;
  			permissions: number;
  			}
  			]
  		}
  		]
  	}
  	]
  }
  ```
