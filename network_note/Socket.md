兩個process之間的溝通，除了需要透過port之外，還需要有socket的存在。
當process需要傳送訊息給另一個訊息時，需要註明三個資訊
- protocol 協議: TCP, UDP, IP等
- ip address
- port
socket 和port在某些語境底下是相同的意思，但本質上是不同的概念，因process之間需要溝通時，需要開一個port並綁定IP

查看Linux socket資訊
```
ss

```