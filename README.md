# Shortener Url
## Require
* docker
* docker-compose
* golang
## Usage
### Setup
1. 進到 repo 並將backend pull下來
```
git clone git@github.com:fourcolor/backend.git
```
2. 需要的 Database Application 已經打包成 docker-compose
```
docker-compose up -d --build
```
3. 開啟 backend
```
$ cd backend/
$ sudo go run src/app.go
```
### Backend API
* POST http://localhost:8080/api/v1/urls 需要欄位為 url 以及 expireAt 會回傳 ```{ "id": originUrl, "shortUrl": short }```

e.g
```
#POST
$ curl -X POST -H "Content-Type:application/json" http://localhost:8080/api/ v1/urls -d '{"url": "https://google.com","expireAt": "2021-02-08T09:20:41Z"}'

#結果
{
    "id": "https://google.com",
    "shortUrl": "AQAAAAAAAAA="
}
```
* GET http://localhost:8080/{short} 會檢查 short 是否合法，若合法則 redirect ，否則回傳404
e.g
```
# GET
$ curl -X GET -H "Content-Type:application/json" http://localhost:8080/AQAAAAAAAAA=

#結果
<a href="https://google.com">Found</a>.
```
## 實做過程
* 使用語言: Golang
* 使用資料庫: Mysql, Redis
這次總共要設計兩個API，一個是用來產生短網址，一個是用來產生短網址，一個用來做短網址的重新導向。
1. 產生短網址
此API會先檢查使用者傳入的原始網址是否是真的，接著再檢查是否有存過相同的短網址(要url和expiredAt同時一樣)，接著才會開始產生短網址。而產生短網址的方式為利用一個計數器(存在redis的64 bits整數)分成 8 個字元，再透過Base64碼產生短網址。
2. 重新導向
會檢查redis或mysql是否存有該短網址，若是友才會重新導向，若沒有或是過期則會回傳 404 Not Found。
