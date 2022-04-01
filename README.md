# Shortener Url
## Require
* docker
* docker-compose
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
首先
