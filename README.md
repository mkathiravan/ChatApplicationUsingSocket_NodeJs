# ChatApplicationUsingSocket_NodeJs

Socket.IO enables real-time bidirectional event-based communication. It works on every platform, browser or device, focusing equally on reliability and speed. Socket.IO is built on top of the WebSockets API (Client side) and NodeJs.

Socket.IO is not a WebSocket library with fallback options to other realtime protocols. It is a custom realtime transport protocol implementation on top of other realtime protocols. Its protocol negotiation parts cause a client supporting standard WebSocket to not be able to contact a Socket.IO server.
And a Socket.IO implementing client cannot talk to a non-Socket.IO based WebSocket or Long Polling Comet server. Therefore, Socket.IO requires using the Socket.IO libraries on both client and server side.

The official site says,
Socket.IO enables real-time bidirectional event-based communication.

The Socket.IO enables the client and server to communicate in real time. The Socket.IO NodeJs server defines the events that to be triggered on a specific route.Those events are listened by server which are emitted by clients.Also, a server can emit an event to which a client can listen.

The Socket.IO can easily be implemented over web apps using NodeJs server as a back-end. 

While communicate with NodeJs we need to follow the steps as below.

1. Initialise the Socket
2. Connect the socket to the server
3. Start emitting events or listening events

# Initalize the socket

        private void initiateSocketConnection() {

        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder().url(SERVER_PATH).build();
        webSocket = client.newWebSocket(request, new SocketListener());

    }
    
 ## Connect the socket to the server
     
          private class SocketListener extends WebSocketListener {

        @Override
        public void onOpen(WebSocket webSocket, Response response) {
            super.onOpen(webSocket, response);

            runOnUiThread(() -> {
                Toast.makeText(ChatActivity.this,
                        "Socket Connection Successful!",
                        Toast.LENGTH_SHORT).show();

                initializeView();
            });

        }

        @Override
        public void onMessage(WebSocket webSocket, String text) {
            super.onMessage(webSocket, text);

            runOnUiThread(() -> {

                try {
                    JSONObject jsonObject = new JSONObject(text);
                    jsonObject.put("isSent", false);

                    messageAdapter.addItem(jsonObject);

                    recyclerView.smoothScrollToPosition(messageAdapter.getItemCount() - 1);

                } catch (JSONException e) {
                    e.printStackTrace();
                }

            });

        }
    }
