#### Web Workers
* 技术背景

  单线程的大量计算不仅会阻塞 UI 的渲染

  还无法充分发挥多核 CPU 的计算能力

  甚至会造成浏览器的卡死

* 技术弊端

  1、同源策略的限制

      分配给 Worker 线程运行的脚本文件，必须与主线程的脚本文件同源

  2、操作 DOM 的限制

      无法读取主线程所在网页的 DOM 对象，也无法使用 window 和 document 对象，但可以使用 window 对象的其他子对象

  3、本地文件的限制

      Worker 线程无法读取本地文件，因此它所加载的脚本必须来自于网络

  4、消息通信

      Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息的发送和接收来通信

* 共有特性

  1、设置线程名称，便于调试

  2、工作线程有独立的上下文环境，可以通过 self 代表子线程自身，即子线程全局对象

  3、引入脚本库


#### 专用Worker
* 主线程

  1、生成一个专用worker
  ```JS
  const worker = new Worker('md5.worker.js', { name: 'hash_worker' })
  ```
  专用 worker 仅能被首次生成它的脚本使用

  2、消息的发送和接收
  ```JS
  worker.postMessage()
  worker.addEventListener('message', ({ data }) => {
    // event.data <=> data
  })
  ```

  3、终止主线程
  ```JS
  worker.terminate()
  ```
  立即终止，并不会等待工作线程去完成它剩余的操作

* 工作线程

  1、消息的接收和发送
  ```JS
  self.addEventListener('message', ({ data }) => {
    // event.data <=> data
  })
  self.postMessage()
  ```

  2、关闭当前工作线程
  ```JS
  self.close()
  ```

  3、引入脚本与库
  ```JS
  importScripts()
  ```
  说明：同步执行，所引入的脚本与库都会绑定在子线程的全局对象上

* 应用场景 - 计算文件的MD5值
  ```JS
  importScripts(`${location.origin}/static/worker/index.umd.js`)
  const bmf = new self.browserMD5File()
  const getFileMD5 = function(filesInfo) {
    for (const item of filesInfo) {
       bmf.md5(
        item.file,
        (err, md5) => {
          item.metamd5 = md5
          self.postMessage(item)
        },
        progress => {
          console.log('MD5 progress number:', progress)
        }
      )
    }
  }
  self.addEventListener('message', event => getFileMD5(event.data))
  ```

#### 共享Worker
* 主线程

  1、生成一个共享worker
  ```JS
  const sharedWorker = new SharedWorker('fetch.worker.js', { name: 'fetch_worker' })
  ```
  共享 worker 可以被多个上下文共同使用

  2、消息的发送和接收
  ```JS
  const worker = sharedWorker.port
  worker.postMessage()
  worker.onmessage = ({ data }) => {
    // event.data <=> data
  }
  ```

* 工作线程

  1、消息的接收和发送
  ```JS
  self.addEventListener('connect', ({ ports }) => {
    const PORT = ports[0]
    PORT.onmessage = ({ data }) => {
      PORT.postMessage(data)
    }
  })
  ```

  2、显式的打开端口连接

      使用端口对象的 onmessage事件处理函数 或 start()方法

* 如何调试

  1、通过 chrome://inspect 打开浏览器新标签页

  2、选择左侧 Shared workers 选项，然后选择相应的脚本，点击 inspect 进行调试

* 注意事项

  1、在传递消息之前，必须显式的打开端口连接

  2、使用 start() 方法打开端口连接时，如果主线程和 worker 线程需要双向通信，那么它们都需要调用 start() 方法
