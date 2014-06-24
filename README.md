# smtp-connection

Connect to SMTP servers

This is a fork of [simplesmtp](https://github.com/andris9/simplesmtp). Includes only the client and nothing more. API is simplified and should be easier to use.

## Usage

Install with npm

    npm install smtp-connection

Require in your script

    var SMTPConnection = require('smtp-connection');

### Create SMTPConnection instance

```javascript
var connection = new SMTPConnection(options);
```

Where

  * **options** defines connection data
    * **options.port** is the port to connect to (defaults to 25 or 465)
    * **options.host** is the hostname or IP address to connect to (defaults to 'localhost')
    * **options.secure** defines if the connection should use SSL (if `true`) or not (if `false`)
    * **options.ignoreTLS** turns off STARTTLS support if true
    * **options.name** optional hostname of the client, used for identifying to the server
    * **options.localAddress** is the local interface to bind to for network connections
    * **options.connectionTimeout** how many milliseconds to wait for the connection to establish
    * **options.greetingTimeout** how many milliseconds to wait for the greeting after connection is established
    * **options.socketTimeout** how many milliseconds of inactivity to allow
    * **options.debug** if true, the connection emits all traffic between client and server as 'log' events
    * **options.authMethod** defines preferred authentication method, eg. 'PLAIN'
    * **options.tls** defines additional options to be passed to the socket constructor, eg. *{rejectUnauthorized: true}*

### Events

SMTPConnection instances are event emitters with the following events

  * **'error'** *(err)* emitted when an error occurs. Connection is closed automatically in this case.
  * **'connect'** emitted when the connection is established
  * **'log'** *(data)* emitted for all traffic when debug option is set to true
  * **'end'** when the instance is destroyed

### connect

Establish the connection

```javascript
connection.connect(callback)
```

Where

  * **callback** is the function to run once the connection is established. The function is added as a listener to the 'connect' event.

### login

If the server requires authentication you can login with

```javascript
connection.login(auth, callback)
```

Where

  * **auth** is the authentication object
    * **auth.user** is the username
    * **auth.pass** is the password for the user
    * **auth.xoauth2** is the OAuth2 access token (preferred if both `pass` and `xoauth2` values are set)
  * **callback** is the callback to run once the authentication is finished. Callback has the following arugments
    * **err** and error object if authentication failed

### send

Once the connection is authenticated (or just after connection is established if authentication is not required), you can send mail with

```javascript
connection.send(envelope, message, callback)
```

Where

  * **envelope** is the envelope object to use
    * **envelope.from** is the sender address
    * **envelope.to** is the recipient address or an array of addresses
  * **message** is either a String, Buffer or a Stream. All newlines in converted to \r\n and all dots are escaped automatically, no need to convert anything before.
  * **callback** is the callback to run once the sending is finished or failed. Callback has the following arugments
    * **err** and error object if sending failed
    * **info** information object about accepted and rejected recipients
      * **acepted** and array of accepted recipient addresses
      * **rejected** and array of rejected recipient addresses

### quit

Use it for graceful disconnect

```javascript
connection.quite();
```

### close

Use it for less graceful disconnect

```javascript
connection.close();
```

## License

**MIT**