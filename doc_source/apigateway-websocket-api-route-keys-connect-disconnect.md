# Managing Connected Users and Client Apps: `$connect` and `$disconnect` Routes<a name="apigateway-websocket-api-route-keys-connect-disconnect"></a>

**Topics**
+ [The `$connect` Route](#apigateway-websocket-api-routes-about-connect)
+ [The `$disconnect` Route](#apigateway-websocket-api-routes-about-disconnect)

## The `$connect` Route<a name="apigateway-websocket-api-routes-about-connect"></a>

Client apps connect to your WebSocket API by sending a WebSocket upgrade request\. If the request succeeds, the `$connect` route is executed while the connection is being established\.

Because the WebSocket connection is a stateful connection, you can configure authorization on the `$connect` route only\. `AuthN`/`AuthZ` will be performed only at connection time\.

Until execution of the integration associated with the `$connect` route is completed, the upgrade request is pending and the actual connection will not be established\. If the `$connect` request fails \(e\.g\., due to `AuthN`/`AuthZ` failure or an integration failure\), the connection will not be made\.

**Note**  
If authorization fails on `$connect`, the connection will not be established, and the client will receive a `401` or `403` response\.

Setting up an integration for `$connect` is optional\. However, doing so can be useful, because the backend receives the user ID of clients that connect\. You should consider setting up a `$connect` integration if:
+ You want to be notified when clients connect and disconnect\.
+ You want to throttle connections or control who connects\.
+ You want your backend to send messages back to clients using a callback URL\.
+ You want to store each connection ID and other information into a database \(e\.g\., Amazon DynamoDB\)\.

## The `$disconnect` Route<a name="apigateway-websocket-api-routes-about-disconnect"></a>

The `$disconnect` route is executed after the connection is closed\.

The connection can be closed by the server or by the client\. As the connection is already closed when it is executed, `$disconnect` is a best\-effort event\. API Gateway will try its best to deliver the `$disconnect` event to your integration, but it cannot guarantee delivery\.

The backend can initiate disconnection by using the `@connections` API\. For more information, see [Use `@connections` Commands in Your Backend Service](apigateway-how-to-call-websocket-api-connections.md)\.