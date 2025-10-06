[![](https://github.com/TechTutoPPT/-Turn-TV-Box-Into-Mini-PC/blob/main/IMG_8263.jpg)](https://youtu.be/7c2HJU334oo)

手上有部電視盒子(Allwinner sun50iw6 4核心 Cortex-A53(SoC), 4GB RAM, 32GB ROM, Android 7.0 預設已Root), 
由於年代久遠, 內置的影音平台已不再提供服務, 一直想找方法將它刷成Linux系統但至令未果, 假如有高人能指教在下萬分感激!
即然實刷Linux未成, 唯有使用Termux代替, 再為它加上桌面環境就能變成一台迷你PC作為僚機處理一些定時工作(長開省電).

過程如下:
下載並安裝Termux:
```
https://f-droid.org/repo/com.termux_1002.apk
```
啟動Termux後執行更新:
```
pkg update && pkg upgrade -y
```
安裝x11套件庫:
```
pkg install x11-repo
```
安裝桌面環境及xRDP服務:
```
pkg update
pkg install xfce4 xfce4-goodies xrdp
```
編輯xRDP的配置文件:
```
sed -i 's/port=-1/port=5901/g' ../usr/etc/xrdp/xrdp.ini
```
啟動xRDP:
```
xrdp
vncserver -xstartup ../usr/bin/startxfce4 -listen tcp:1
```
執行後會要求輸入登入密碼(8個位), 不用設定唯讀顯示密碼.
然後Windows端便能透過遠端桌面連線輸入IP地址來連接.

結束連線後需關閉服務:
```
vncserver -kill :1
```
不然於/data/data/com.termux/files/usr/tmp/.X11-unix/會殘留鎖定文件, 影響下次派發的X server顯示號碼

再補上一些技巧:
```
https://github.com/Universal-Debloater-Alliance/universal-android-debloater-next-generation
```
使用Universal Android Debloater Next Generation去移除預裝及失效的應用以釋放電視盒子的效能及儲存容量.
另可安裝Auto Start這應用, 使電視盒子能啟動後自動執行Termux, 而Termux亦自動執行.bashrc運行xrdp服務, 
這樣就能無線遠端連線.
最後其實只要Termux免Root安裝Docker再運行LibreTV又能回復電視盒子功能...
