IP 地址可想像成收件人
MAC address可想像成收件地址，在網路中是唯一的
MAC address由六個字節bytes(48位元bits)組成
>例如 XX-XX-XX-XX-XX-XX
>前三碼為廠商號碼
>後三碼為網卡序列號

mac 的廠商可在[網站](https://macvendors.com/)查詢

mac address是使用16進位方式記錄，因此需要使用4個bits紀錄一個數字，因此總共會有12個數字
表現的方式有所不同
>微軟表示法(使用-)
>01-01-55-55-55-5e
>其他
>11:33:33:33:33:3e
>網路設備作業系統的表示法(Cisco, IOS)
>0123.456.786e

MAC的特殊位元
第七個位元0表示全球唯一
第七個位元1表示本地自行管理
第八個位元0表示單播
第八個位元1表示群播 




透過網路發送訊息給目標用戶，基礎資訊包含IP和要傳送的內容，而mac address會由OS查詢ARP表補齊，並從自身的設備上網路卡發送而出

而設備發送網路訊息時，會透過switch上分析以及轉送訊息，如果是網路卡連接網路線插上switch的port，switch會將當前的port與mac addres綁定

IPV4的IP已經在2011年被完全使用完畢

DHCP
在日常使用中，手機連上wifi或者電腦插上網路線能上網，很多時候是透過DHCP機制，由操作系統向外發出一個DHCP request，router獲取到request會自動分配一個地址，透過DHCP message回傳地址，將IP綁定到網路卡上，並且這個IP是局域網內唯一的

ARP協議
當操作系統要發送一個包含IP和內容的訊息前，會先發一個ARP message，詢問該地址所對應的mac address是誰，而這個過程所有網路上的設備都收到了該訊息，除了正確的IP所對應的系統，都會將這個詢問message丟棄，只有對應的IP設備會將自己的mac address發送回去，收到mac address的的機器會cache這個mac address和IP的對應後，再將需要傳送的訊息發出


## Bus 
bus 為匯流排，指各個網路部件傳送訊息的公共通路
Hub 在傳遞資料的方式為將全部收到的資料都送出，所有連接在hub上的設備都會收到資料，這種特性稱為multiple access
舉例而言，當hub內的PC1 ping PC2時，hub上連接的所有電腦都會收到訊息