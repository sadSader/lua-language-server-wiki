This server have been supporting multi-workspace by server side since 2.6.0.
The client just need to startup one server, then send all lua files to this server(even if they are not included in current workspaces).

Note: the server does not support dynamically add or remove workspaces. If workspaces change, client should restart server.