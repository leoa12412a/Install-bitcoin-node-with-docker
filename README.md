# 使用Docker架設Bitcoin Full Node

## 直接安裝

### Step 1. 下載安裝包

到<a href="https://bitcoin.org/en/download">這裡</a>下載，這裡只有嘗試Linux的安裝

下載完後解壓縮
```
tar xzf bitcoin-0.20.0-x86_64-linux-gnu.tar.gz
```

安裝到/usr/local/bin並給予755權限
```
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-0.20.0/bin/*
```

安裝後可以選擇3中模式

- 圖形化(請閱讀<a href="https://bitcoin.org/en/full-node#other-linux-gui">圖形化設定方式</a>)
- 指令模式(請閱讀<a href="https://bitcoin.org/en/full-node#other-linux-gui">指令設定方式</a>)
- 同時都有(請兩個都閱讀，但兩者的配置不能一樣)

這裡安裝的是指令模式

先配置一個設定的檔案和存放的路徑
```
touch /usr/local/bin/bitcoin.conf
mkdir /usr/local/bin/bitcoin-data
```

在/usr/local/bin/bitcoin.conf裡寫上以下內容
```
conf=/usr/local/bin/bitcoin.conf
datadir=/usr/local/bin/bitcoin-data
```

輸入以下指令將會已配置啟動btc輸出"Bitcoin Core starting"，表示btc已經在啟動了
```
bitcoind -conf=/home/bitcoin/bitcoin.conf -daemon
``` 
啟動需要一些時間，你可以使用bitcoin-cli的指令來測試是否完成
```
bitcoin-cli getblockchaininfo
```
如果尚未完成會顯示以下錯誤代碼
```
bitcoin-cli：

error: {"code":-28,"message":"Verifying blocks..."}
```

當Bitcoin Core首次啟動時，他將會開始下載區塊鏈，並會占用大部分的平寬，下載至少需要幾天的時間，但我們也可以隨時使用指令停止他，下次啟動時會從停止時位置繼續下載
```
bitcoin-cli stop
```

<a href="bitcoin_core_install_centos7.txt">安裝教學 by.沅平</a>

## 使用docker安裝

安裝好docker(<a href="https://github.com/leoa12412a/Docker">方法</a>)後輸入以下指令，在/home/bitcoin裡面可以找到bitcoin.conf

<a href="https://github.com/kylemanna/docker-bitcoind">來源</a>

```
docker volume create --name=bitcoind-data;
docker run -v /home/bitcoin/bitcoind-data:/bitcoin/bitcoind-data --name=bitcoind-node -d \
-p 8333:8333 \
-p 127.0.0.1:8332:8332 \
-v /home/bitcoin:/bitcoin/.bitcoin \
kylemanna/bitcoind;
```

輸入以下查詢資訊指令已檢測是否安裝成功

```
docker {CONTAINER ID} e8646e8 bitcoin-cli getblockchaininfo
```

安裝成功後可以在<a href="https://bitnodes.io/#join-the-network">官網</a>上查到是否成功與其他節點連線

![image](img/check-btc.PNG)<br />

