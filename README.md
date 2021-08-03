# [Do it!](https://doitouob.com)

### 設計理念

![Doit設計理念](https://user-images.githubusercontent.com/60848391/127813772-9b303588-0d36-43ea-a6ca-c3aba4034396.png)

老闆交代工作，員工提交進度，看似簡單，但人一多或交代的事情一多就會有些問題產生，例如
1. 老闆忘記自己交代了哪些工作，或是員工忘記自己被交代了哪些工作
2. 員工提交進度，老闆沒有查看反而懷疑員工偷雞摸狗

為了解決這個問題而設計出了Do it!這個平台，讓雙方都可以掌握彼此的進度，讓溝通更順利。

### 使用技術

* Python Flask
* 以RESTful API架構實現
* 串接FB API實現FB第三方登入
* SMTP進行email 邀請加入團隊
* 使用Redis存取通知，確認完成後可以將通知刪除
* 以S3儲存使用者上傳的大頭貼
* 申請SSL憑證實踐HTTPS

### 系統架構圖

![系統架構圖](https://user-images.githubusercontent.com/60848391/127816439-e55aa085-b09e-4c95-b8c8-0d23f7569c24.png)

### MySQL架構圖
![mysql架構圖2](https://user-images.githubusercontent.com/60848391/127818729-4eacc077-5ca3-4f37-81af-0d44022a916c.png)

## 功能介紹

### 首頁進行帳號登入及註冊
![首頁](https://user-images.githubusercontent.com/60848391/127956269-1218844f-1466-41b0-b6fd-750ff32d4156.png)

首頁可進行登入及註冊，目前支援FB實現第三方登入。
註冊會進行檢查
* email需輸入合法email地址(確保收的到信)
* 使用者名稱會檢查是否有重複
* 密碼需輸入英文大小寫及數字共8位進行註冊

### 團隊總覽
![團隊總攬](https://user-images.githubusercontent.com/60848391/127984859-eafae573-1d6c-4154-9478-232fba83b58e.png)

#### 1. 可查看個人資料
#### 2. Leading team為您所帶領的團隊，點擊Add Team可以建立團隊(綠色block)，點擊綠色block可以進入工作頁面，可以邀請其他人加入您的團隊，並且安排工作給團隊內的工作者。
#### 3. Working team為您所加入的團隊，點擊綠色block可以進入工作頁面，可以查看其團隊領導者指派給您的工作。

* 點擊團隊的畫面即會進入團隊內工作頁面。

### 工作頁面

![工作區塊領導首頁](https://user-images.githubusercontent.com/60848391/127992532-95bb39d3-56a9-47da-80a0-510fa31355d1.png)

#### 1. 列出所有的團隊，包含您領導的，以及你所加入的團隊
#### 2. 團隊名稱、工作狀態篩選、邀請別人加入團隊、分派工作 (先以你所帶領的團隊進行說明)

Do it!平台將工作狀態分成幾種
1. Working : 剛指派完工作的預設狀態，會需要被指派到該工作者進行 __Submit(提交)__ 動作。
2. Pending : 工作被 __Submit(提交)__ 之後，會進入此狀態，此狀態下團隊的領導者會進行審核，審核通過會進入 __Finish(完成)__ 狀態，不通過則會再變成 __Working(工作)__ 狀態。
3. Finish : 審核通過的工作，可以保留或者是進行 __Delete(刪除)__ 動作。

Search Box 及旁邊的下拉式選單可以藉由 __工作狀態__ 及 __工作者__ 進行篩選。

(2a、2b.) 可以透過 Invite 及 Assign 按鈕進行邀請別人加入團隊及分派工作
![分派及邀請](https://user-images.githubusercontent.com/60848391/127992957-206087f6-fe5b-4fd1-a99f-c3c711a90f84.png)

#### 3. 通知系統

當團隊內部的工作者將其中某項工作進行 __Submit(提交)__ 之後或是工作者在某項工作內部進行留言(留言方式於下方說明)，會出現在右側的通知欄，通知查看完畢後可點擊 __垃圾桶__ 圖案將通知刪除。

#### 4. 工作區塊

列出目前團隊內部所有人的所有工作，不同的工作狀態會以不同的顏色呈現。點擊該工作可以查看其工作細項。

#### 5. 工作細項

![細項](https://user-images.githubusercontent.com/60848391/127996736-1d657bc8-89f4-47b0-bf1a-49bf2ec4f823.png)

* 1. 此工作的工作者
* 2. 此工作之工作細項
* 3. 工作者留言區，點comment可進行回覆
* 4. 工作審核區，依據該工作有不同的工作狀態可進行不同的操作
 工作狀態  | 可進行之相應操作
 ---- | -----   
 Working  |  Leave(離開細項畫面)
 Pending  |  Done(工作完成，轉為Finish狀態) 、 Keep Working(審核未通過，轉為Working狀態)
 Finish  |  Delete(將工作刪除)


