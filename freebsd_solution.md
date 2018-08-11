# FreeBSD's solutions
此處不定期更新，此處會有新加入的解決方案：[Telegram](https://t.me/joinchat/AAAAAE4fEBbZSpfeyuuE0Q)

# 常見問題類別
## 如何設定 Wi-Fi 網路？
Tag: #WiFi #Setting

1. 查詢自己的網卡<br>
   `$ pciconf -lv` <br>
   找到您無線網卡的部分。

2. `man -k (網卡廠商名稱)` <br>
   找到自己網卡廠商的設定說明文件，並依循設定。<br>
   <br>
   以 ath 為例，在 /boot/loader.conf 插入：<br>

   ```
   if_ath_load="YES"
   if_ath_pci_load="YES"
   ```

   不過 ath 驅動內核已經預先載入，是否加入這兩條其實沒有太大影響，
   設定完成後請重開機以套用變更。

3. 設定 Wi-Fi 介面<br>
   `# ifconfig wlan0 create wlandev (pciconf 得到的網卡介面名稱) up`
   <br>
   從 Wi-Fi 網卡介面設定出一個 wlan0 介面，並設定為啟用狀態。

4. 寫入 Wi-Fi 設定<br>
   `# wpa_passphrase (SSID 名稱) (SSID 密碼) >/etc/wpa_supplicant.conf`
   <br>
   透過這個方式設定的 wpa_supplicant.conf，密碼應為加密狀態。

5. Wi-Fi 連線<br>
   `# wpa_supplicant -i (剛建立的介面名稱) -c /etc/wpa_supplicant.conf -B`

6. 設定 DHCP<br>
   `# dhclient wlan0`

至此你可以開始使用無線網路瀏覽網路囉！

## 讓 FreeBSD 有中文語系能用
Tag: #Chinese #中文 #login.conf #chpass #tchinese

1. 開啟 /etc/login.conf，接著找到有 russian|Russian Users Account:\ 的地方<br>
   在該位置底下增加：( `[Tab]` 按下 Tab 鍵)

   ```
   tchinese:\
   [Tab]charset=UTF-8:\
   [Tab]lang=zh_TW.UTF-8:\
   [Tab]language=zh_TW:\
   ```

2. 輸入 `cap_mkdb /etc/login.conf` (root) 讓設定生效。

3. 輸入 `chpass (使用者名稱)` (root)

   在 class 的部分填入 tchinese。<br>
   假如你在第一步驟把 tchinese 改成 chineseT 之類的，
   那你就在 class 填入你剛修改的名稱。

4. `chpass root` (root)
   
   同第三步驟。

5. 重新登入使用者帳號即可。

## 安裝 Xorg 環境
Tag: #Xorg #MATE #LightDM #sysrc

1. 安裝 Xorg、MATE 與 LightDM 登入管理器<br>
   `# pkg install xorg mate lightdm lightdm-gtk-greeter`

2. 啟用 dbus 和 lightdm<br>
   `# sysrc dbus_enable=YES lightdm_enable=YES`

3. 使用想要使用 Xorg 的帳號，在其家目錄建立 `.xinitrc` ，
   內容為：<br>

   ```
   exec mate-session
   ```

4. 儲存檔案，重開機即可。

> Note: 若開機後未正常進入畫面，請手動輸入 `startx` 進去，或是輸入
> `sudo lightdm` 進去即可！

## 設定 Fcitx 輸入法
接續自《安裝 Xorg 環境》<br/>
Tag: #Fcitx #MATE #chewing

1. 安裝 Fcitx 與新酷音<br>
   `# pkg install zh-fcitx zh-fcitx-chewing`

2. 建立 `.xprofile` ，內容為：<br>
   ```
   export GTK_IM_MODULE=fcitx
   export GTK3_IM_MODULE=fcitx
   export QT_IM_MODULE=fcitx
   export XMODIFIERS="@im=fcitx"
   fcitx &
   ```

3. 開啟 `.xinitrc` ，在最上面增加：<br>
   ```
   export GTK_IM_MODULE=fcitx
   export GTK3_IM_MODULE=fcitx
   export QT_IM_MODULE=fcitx
   export XMODIFIERS="@im=fcitx"
   fcitx &
   ```

4. 重開機即可生效。

## 下載與解壓縮 /usr/ports 內容
Tag: #ports #portsnap

# 疑難雜解類別
## 無法使用 Xorg，顯示 `no screens found` 錯誤


