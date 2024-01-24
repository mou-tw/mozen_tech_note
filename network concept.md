IP 地址可想像成收件人
MAC address可想像成收件地址，在網路中是唯一的
MAC address由六個字節組成
>例如 XX-XX-XX-XX-XX-XX
>前三碼為廠商號碼
>後三碼為網卡序列號


透過網路發送訊息給目標用戶，基礎資訊包含IP和要傳送的內容，而mac address會由OS查詢ARP表補齊，並從自身的設備上網路卡發送而出

而設備發送網路訊息時，會透過switch上分析以及轉送訊息，如果是網路卡連接網路線插上switch的port，switch會將當前的port與mac addres綁定

IPV4的IP已經在2011年被完全使用完畢

網卡上分配IP位置