### FastCGI 通信


### Client 请求数据 

> 分为三部分 BeginRequest, Params, STDIN, 依次传输。
> 每一部分又分为 Header Contant 两段, Header 用于标识当前数据的类型和内容的长度, Contant 是当前部分的传输的内容

#### 每一部分 Header 段的长度为固定的 8 的字节
1. FastCGI 的版本号
2. 这一部分的类型
3. RequestID 头 8 位
4. RequestID 后 8 位
5. 这一部分的内容长度 前 8 位
6. 这一部分的内容长度 后 8 位
7. 补位字节，用于对齐
8. 暂时留空

#### 第一部分 BeginRequest

第二部分 Params

空
第三部分 STDIN
空


Server
