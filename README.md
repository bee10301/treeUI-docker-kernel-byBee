## 免責聲明

本文章僅為技術分享，刷機本身即無法保證您能維持原有的保固權益，且會格式化手機裡的文件（不包含記憶卡）。一切的操作若導致變磚，均為您本人所負責，作者沒辦法保證提供任何保固、維修、諮詢等協助。在開始刷機前請確定你擁有足夠的認知知道你在做什麼以及相應的責任。

## 只想要拿包的懶人

如果你是急急國王並且很熟悉刷機、且知道你在幹嘛，這邊直接附上我改好的 kernel，要注意的是這對應 `tree ui` 而不是原廠 rom 喔！

[tree ui XDA 原帖](https://u.bee.moe/43Vp5RD)

[tree ui XDA 原帖提供的載點](https://u.bee.moe/441LLj1)

[修改過的 kernel](https://github.com/bee10301/treeUI-docker-kernel-byBee/blob/main/kernel3.zip)


## 從 0 開始的刷機過程

強烈建議準備記憶卡並把資源包內的必要檔案放入，本教學也是以記憶卡為主進行說明。本人所持有為 `sm-a530F` 進行實機測試，其他相近機種不保證可行。

### 解鎖 OEM

設定 → 開發者選項 → 解鎖 OEM

沒有開發者選項請去「關於手機」「韌體資訊」對著組件版本多按幾次。

### 下載資源包

[github 連結](https://github.com/bee10301/treeUI-docker-kernel-byBee.git)

有在用 git 的同學可以直接 clone，複製一份進去記憶卡。

```bash
git clone https://github.com/bee10301/treeUI-docker-kernel-byBee.git
```

### 手機-進入下載模式

手機徹底關機。手機按 `Power`+`音量上`+`音量下`，進入下載模式。接上電腦。

### 電腦-刷入 twrp

用 `odin 3.12.7`，options 取消 `auto reboot`，只留下 `F. Reset Time`，勾選 AP 並選取 TWRP 附件，在點 start 之前，先做好心理準備，等一下上方出現 PASS！要在手機上馬上按下`Power`+`音量下` 強制結束，並且螢幕一黑就`Power`+`音量上` 進入 recovery 模式。

整個流程：

- 確定取消 auto
- 勾選 F
- 勾選 ap
- 選擇 twrp 檔案
- start（之後上方會出現 PASS！）
- 手機馬上按下`Power`+`音量下` 強制結束（之後螢幕會黑）
- `Power`+`音量上` 進入 recovery 模式

![](https://i.imgur.com/8RRtmFI.png)

### twrp 格式化

進了 twrp 恭喜你過了第一道門檻。手機在原廠的情況下內容都是加密的。所以首先我們需要 wipe 一下裡面的數據。

- `wipe` → format data
- `reboot` → recovery
- `install` 刷入 no-verity-opt-encrypt-6.0.zip、RMM_Bypass_v3_corsicanu.zip

### Treble 化

- `mount` 掛載 `system` 、`cache`、`data`
- 如果顯示無法 mount，請去 wipe data 裡面 format 成 ex4 然後選 fix
- `wipe` 掉 `system` 、`cache`、`data`
- `install` 刷入 TrebleCreator.zip，然後 reboot system 進入 recovery
- `install` → `img` 刷入 TWRP-jackpotlte-Treble.img 到 recovery，然後 reboot 進入 recovery
- `mount` 掛載 `system` 、`cache`、`data` 、`vendor`，
- `wipe` 清除 `system` 、`cache`、`data` 、`vendor` 、 `dalvik`

### 刷入 Tree UI

- `install` 刷入 TreeUiPlus 0.9.5.zip
- `wipe` 掉 `data`
- `install` 刷入 kernel3.zip
- `install` 刷入 Magisk-v26.1.zip
- Reboot

## 第一次開機過高的 dpi

第一次開啟 TreeUi 會是過高的 dpi

- 先去設定 → 螢幕顯示 → 螢幕解析度，改成 720P，之後再改回 1080P
- 螢幕縮放、字體大小樣式 改動成任意配置，再改到自己想要的即可

## 相關介紹

### Termux 配置

建議安裝官方 [github 版](https://github.com/termux/termux-app/releases)的 或者 [ZeroTermux](https://github.com/hanxinhao000/ZeroTermux)。

官方已經有整合好修改過的 docker 包了，建議先把 `Main repository`，`Game repository`，`Science repository` 切換到清華源，再安裝 root-repo 跟 docker。

```bash title="切換源" showLineNumbers
termux-change-repo
```

```bash title="更新資源" showLineNumbers
pkg update
```

```bash title="安裝root跟docker包" showLineNumbers
pkg install root-repo && pkg install docker
```

裝好了之後記得要執行 docker 才行

```bash title="termux" showLineNumbers
dockerd
```

### Termux 允許存取 android 空間

```bash title="用來方便存取檔案" showLineNumbers
termux-setup-storage
```

## 額外推薦

這邊推薦一個 termux 的分支 [ZeroTermux](https://github.com/hanxinhao000/ZeroTermux)，左滑選單多了很多便利的一鍵指令，還包含 2MOE Linux 導航的安裝指令，很適合剛入門的小白
