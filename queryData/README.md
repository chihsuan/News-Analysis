操作說明
=============
前置 : 
`git clone git@github.com:brandboat/News-Analysis.git`
`cd News-Analysis/queryData`

1. 建立server
`python -m SimpleHTTPServer 8080`

2. 在瀏覽器輸入網址 : http://localhost:8080

程式碼說明 : 

`resultList` : 為一二維陣列，格式如下
resultList[i][0] ~ resultList[i][3] 共4筆資料，分別是此四筆
getMorePost("2014-04-15", "2014-05-03", friendsList, numOfFriend, 0);
getMorePost("2014-04-01", "2014-04-15", friendsList, numOfFriend, 1);
getMorePost("2014-03-15", "2014-03-31", friendsList, numOfFriend, 2);
getMorePost("2014-03-01", "2014-03-14", friendsList, numOfFriend, 3);
其中i代表其中一位使用者，若您有560位朋友，此陣列就是560 * 4

`resultCheck` : 為一二維boolean陣列，格式同上，由於FB.api是asychronous，為了確認每一個人query都有完成，用以整合resultList[i][0] - resultList[i][3]的資料。

`queryTimes` : 每一個人query的次數，queryCommand()裏頭有4條query，所以為4

function : 

`Login` : FB login，scope 中擷取權限

`getUserInfo` : 取得使用者資料FB.api('/me'),取得使用者朋友資料FB.api('/me/firends')->取得friendsList 

`getFriendsInfo(friendsList)` : 在這邊可以去修改要query的好友個數，因為FB有query的次數限制。
EX.numOfFriend=0 queryFriendsLimit=10 ，取得好友第0 - 10位

`queryCommand` : query 指令，目前只有取得3月到5月的資料，為何不是一口氣改為一條指令呢?因為FB api很怪，他每次回傳最多給我約20條資料，我不得已採用分段的方式，若要新增指令，記得全域變數`queryTimes`亦要改變。

`getMorePost` : 將資料餵給resultCheck。

`getResult` : 要取得使用者好友那些資料，目前是Message，Description，Story這幾項，當然不只這些，在瀏覽器中按F12點開console中的log訊息可以看到每一時間的response，點擊後就可以知道有哪些訊息可以供parse，若要新增parse項目，在此function修改即可

`checkAllDone` : 確認單一個使用者朋友的query項目都已做完，再做訊息整合，並生成檔案。

整體來說，流程大致是這樣 -> 
Login -> getUserInfo -> getFriendsInfo -> queryCommand -> getMorePost -> getResult -> checkAllDone

要注意的是因為式asychronous，事實上每一個query都會是在背景運作，在console中就可以觀察到。
