Unix/ Linux 精神為"萬物皆文件"

window中的process, disk, socket等非文件類的東西，在linux OS中都被"抽象"成文件

好處是所有的操作都透過對文件的讀/寫完成，也構成Infrastructure as code 的基礎

壞處是過度的抽象有時難以理解，且使用任何硬體設備都必須要在根目錄(root dir)下執行掛載(mount)，可以想像成硬體設備本身也是一個樹結構的目錄，要把這個樹結構的目錄放到root dir底下，這就是mount的精神所在。

而root dir類似"掛載"到其他的root dir就是containerize的精神所在。