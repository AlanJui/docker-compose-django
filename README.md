# 專案指引

建立 Django 專案模版，示範如何運用 docker 及 docker compose 建置「系統開發環境」；並說明應有之「作業流程」及「程序操作」。

此專案模版之 Django App ，係利用 GitHub 現成之 Django REST API 應用系統，學習 Docker 與 Docker Compose 在 SDLC 之應用與操作程序。

# 系統開發作業

## （一）建置開發環境設定檔程序

### 1. 進入專案根目錄

```commandline
cd byob-profiles-rest-api
```

### 2. 建立 Docker 程序設定檔

建立檔案：Dockerfile ，並輸入以下之內容：
```buildoutcfg
ROM python:3

ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
COPY . /code/
RUN pip install -r requirements.txt
```

當執行 docker build 指令時， Docker Engine 將依據 Dockerfile 檔案內容之描述，完成 Docker Image 檔案之建置。

已完成建置的 Docker Image 檔案，可用 docker images 指令查詢，藉以確認建置工作的確如預期完成。

### 3. 建立 Docker Compose 程序設定檔

建立檔案：docker-compose.yml ，並輸入以下之內容：
```buildoutcfg
version: '3'

services:
  web:
    build: .
    command: python src/profiles_project/manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
```

## （二）初始資料庫程序

### 執行 migrate 指令，令 Django Framework ，透過 Model 物件之設計，對資料庫進行初始之建置工作。

```commandline
docker-compose run web python src/profiles_project/manage.py migrate
```

## （三）執行開發中應用系統程序

### 1. 執行啟動應用系統指令

```commandline
docker-compose up
```

### 2. 執行 Web 瀏覽器

### 3. 在網址列輸入 URL 位址

```commandline
http://localhost:8000/api
```

### 4. 在 Web 瀏覽器觀察輸出結果。



---

# 參考

參考網路教學文章：[Profiles REST API](https://github.com/LondonAppDeveloper/byob-profiles-rest-api) -- REST API providing basic functionality for managing user profiles.
