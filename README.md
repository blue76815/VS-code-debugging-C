# Linux環境下 使用VS code debugging C設定步驟
---
# 問題:

#### 在實做Lab0作業時,

##### 依照[GUN/Linux開發工具共筆](https://hackmd.io/@sysprog/gnu-linux-dev/https%3A%2F%2Fhackmd.io%2Fs%2FrJPKpohsx)中的[編輯器:Visual Studio Code 偵錯](https://youtu.be/eKIDP_9EexE?t=86)教學去設定debug 環境

##### 發現組態用GDB 設定時,在debug  mode環境下,無法同時藉由終端機下command和debug  mode程式連動

##  詳見實驗操作紀錄 [vs code debug test](https://youtu.be/S_NRCxbZpRU)

---
# 解決辦法
參考 [Debugging C/C++ with Visual Studio Code](https://youtu.be/X2tM21nmzfk?t=286) 方法
---
##### step1. 在vs code開啟終端機頁面
![](https://i.imgur.com/tEHrPWR.png)

##### 先下make指令,產出qtest執行檔
![](https://i.imgur.com/XGniTIp.png)

---
##### step2.debug mode改用 C++(GDB/LLDB)  設定
![](https://i.imgur.com/sZ3wKKe.png)

##### 選擇gcc-建置及偵錯使用中的檔案
![](https://i.imgur.com/EHTwL7H.png)

---
##### 在launch.json組態設定預設為
![](https://i.imgur.com/aNZU8zy.png)
##### 修改launch.json組態
##### step3-1. 確認
"request": "launch",

##### step3-2.  
"program": "${fileDirname}/qtest", //改成我們指定的指定 qtest 執行檔名稱路徑


##### step3-3.砍掉
     "preLaunchTask": "gcc build active file",
     "miDebuggerPath": "/usr/bin/gdb"
修改後如下

`launch.json`
```clike=
{
    // 使用 IntelliSense 以得知可用的屬性。
    // 暫留以檢視現有屬性的描述。
    // 如需詳細資訊，請瀏覽: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "gcc - 建置及偵錯使用中的檔案",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/qtest", //指定 qtest 執行檔名稱路徑
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "啟用 gdb 的美化顯示",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
        }
    ]
}
```
#### step4.
#### 最後按debug mode下,也能跨到console.c執行,且能在終端機一邊下command,同時trace code執行過程做觀察

![](https://i.imgur.com/J7xE6lf.png)


## 實驗結果紀錄 [vs code debug test2](https://youtu.be/geMp11McV0w)

# 補充說明 
### 若想重設vs code組態設定
### step1.在專案文件夾中 開啟顯示隱藏檔
### step2.將隱藏文件夾.vscode刪除 即能重新配置launch.json組態設定

![](https://i.imgur.com/ssOhTVy.png)

[How to debug C code with GDB in VSCode (Linux)](https://www.youtube.com/watch?v=aWIs6Kv1MvE)