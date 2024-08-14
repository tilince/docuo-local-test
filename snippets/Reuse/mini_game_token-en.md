
## Overview

When a user initializes ZEGO MiniGameEngine SDK, the ZEGO server checks whether the user has the permissions to use the mini-game service based on the token in the user request. This process is called token-based authentication. Token-based authentication prevents risks caused by insufficient permissions or improper operations.

## How it works

Before you initialize ZEGO MiniGameEngine SDK, your server needs to generate a token. The ZEGO server checks whether the user is a legitimate user based on the token in the user request.

The following figure shows how to perform token-based authentication during initialization.

![/Pics/zegocloud/mini_game/TokenAuthentication1.png](//doc.oa.zego.im/Pics/zegocloud/mini_game/TokenAuthentication1.png)

1. A client applies for a token.
2. The developer server generates a token and returns the token to the client.
3. The client initializes ZEGO MiniGameEngine SDK with the token.
4. ZEGO MiniGameEngine SDK automatically sends the token to the ZEGO server for authentication.
5. The ZEGO server returns the authentication result to ZEGO MiniGameEngine SDK.
6. ZEGO MiniGameEngine SDK returns the authentication result to the client. If the client is not authorized, the initialization fails.

## Prerequisites

Before you perform token-based authentication, make sure that ZEGO MiniGameEngine SDK is integrated with the project. For more information, see [Integrate the SDK\|_blank](!ZegoMiniGameEngine-Integrate_SDK).

## Procedure

The following section describes how to generate a token on your server, configure the token by using ZEGO MiniGameEngine SDK, and handle an expired token.

### 1. Generate a token

<div class="mk-warning">

To ensure security, we recommend that you generate a token on your own server.

</div>

#### 1.1 Obtain the values of the `AppID` and `ServerSecret` parameters

Log in to the [ZEGO console\|_blank](https://console.zegocloud.com) and create a project. Obtain the values of the `AppID` and `ServerSecret` parameters that are required to connect to the mini-game service. Then, contact ZEGO business staff to activate the mini-game service.

#### 1.2 Generate a token on your server

<div class="mk-hint">

The client applies for a token from your server. Your server generates a token and sends the token to the client.

</div>

For ease of use, ZEGO provides the open source zego_server_assistant plug-in on GitHub and Gitee. The plug-in supports multiple programming languages, such as Go, C++, Java, Python, PHP, .NET, and Node.js. You can deploy the plug-in on your server to generate a token.

<table>
  <colgroup>
    <col width="10%">
    <col width="20%">
    <col width="20%">
    <col width="20%">
    <col width="15%">
    <col width="15%">
  </colgroup>
  <tbody><tr>
    <th rowspan="2">Language</th>
    <th rowspan="2">Supported version</th>
    <th rowspan="2">Core function</th>
    <th rowspan="2">Code base</th>
    <th colspan="2">Sample code</th>
  </tr>
  <tr>
    <th>User identity Token</th>
    <th>User privilege Token</th>
  </tr>
  <tr>
    <td>Go</td>
    <td>Go 1.14.15 or later</td>
    <td>GenerateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/go/src/token04">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/go/sample/sample.go">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/go/sample/sample-for-rtcroom.go">GitHub</a></li></ul></td>
  </tr>
  <tr>
    <td>C++</td>
    <td>C++ 11&nbsp; or later</td>
    <td>GenerateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/c%2B%2B">GitHub</a></li></ul></td>
    <td colspan="2"><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/c%2B%2B/sample/demo/main.cc">GitHub</a></li></ul></td>
  </tr>
  <tr>
    <td>Java</td>
    <td>Java 1.8&nbsp; or later</td>
    <td>generateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/java/token04">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/java/token04/src/im/zego/serverassistant/sample/Token04SampleBase.java">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/java/token04/src/im/zego/serverassistant/sample/Token04SampleForRtcRoom.java">GitHub</a></li></ul></td>
  </tr>
  <tr>
    <td>Python</td>
    <td>Python 3.6.8&nbsp; or later</td>
    <td>generate_token04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/python/token04">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/python/token04/test/base_sample.py">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/python/token04/test/rtcroom_sample.py">GitHub</a></li></ul></td>
  </tr>
  <tr>
    <td>PHP</td>
    <td>PHP 7.0&nbsp; or later</td>
    <td>generateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/php/token04">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/php/token04/test/test.php">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/php/token04/test/testForRtcRoom.php">GitHub</a></li></ul></td>
  </tr>
  <tr>
    <td>.NET</td>
    <td>.NET Framework 3.5&nbsp; or later</td>
    <td>GenerateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/.net">GitHub</a></li></ul></td>
    <td colspan="2"><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/.net/demo/WindowsFormsApp1/Form1.cs">GitHub</a></li></ul></td>
  </tr>
  <tr>
    <td>Node.js</td>
    <td>Node.js 8&nbsp; or later</td>
    <td>generateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/nodejs">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/nodejs/sample/sample-base.js">GitHub</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/ZEGOCLOUD/zego_server_assistant/blob/master/token/nodejs/sample/sample-rtc-room.js">GitHub</a></li></ul></td>
  </tr>
</tbody></table>


Take Go language as an example, you can do the following steps to generate a Token：

1. go get github.com/ZEGOCLOUD/zego_server_assistant
2. import "github.com/ZEGOCLOUD/zego_server_assistant/token/go/src/token04"
3. Call the `GenerateToken04` method to generate a Token.

The following code shows how to generate a user identity Token:


```go
package main

import (
    "fmt"
    "github.com/ZEGOCLOUD/zego_server_assistant/token/go/src/token04"
)

/*
Sample code for generating a user identity Token: 
*/

func main() {
    var appId uint32 = <Your AppId>   // The application ID that you obtain after you create a project in the ZEGO console. The value is of the UINT32 type.
    userId := <Your userID>  // The user ID that you specify. The value is of the STRING type.
    secret := <ServerSecret>  // The secret key that you obtain after you create a project in the ZEGO console. The value is a string of 32 bytes.
    var effectiveTimeInSeconds int64 = <Your token effectiveTime> // The validity period of the token. The value is of the INT64 type. Unit: seconds.
    var payload string = ""

    token, err := token04.GenerateToken04(appId, userId, serverSecret, effectiveTimeInSeconds, payload)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(token)
}
```






