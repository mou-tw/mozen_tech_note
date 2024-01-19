overlay2本身是一種聯合檔案系統(Union File System)的檔案系統(file system)
會將好幾個folder整合成一個folder對外使用


lowerdir可以有多個，可理解為docker image的layer dir
也可以形象為火鍋的湯底，是每次創建overlay FS時候，必須存在的物件
upper, work, merge都只會有一個
merge是對外服務用的dir
當在merged資料夾中

當lower和upper dir中都有相同名稱文件，會在merged dir中顯示upper的文件
而lower一定是read only

如果真對merged中的文件做修改，而該文件不存在upper中，只存在於lower時，會觸發whiteout的機制，即從lower複製一份到upper中做修改


![[Pasted image 20240118231137.png]]

create overlay2 by mount CLI
[[mount#create overlay2 mount]]
