如果今天有一個實際需要，必須在image中放入一些機敏資訊，但使用場景又不能連網，或者不能讓image有資安洩漏的風險，最常使用的Dockerfile ENV是被禁止使用的

舉例來說，在撰寫Dockerfile時，使用ENV語法
```
ENV KEY <VALUE>
```

這樣的作法，可以在後續的container中在環境變數中取得該機敏資訊，
但這些資訊，仍然可以透過docker history取得，或者在container中透過export語法得知
因此是相當不安全的，但同時又存在放入機敏訊息的需要

因此動態插入機敏資訊顯得重要


