开发者获得项目的 AppID 和 ServerSecret 信息后，根据实际业务需求，即可在自己的服务端生成 Token。开发者客户端向开发者服务端发送申请 Token 请求，由开发者服务端生成 Token 后返回给到对应客户端。

ZEGO 在 GitHub/Gitee 提供了一个开源的 zego_server_assistant 插件，支持使用 Go、C++、Java、Objective-C、Python、PHP、.NET、Node.js 语言，在开发者的服务端或客户端部署生成 Token。

zego_server_assistant 插件中提供了生成基础鉴权 Token 和生成权限认证 Token 的示例代码，两者的适用场景如下，开发者可按需选择：
- 基础鉴权 Token：用于业务的简单权限验证的场景，此时 “payload” 字段可传空。
- 权限认证 Token：为了进一步提高安全性，开放房间 ID 和推流 ID 这两个权限位，可以验证登录房间的 ID 和推流 ID，此时 “payload” 字段需要按照规则生成，可参考下表的“权限认证 Token”示例代码。权限位校验规则如下：
    - 只校验登录权限：登录时校验权限，推流时不校验权限。
    - 校验推流权限：推流时校验权限，登录时不校验权限。
    - 同时校验登录和推流权限：登录和推流时均校验权限。

<div class="mk-warning">

- 为保证安全性，强烈推荐您使用服务端生成 Token，否则会存在 ServerSecret 被窃取的风险。
- 请使用 zego_server_assistant 中的 “token04” 来生成 Token。
</div>


zego_server_assistant 插件中用于服务端生成 Token 的各语言参考信息如下：

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
    <th rowspan="2">语言</th>
    <th rowspan="2">支持版本</th>
    <th rowspan="2">关键函数</th>
    <th rowspan="2">代码库地址</th>
    <th colspan="2">示例代码地址</th>
  </tr>
  <tr>
    <th>基础鉴权 Token</th>
    <th>权限认证 Token</th>
  </tr>
  <tr>
    <td>Go</td>
    <td>Go 1.14.15 或以上版本</td>
    <td>GenerateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/tree/release/github/token/go/src/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/go/src/token04">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/go/sample/token04/sample.go">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/go/sample/token04/sample.go">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/go/sample/token04/sample-for-rtcroom.go">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/go/sample/token04/sample-for-rtcroom.go">Gitee</a></li></ul></td>
  </tr>
  <tr>
    <td>C++</td>
    <td>C++ 11 或以上版本</td>
    <td>GenerateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/c%2B%2B/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/c++/token04">Gitee</a></li></ul></td>
    <td colspan="2"><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/c%2B%2B/token04/sample/demo/main.cc">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/c%2B%2B/token04/sample/demo/main.cc">Gitee</a></li></ul></td>
  </tr>
  <tr>
    <td>Java</td>
    <td>Java 1.8 或以上版本</td>
    <td>generateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/tree/release/github/token/java/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/java/token04">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/java/token04/src/im/zego/serverassistant/sample/Token04Sample.java">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/java/token04/src/im/zego/serverassistant/sample/Token04Sample.java">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/java/token04/src/im/zego/serverassistant/sample/Token04ForRtcRoomSample.java">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/java/token04/src/im/zego/serverassistant/sample/Token04ForRtcRoomSample.java">Gitee</a></li></ul></td>
  </tr>
  <tr>
    <td>Python</td>
    <td>Python 3.6.8 或以上版本</td>
    <td>generate_token04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/tree/release/github/token/python/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/python/token04">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/python/token04/test/test.py">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/python/token04/test/test.py">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/python/token04/test/test_rtcroom.py">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/python/token04/test/test_rtcroom.py">Gitee</a></li></ul></td>
  </tr>
  <tr>
    <td>PHP</td>
    <td>PHP 7.0 或以上版本</td>
    <td>generateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/tree/release/github/token/php/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/php/token04">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/php/token04/test/test.php">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/php/token04/test/test.php">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/php/token04/test/testForRtcRoom.php">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/php/token04/test/testForRtcRoom.php">Gitee</a></li></ul></td>
  </tr>
  <tr>
    <td>.NET</td>
    <td>.NET Framework 3.5 或以上版本</td>
    <td>GenerateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/tree/release/github/token/.net/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/.net/token04">Gitee</a></li></ul></td>
    <td colspan="2"><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/feature/token04/token/.net/token04/demo/WindowsFormsApp1/Form1.cs">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/feature/token04/token/.net/token04/demo/WindowsFormsApp1/Form1.cs">Gitee</a></li></ul></td>
  </tr>
  <tr>
    <td>Node.js</td>
    <td>Node.js 8 或以上版本</td>
    <td>generateToken04</td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/tree/release/github/token/nodejs/token04">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/tree/release/github/token/nodejs/token04">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/nodejs/token04/sample/sample.js">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/nodejs/token04/sample/sample.js">Gitee</a></li></ul></td>
    <td><ul><li><a target="_blank" href="https://github.com/zegoim/zego_server_assistant/blob/release/github/token/nodejs/token04/sample/sample-rtc-room.js">GitHub</a></li><li><a target="_blank" href="https://gitee.com/zegodev_admin/zego_server_assistant/blob/release/github/token/nodejs/token04/sample/sample-rtc-room.js">Gitee</a></li></ul></td>
  </tr>
</tbody></table>


以 Go 语言为例，开发者可参考以下步骤使用 zego_server_assistant 生成 Token：

1. 使用命令 `go get github.com/zegoim/zego_server_assistant` 获取依赖包。
2. 在您的代码中，通过 `import "github.com/zegoim/zego_server_assistant/token/go/src/token04"` 引入插件。
3. 调用插件提供的 GenerateToken04 方法生成 Token。

生成基础鉴权 Token 示例代码如下：

```go
package main

import (
    "fmt"
    "github.com/zegoim/zego_server_assistant/token/go/src/token04"
)

/*
基础鉴权 Token 示例代码
*/

func main() {
    var appId uint32 = 1
    userId := "demo"
    serverSecret := "fa94dd0f974cf2e293728a526b028271"
    var effectiveTimeInSeconds int64 = 3600
    var payload string = ""

    token, err := token04.GenerateToken04(appId, userId, serverSecret, effectiveTimeInSeconds, payload)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(token)
}
```

生成权限认证 Token 的示例代码如下：

<div class="mk-warning">


生成权限认证 Token 时必须传入 “RoomId”。
</div>

```go
package main

import (
    "encoding/json"
    "fmt"
    "github.com/zegoim/zego_server_assistant/token/go/src/token04"
)

/*
权限认证 Token 生成示例代码
 */


//token业务扩展：RTC房间认证属性
type RtcRoomPayLoad struct {
    RoomId       string      `json:"room_id"`           //房间id（必填）；用于对接口的房间 id 进行强验证
    Privilege    map[int]int `json:"privilege"`         //权限位开关列表；用于对接口的操作权限进行强验证
    StreamIdList []string    `json:"stream_id_list"`    //流列表；用于对接口的流 id 进行强验证；允许为空，如果为空，则不对流 id 验证
}

func main() {
    var appId uint32 = 1
    roomId := "demo"
    userId := "demo"
    serverSecret := "fa94dd0f974cf2e293728a526b028271"
    var effectiveTimeInSeconds int64 = 3600
    privilege := make(map[int]int)
    privilege[token04.PrivilegeKeyLogin] = token04.PrivilegeEnable
    privilege[token04.PrivilegeKeyPublish] = token04.PrivilegeDisable

    payloadData := &RtcRoomPayLoad{
        RoomId:       roomId,
        Privilege:    privilege,
        StreamIdList: nil,
    }

    payload, err := json.Marshal(payloadData)
    if err != nil {
        fmt.Println(err)
        return
    }

    token, err := token04.GenerateToken04(appId, userId, serverSecret, effectiveTimeInSeconds, string(payload))
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(token)
}
```


<div class="mk-hint">


运行生成 Token 的 Java 源码时，如果出现 “java.security.InvalidKeyException:illegal Key Size” 异常提示，可参考如下步骤处理：


1. 在官方网站下载 JCE 无限制权限策略文件。
    - 如果您安装的是 JDK7，可以到 [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 7\|_blank](http://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html) 下载该文件。
    - 如果您安装的是 JDK8，可以到 [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8\|_blank](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) 下载该文件。
2. 下载完成后，解压该文件，得到 local_policy.jar 、US_export_policy.jar 和 readme.txt 文件。
    - 如果您安装了 JRE，需要将 local_policy.jar 和 US_export_policy.jar 文件拷贝到 “%JRE_HOME%\lib\security” 目录下覆盖原有文件。
    - 如果您安装了 JDK，需要将 local_policy.jar 和 US_export_policy.jar 文件都拷贝到 “%JRE_HOME%\lib\security” 和 “%JDK_HOME%\jre\lib\security” 目录下覆盖原有文件。
</div>

**临时 Token 获取方式**

为方便开发者体验使用 Token 鉴权，[ZEGO 控制台\|_blank](https://console.zego.im/) 提供生成临时 Token 的功能，开发者可直接获取临时 Token 来使用，详情请参考 [控制台（新版） - 项目管理\|_blank](12107) 中的 “项目信息”。但是在开发者自己的线上环境中，一定要通过自己的服务端生成 Token。














