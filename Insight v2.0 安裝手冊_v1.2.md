# Insight v2.0 安裝手冊

[TOC]



## 第 1 章 環境架構圖

1.1 系統環境說明

1.2 系統安裝需求

1.2.1 Server 1

1. CentOS 64位元 1.804
2. Java 8 SDK

1.2.2 Server 2

1. Ubuntu 16.04.5

2. Python 2.7版本以上(3.X版本以下)

3. 環境規劃(安裝前請務必詳讀)

   Server | 需確認物件|預設值|用意|須調整位置
   ---- | --- | ---- |---- |---- 
   Server1\|Server2 | 是否可對外連線 | 開啟   |安裝套件時需有此功能|找客戶 IT詢問
   Server1 | Solr Port | 8983   |確認 Solr 對外 Port 是否開通|找客戶 IT詢問
   Server1 | JAVA版本 | 8      |Solr 需搭配 JAVA 8 SDK 以上版本，目前只實測8版|如以有安裝則安裝新的時，詢問是否可將版本更至同一版本
   Server1 | IP | 無 |使Server 2 能夠連接(於Nginx設定)|修改Server 2 Nginx設定
   Server2 | http Port | 開啟   |檢查 80 Port 是否遭占用|找客戶 IT詢問
   Server2 | Mysql版本 | 5.7.24 |MySQL 根據版本不同，資料存放限制也不同|如以有安裝則會出問題，詢問是否可將版本更至同一版本
   Server2 | Insight Port | 12221  |確認 Insight 對外Port是否開通|找客戶 IT詢問
   Server2 | 圖表平台 Port | 8080   |確認 圖表平台 對內Port是否開通|修改Nginx設定
   
   
   
   
   
   
   
   
   
   
   

## 第 2 章 Insight V2.0 安裝程序

### 2.1 平台系統安裝流程 

#### 2.1.1 建立於Server 1 中的系統環境

1. 系統環境建立
2. Yum套件更新
3. Python套件安裝
4. 安裝Zip與UNZip套件
5. 準備安裝所需程式
6. Java 8 SDK安裝
7. 防火牆設定
8. Solr服務啟動
9. Solr運行狀態檢視
10. Solr自動匯入工具啟動

#### 2.1.2 建立於Server 2 中的系統環境

1. 系統環境建至
2. apt套件更新
3. 安裝openssh-server套件
4. 安裝Zip與UNZip套件
5. 準備安裝相關程式
6. 圖表系統安裝
7. MySQL設定檔修改
8. Insight平台系統安裝
9. Nginx套件安裝
10. 系統執行狀態檢視

### 2.2 安裝流程細部說明

#### 2.2.1 基本定義

為說明而假設幾個資料路徑，實際安裝時請將路徑依照實際狀態進行更換

#### ●Server1

1. 壓縮檔放置位置

   ```
   /home/temp
   ```

2. Solr安裝路徑

   ```
   /home/Solr
   ```

3. 自動匯入程式安裝路徑

   ```
   /home/SolrInsert
   ```

#### ●Server2

1. 壓縮檔放置位置

   ```
   /home/temp
   ```

2. 圖表系統安裝路徑

   ```
   /home/ivp2017
   ```

3. Insight平台安裝路徑

   ```
   /home/Insight
   ```

4. MySQL設定檔路徑

   ```
   /etc/mysql
   ```

5. Nginx設定檔路徑

   ```
   /etc/nginx
   ```

#### 2.2.2 Server 1 中的系統環境細部安裝說明

#### ●系統環境建置

Step 1. 安裝 CentOS 系統環境

如果是 VM 環境請掛載系統安裝程式 ISO 檔，如果是實體主機請放入系統安裝程式光碟 

Step 2. 更新 Yum 套件

```
yum update -y
```

Step 3. 安裝Zip/unZip套件

``` 
yum install zip -y
yum install unzip -y
```

Step 4. 安裝Python 套件

```
yum -y install epel-release
yum install python-pip -y
pip install requests
```

Step 5. 安裝Java 8 SDK 

```
yum install java-1.8.0-openjdk-devel
```

#### ●準備安裝所需程式

Step 1. 解壓縮Solr安裝包

```
unzip Solr.zip
```

Step 2. 移動解壓縮檔案到Solr路徑

```
mv Solr /home
```

Step 3. 解壓縮Solr自動匯入工具安裝包

```
unzip SolrInsert
```

Step 4.移動解壓縮檔案到自動匯入程式安裝路徑

```
mv SolrInsert /home
```

#### ●防火牆設定

Step 1. 檢查 Solr Port 於防火牆是否開啟

```
firewall-cmd --zone=public --list-all
```

Step 2. 開起 Solr Port

```
firewall-cmd --zone=public --add-port=8983/tcp --permanent
firewall-cmd --reload
```

#### ●Solr服務啟動

Step 1. 進入 Solr 啟動目錄

```
cd /home/Solr/bin
```

Step 2. 啟動 Solr 服務

```
#Solr開啟預設為 8983 Port
Solr start
#如要以其他Port號開啟
Solr start -port Port
```

Step 3. 驗證 Solr 服務是否正確啟動

#### ●Solr自動匯入工具啟動

Step 1. 進入自動匯入工具目錄

```
cd /home/SolrInsert
```

Step 2. 啟動自動匯入工具

```
python MongoJsonToSolr.py
```

#### 2.2.3 Server 2 中的系統環境細部安裝說明

#### ●系統環境建置

Step 1. 安裝 Ubuntu 系統環境

如果是 VM 環境請掛載系統安裝程式 ISO 檔，如果是實體主機請放入系統安裝程式光碟 

Step 2. 更新Apt 套件

```
apt update -y
```

Step 3. 安裝 openssh-server 套件(#此步驟為使系統能使用ssh連接，如不需要則可不安裝)

```
apt install openssh-server -y
```

Step 4. 安裝 Zip 套件

```
apt install zip -y
```

Step 5. 安裝 Nginx 套件

```
apt install nginx
```

#### ●準備安裝所需程式

Step 1. 解壓縮圖表系統 tar

```
tar zxf insight_20181230.tar.gz
```

Step 2. 解壓縮 Insight平台系統 Zip

```
unzip Insight-linux-1.0.0.0-20181230-214024.zip
```

Step 3. 解壓縮 other.zip

```
unzip other.zip
```

#### ●圖表系統安裝

Step 1. 進入解壓縮後的圖表系統安裝目錄

```
cd /Insight
```

Step 2. 修改設定檔( install_config.py )

```
vim install_config.py
```

Step 3. 執行安裝程式

```
bash install.sh
```

#### ●MySQL設定檔修改

Step 1. 進入MySQL設定檔目錄

```
cd /etc/mysql
```

Step 2. 修改設定檔

```
#可依照第4章節的說明進行修改，或直接將該目錄的mysqld.cnf以other解壓縮後的資料夾中的mysqld.cnf做覆蓋。
```

Step 3. 重新讀取 MySQL 服務

```
service mysql reload
```

#### ●Insight 平台系統安裝

Step 1. 預先建立 Insight 平台系統須放置的位置目錄

```
mkdir /home/Insight
```

Step 2. 解壓縮 Insight 壓縮檔後的 Insight 平台系統安裝目錄

```
unzip Insight-linux-1.0.0.0-20181230-214024.zip
cd Insight-linux-1.0.0.0-20181230-214024
```

Step 3. 執行安裝程式

```
extractcmd.sh -path /home/Insight
```

Step 4. 進入安裝後 Insight 平台系統目錄

```
cd /home/Insight
```

Step 5. 進入 Insight 平台系統設定檔目錄

```
cd /home/Insight/apache-tomcat/extension/Insight/config
```

Step 6. 修改 Insight 平台系統設定檔

```
#可依照第4章節的說明進行修改，或直接將該目錄的datasource.xml以other解壓縮後的資料夾中的datasource.xml做覆蓋。
```

Step 7.  進入 Insight 平台系統 tool 目錄

```
cd /home/Insight/tool
```

Step 8. 進行資料庫初始化動作

```
./dsinitcmd.sh -ds default
```

Step 9. 執行 Insight 平台系統

```
./server.sh
```

#### ●Nginx設定檔修改

Step 1. 進入 Nginx 設定檔目錄

```
cd /etc/nginx
```

Step 2. 修改 Nginx 設定檔

```
#可依照第4章節的說明進行修改
#主要修改為
location /solr{
    proxy_pass http://192.168.1.1:8983; #請將此位置修改為Server1 的Solr IP及Port號
    proxy_http_version 1.1;
}
```

Step 3. 重新讀取Nginx服務

```
service nginx reload
```

#### ●系統執行狀況檢視

Step 1. 網頁連接檢視系統是否正常

## 第 3 章 Solr 自動匯入資料夾說明

Solr自動匯入工具的資料夾有部分預先寫好的邏輯，再不進行設定檔的調整前將按照預設的方式建立資料夾，因此設定Ftp檔案上傳前須先了解自動產生的資料夾意義。

### 3.1 MongoJson

放置需匯入至Solr的Json檔案，當Json檔案進入時程序會自動抓取該路徑下的檔案做匯入動作。

### 3.2 SaveJson

放置匯入Solr完成的後Json，以供後續備查使用。

### 3.3 supplementJson

放置需匯入至Solr的Json檔案，當Json檔案進入時程序會自動抓取該路徑下的檔案做匯入動作，此資料夾為備用入口，如需臨時匯入但又不想影響到原先匯入狀況可將檔案放置於此。

### 3.4 errorJsonContent

Json匯入至Solr時發生錯誤的部分，自動匯入工具發生錯誤時會將該筆做獨立寫入於此，以便後續做問題的排除。

## 第 4 章 各項設定及調整說明

### 4.1 Solr 自動匯入設定

#### 4.1.1 install_config.py

*  ivp2017_src_path

  圖表系統安裝包路徑，結尾的ivp2017資料夾路徑需保留(目前已用邏輯方式處理，請勿更動此行)

* ivp2017_dest_path

  圖表系統安裝後路徑位置，該路徑資料夾須不存在，如指定已存在資料夾將會出現錯誤。

* mysql_host

  MySQL 的 IP 位置，使用 localhost 即可請勿更動。

* mysql_root_password

  MySQL 的 root 密碼，請勿更動。

### 4.2 Insight平台系統設定

可查看Other.zip解壓縮後的datasource.xml作為實際使用或範例

#### 4.2.1 datasource.xml

* id

  該連線資訊於 Insight 平台系統中所命名的名稱。

* driver-class

  該連線資訊於 Insight 平台系統中所使用的 Java 資料庫連接套件。

* url

  該連線資訊於 Insight 平台系統中所使用的部分資訊(包含Host 及 DatabaseName)。

* user

  資料庫連接時所使用的帳號。

* password

  資料庫連接時所使用的密碼。

### 4.3 MySQL 設定

#### 4.3.1 mysqld.cnf

可查看Other.zip解壓縮後的datasource.xml作為實際使用或範例

1. [client]

   *  default-character-set

     MySQL 連線編碼設定，因 Insight 平台系統需使用 utf8 因此在此設定為 utf8 編碼格式。

2. [mysqld]

   * bind-address

     MySQL限制 IP 連接部分，預設應為 127.0.0.1 也就是只有本機可連線，如果註解或者更改成0.0.0.0則任意主機皆可連線，如要限制連線可輸入限制的主機 IP ，就會只有該台主機可以連線，但不可設定 2 台以上。

   * character-set-server

     MySQL 主機編碼設定，因 Insight 平台系統需使用 utf8 因此在此設定為 utf8 編碼格式。

   * collation-server

     MySQL 主機編碼設定，因 Insight 平台系統需使用 utf8_general_ci 因此在此設定為 utf8_general_ci 編碼格式。

### 4.4 Nginx設定

#### 4.4.1 nginx.conf

```
server{
        listen 80;
	    server_name test;
	    location /ivp2017{
	        proxy_pass http://127.0.0.1:8080;
            proxy_read_timeout 1000s;
	    }
	    location /Insight{
		    proxy_pass http://127.0.0.1:12221;
		    proxy_http_version 1.1;
	    }
        location /solr{
            proxy_pass http://192.168.1.1:8983; #請將此位置修改為Server1 的Solr IP及Port號
            proxy_http_version 1.1;
        }
        location /phpmyadmin{
            proxy_pass http://127.0.0.1:8080;
        }
	}
```

## 第 5章 帳號權限設置

Insight的帳號與圖表帳號是兩個不同的權限設置，

因此會以帳號所屬的部門負責人為主要管理者。

(以下說明已 Toyota 為例)

##### 5.1 帳號建立說明

這邊將會建立兩個帳號，管理者及使用者，管理者為我方檢視使用，使用者為客戶所使用。

(如果以建立過該客戶部門，將略過4、7、8步驟)

1. 於Insight平台介面點選系統管理頁籤

2. 於系統管理頁籤上點選員工

3. 於員工頁面上點選新增

4. 建立該客戶管理者帳號(帳號 ToyotaAdmin 密碼 Toyota)

5. 建立該客戶使用者帳號(帳號 Toyota 密碼 Toyota)

6. 於系統管理頁籤上點選部門

7. 於部門頁面上點選新增

8. 新增一個該客戶的部門並將管理者設定為部門負責人

9. 將使用者加入該部門

###### 5.1.1權限設定說明

​     帳號權限將區分為 4 類 系統管理者、專案開發、專案設計、圖表展示

- 系統管理者

  此權限為部門負責人將不會開給客戶，設置方式為：

  1. 於Insight平台介面點選系統管理頁籤

  2. 於系統管理頁籤上點選員工

  3. 點選欲修改權限員工至選項並雙點擊(或右上角打開)開啟

  4. 點選該員工資訊左側的角色選項

  5. 於角色頁面上點選新增

  6. 點選系統管理者權限並雙點擊(或按右上角確定)加入權限

     

- 專案開發
  此權限為客戶的最高權限，設置方式為：

  1. 於Insight平台介面點選系統管理頁籤

  2. 於系統管理頁籤上點選員工

  3. 點選欲修改權限員工至選項並雙點擊(或右上角打開)開啟

  4. 點選該員工資訊左側的角色選項

  5. 於角色頁面上點選新增

  6. 點選專案開發權限並雙點擊(或按右上角確定)加入權限

     
- 專案設計

  此權限客戶的次要權限，設置方式為：

  1. 於Insight平台介面點選系統管理頁籤

  2. 於系統管理頁籤上點選員工

  3. 點選欲修改權限員工至選項並雙點擊(或右上角打開)開啟

  4. 點選該員工資訊左側的角色選項

  5. 於角色頁面上點選新增

  6. 點選專案設計權限並雙點擊(或按右上角確定)加入權限

     

- 圖表展示
  此權限為預設權限，不須而外設定。

## 第 6章 各項服務啟動說明

### 6.1 Solr 啟動/關閉說明

#### 6.1.1 啟動說明

```Solr start 
#於 Solr 執行目錄輸入，Solr即會啟動 
Solr start
#啟動後預設為 8983 Port，如需更增加輸入參數
Solr start -prot Port
```

#### 6.1.2 關閉說明

```插入
#於 Solr 執行目錄輸入，Solr即會關閉
#需輸入Port，Solr會依照所要關閉的Port進行關閉。
Solr stop -port Port
```

### 6.2 Solr 自動匯入啟動/關閉說明

#### 6.2.1 啟動說明

```
#於 Solr自動匯入工具目錄輸入，自動匯入工具即會啟動。
python MongoJsonToSolr.py
#如遇到其他因素需開起備用匯入，則可於 Solr自動匯入工具目錄中再一次執行指令，並帶入備用輸入指令。
python MongoJsonToSolr.py -status 1
```

#### 6.2.2 關閉說明

### 6.3 MySQL啟動/關閉說明

#### 6.3.1 啟動說明

```
#MySQL啟動使用的為Linux自帶的Service服務，因此以該服務執行方式執行即可。
service mysql start
#如要查看開啟狀況可輸入
service mysql status
```

#### 6.3.2 關閉說明

```
#MySQL啟動使用的為Linux自帶的Service服務，因此以該服務執行方式執行即可。
service mysql stop
```

### 6.4 圖表系統啟動/關閉說明

#### 6.4.1 啟動說明

```
#圖表系統是基於PHP系統架構，並且掛載於Apache2的服務上，
#因此如要啟動只需要啟動Apache2服務即可
service apache2 start
```

#### 6.4.2 關閉說明

```
#圖表系統是基於PHP系統架構，並且掛載於Apache2的服務上，
#因此如要關閉只需要關閉Apache2服務即可
service apache2 stop
```

### 6.5 Insight平台系統啟動/關閉說明

#### 6.5.1 啟動說明

```
#進入Insight平台系統目錄，執行service.sh後 Insight 平台系統即會執行。
./server.sh
```

#### 6.5.2 關閉說明

```
#如果Insight平台系統使用前台顯示方式執行，則直接跳出即可
#如果是後台執行則直接關閉工作流程即可
ps -aux | grep Insight #此部分為搜尋工作流程
kill pid #搜尋到工作流程後關閉即可
```

### 6.6 Nginx啟動/關閉說明

#### 6.6.1 啟動說明

```
#Nginx服務啟動使用的為Linux自帶的Service服務，因此以該服務執行方式執行即可。
service nginx start
```

#### 6.6.2 關閉說明

```
#Nginx服務關閉使用的為Linux自帶的Service服務，因此以該服務執行方式執行即可。
service nginx stop
```
