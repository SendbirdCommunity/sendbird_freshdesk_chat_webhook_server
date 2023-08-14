# SendBird Chat Management Server

This project is a small Express server that integrates with the SendBird API to manage chat channels. It provides endpoints to add and remove users from chat channels based on ticket events, making it suitable for use with a ticketing system.

## Prerequisites

- Node.js (version 12 or higher)
- A SendBird account and application

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your-username/sendbird-chat-management-server.git
   cd sendbird-chat-management-server
   ```

2. **Create a .env file:**

   Before installing the dependencies, create a `.env` file in the root directory of the project with the following content:

   ```env
   APP_ID=your_sendbird_app_id
   API_TOKEN=your_sendbird_api_token
   ```

   Replace `your_sendbird_app_id` and `your_sendbird_api_token` with the actual values from your SendBird application.

3. **Install the dependencies:**

   ```bash
   npm install
   ```

4. **Start the server:**

   ```bash
   npm start
   ```

   The server will start on port 3000, and you can interact with it using the defined endpoints.

## Usage: Set up a webhook in Freshdesk

The following set up will send a webhook any time a ticket's agent is changed. Additionally, if the ticket's agent is changed to none, or from none to and agent. 
This service works for tickets changes in the ticket view and via bulk edit. For bulk edit, the webhook will be sent for each ticket that is changed.

1. Set up Freshdesk Webhooks - at least Growth plan is needed with Time/event based automations.
2. Admin --> Workflow --> Automations --> Ticket Updates --> New rule 
3. Rule name  --> `Sendbird Webhooks` (Or name of your choice)
4. When an action performed by... `Agent or Requester`
5. Involves any of these events -->  `Ticket is` -->  `Updated`
6. Add new event --> `Agent is updated` --> From `Any Agent` To `Any Agent`
7. Add new event --> `Agent is updated` --> From `None` To `Any Agent`
8. Add new event --> `Agent is updated` --> From `Any Agent` To `None`
9. On tickets with these properties --> Do nothing
10. Perform these actions --> `Trigger webhook`
11. Request type --> `POST`
12. URL --> `https://your_server_url/webhook`
13. Encoding --> `JSON`
14. Content --> `Advanced`
15. Write custom API request (below) 
```json
{
"ticket_id": {{ticket.id}},
"new_agent_email": {{ticket.agent.email}},
"event": {{triggered_event}},
"sender_id":{{ticket.contact.unique_external_id}}
}
```

Request body in webhook looks like this:
```json
{
   "ticket_id": 24,
   "new_agent_email": freshdesk_agent1@mailinator.com,
   "event": {responder_id:{from:150006383528,to:150014937111}},
   "sender_id":jason_4
}
```
## API Endpoints

- **POST `/tickets`**: Manages users in SendBird chat channels based on ticket events. Accepts a JSON payload with ticket information and performs the necessary add or remove actions.

## Development

For local development, you can run the server with:

```bash
npm run dev
```

Ensure that the `.env` file is properly configured with your SendBird application credentials.

## Contributing

If you want to contribute to this project, please open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Support

For any questions or support, please contact the maintainer at [your-email@example.com](mailto:your-email@example.com).

