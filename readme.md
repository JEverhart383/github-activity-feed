# GitHub Activity Feed

In this repo, we'll create a real-time activity feed using Node.js, Socket.io, and GitHub webhooks. You can find [step-by-step instructions](https://knock.app/blog/building-a-github-activity-feed-with-nodejs-and-socket-io) on the [Knock blog](https://knock.app/blog).

## What we'll build

We'll create a route that responds to GitHub webhooks and emits WebSocket events using Socket.io:

```javascript
app.post("/webhook", (req, res) => {
  const event = req.body;
  // Check for a push event
  if (event && event.pusher) {
    io.emit("new_commit", event);
  }
  res.status(200).end();
});
```

Then, we'll create a simple JavaScript client that listens for those events and updates the DOM when data is received from the server:

```javascript
document.addEventListener("DOMContentLoaded", function () {
  const socket = io();

  socket.on("new_commit", function (data) {
    const notificationDiv = document.createElement("div");
    notificationDiv.className = "notification";

    const header = document.createElement("div");
    header.className = "notification-header";
    header.textContent = `New commit in ${data.repository.name}`;

    const content = document.createElement("div");
    content.textContent = `Commit by ${data.pusher.name}: "${data.head_commit.message}"`;

    notificationDiv.appendChild(header);
    notificationDiv.appendChild(content);
    document.body.appendChild(notificationDiv);
  });
});
```

## Advanced feeds

If you're looking to build more advanced feeds, [Knock's in-app feed API](https://knock.app/channels/in-app-notifications) provides support for scalable WebSockets as well as all the data modeling abstractions you need to create robust feed experiences. Combine the API with [drop-in React components](https://docs.knock.app/in-app-ui/react/feed) and you can have a fully-functional feed that can scale live in hours.
