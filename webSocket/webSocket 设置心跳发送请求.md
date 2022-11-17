## websocket 发送请求

### 引入包第三方包

```
import {useWebSocket} from '@vueuse/core'
```

使用token，引入token

```
import {getAccessToken} from "@/utils/auth";
```

### 使用

```
const test = {
    tsSubCmds: [
      {
        entityType: 'DEVICE',
        // entityId: route.query.id,
        entityId: 'd4697f60-55a2-11ed-bab5-cf7feadd0b1e',
        scope: 'LATEST_TELEMETRY',
        cmdId: 1
      }
    ]
  }

  const { send, close } = useWebSocket(
    `${import.meta.env.VITE_TB_WEBSOCKER_URL}/tb/api/ws/plugins/telemetry?access_token=${getAccessToken()}`,
    {
      autoReconnect: {
        retries: 3
      },
      heartbeat: {
        message: JSON.stringify(test),
        interval: 3000
      },
      onConnected(ws) {
        const query = {
          tsSubCmds: [
            {
              entityType: 'DEVICE',
              // entityId: route.query.id,
              entityId: 'd4697f60-55a2-11ed-bab5-cf7feadd0b1e',
              scope: 'LATEST_TELEMETRY',
              cmdId: 1
            }
          ]
        }
        send(JSON.stringify(query))
      },
      onMessage(ws, event) {
        const data = todoMessage(JSON.parse(event.data).data)
        // 把data赋值给当前页面
        console.log(data)
      },
      onError(ws, event) {
        console.log('error', event)
      }
    }
  )
```

1. entityId 是设备id 当前只需要修改这一个。记得把这个改为动态的
2. heartbeat 设置心跳时间每次会自动发送信息
   1. **message 发送请求的参数**
   2. interval 心跳的时间

### 连接的数据不直观，创建一个转换数据的方法

```
const todoMessage = (obj: any[]) => {
  const arr: any = []
  // eslint-disable-next-line guard-for-in,no-restricted-syntax
  for (const i in obj) {
    const a = {
      key: '',
      value: '',
      lastTime: ''
    }
    a.key = i
    a.value = obj[i][0][1] || ''
    a.lastTime = obj[i][0][0] || ''
    arr.push(a)
  }
  return arr
}
```

### webSocket获取的结果

![image-20221117111256306](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20221117111256306.png)



