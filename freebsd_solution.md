# FreeBSD's solutions
此處不定期更新，此處會有新加入的解決方案：[Telegram](https://t.me/joinchat/AAAAAE4fEBbZSpfeyuuE0Q)

## 如何設定 Wi-Fi 網路？
Tag: #WiFi #Setting

1. 查詢自己的網卡<br/>
   `$ pciconf -lv` <br/>
   找到您無線網卡的部分。

2. `man -k (網卡廠商名稱)` <br/>
   找到自己網卡廠商的設定說明文件，並依循設定。<br/>
   <br/>
   以 ath 為例，在 /boot/loader.conf 插入：<br/>

   ```
   if_ath_load="YES"
   if_ath_pci_load="YES"
   ```

   不過 ath 驅動內核已經預先載入，是否加入這兩條其實沒有太大影響，
   設定完成後請重開機以套用變更。

3. 設定 Wi-Fi 介面<br/>
   `# ifconfig wlan0 create wlandev (pciconf 得到的網卡介面名稱) up`
   <br/>
   從 Wi-Fi 網卡介面設定出一個 wlan0 介面，並設定為啟用狀態。

4. 寫入 Wi-Fi 設定<br/>
   `# wpa_passphrase (SSID 名稱) (SSID 密碼) >/etc/wpa_supplicant.conf`
   <br/>
   透過這個方式設定的 wpa_supplicant.conf，密碼應為加密狀態。

5. Wi-Fi 連線<br/>
   `# wpa_supplicant -i (剛建立的介面名稱) -c /etc/wpa_supplicant.conf -B`

6. 設定 DHCP<br/>
   `# dhclient wlan0`

至此你可以開始使用無線網路瀏覽網路囉！
