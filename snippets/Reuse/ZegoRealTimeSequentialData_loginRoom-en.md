在开始传输实时有序数据之前，开发者需要调用 [loginRoom\|_blank](@loginRoom) 接口，登录房间。

对于数据的接收端，建议开发者通过 [onRoomStreamUpdate\|_blank](@onRoomStreamUpdate) 回调接口，监听房间的流变化。当房间内其他用户新增或删除流的时候，可以去订阅对应流 ID 的相关信息。

