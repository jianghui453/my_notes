# http status head

#### Authentication

- **Authorization**
Contains the credentials to authenticate a user-agent with a server.

#### Caching

#### Client hints

#### Conditionals

#### Connection management

- **Connection**
Controls whether the network connection stays open after the current transaction finishes.
- **Keep-Alive**
Controls how long a persistent connection should stay open.

#### Content negotiation

- **Accept**
Informs the server about the types of data that can be sent back.
- **Accept-Charset**
Which character encodeings the client understands.
- **Accept-Encoding**
The encoding algorithm, usually a compression algorithm, that can be used on the resource sent back.
- **Accept-Language**
Informs the server about the human language the server is excepted to send back. This is a hint and is not necessarily under the full control of the user: the server should always pay attention not to override an explicit user choice (like selecting la language from a dropdown).

#### Controls

#### Cookies

- **Cookie**
Contains stored HTTP cookies previously sent by the server with the **Set-Cookie** header
- **Set-Cookie**
Send cookies from the server to the user-agent

#### CORS

- **Access-Control-Allow-Origin**
Indicates whether the response can be shared.
- **Access-Control-Allow-Credentials**
Indicates whether the response to the request can be exposed when the credentials flag is true.

#### Do Not Track

#### Download

#### Message bofy information

#### Proxies

#### Redirects

- **Location**
Indicates the URL to redirect a page to.

#### Request context

#### Response context

#### Range requests

#### Security

#### Server-sent events

#### Transfer coding

#### WebSockets

#### Other

## 引用

- [Mozilla: HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
