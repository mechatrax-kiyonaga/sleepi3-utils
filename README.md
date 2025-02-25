sleepi3-utils
==========

slee-Pi 3 を操作するためのツール類を提供します。

## 提供ファイル
次のファイルがパッケージに含まれています。

### /usr/sbin/sleepi3alarm
RTC のアラームを操作するスクリプトです。  
/sys/class/rtc/${RTC_DEVICE}/wakealarm にアクセスします。  
RTC_DEVICE の値は /etc/default/sleepi3 に記述します。

* -g, get  
  アラーム時刻を取得します。  
  未設定の場合は空の文字列を返します。  

* -s, set \<STRING\>  
  アラーム時刻を設定します。  
  翌年同日の一日前までが指定可能です。  

  \<STRING\> : 時刻を指定します。

  次に例を示します。

  - 2017/03/17 19:00:00 に設定する場合
    ```
    sleepi3alarm set "2017/03/17 19:00:00"
    ```
    日時を指定してアラームを設定できます。  
    時刻のみの指定も可能です。

  - 五分後に設定する場合  
    ``` bash
    sleepi3alarm set "+5min"
    ```
    相対値を指定してアラームを設定できます。  
    year, month, day, hour, min, sec 等の指定が可能です。  

  - 翌日午前九時に設定する場合  
    ``` bash
    sleepi3alarm set "tomorrow 9:00"
    ```
    相対値と絶対値を組み合わせてアラームを設定することもできます。

  以上の例で示した文字列のフォーマットは GNU date コマンドに準拠しています。  
  フォーマットの詳細については次の URL を参照してください。  
  https://www.gnu.org/software/coreutils/manual/html_node/Date-input-formats.html

* -c, clear  
  アラーム設定を解除します。

* -h, help  
  ヘルプを表示します。

* -v, version  
  バージョンを表示します。

### /usr/sbin/sleepi3ctl
slee-Pi 3 を操作するための実行ファイルです。  
使用可能なオプションを次に示します。

* -g, get PARAMETER  
  指定したパラメータの値を取得します。  

  パラメータには次のいずれかを指定します。

  - extin  
    外部入力の状態を表示します。  
    
    0 : 外部入力はオフです。  
    1 : 外部入力はオンです。

  - extin-count  
    外部入力のカウント値を表示します。  
    単位は [秒] です。  
    最大値は 255 です。

  - extin-powerdown  
    外部入力による強制電源断の設定を表示します。  

    0 : 外部入力による強制電源断が無効です。  
    1 : 外部入力による強制電源断が有効です。

  - extin-trigger  
    外部入力の起動トリガの設定を表示します。  

    0 : エッジ検出で起動します。  
    1 : レベル検出で起動します。

  - extout  
    外部出力の状態を表示します。  

    0 : 外部出力はオフです。  
    1 : 外部出力はオンです。

  - measurement-interval  
    電源電圧の計測間隔を表示します。  
    単位は [秒] です。

  - restart  
    システムが無応答と判定された場合の動作設定を表示します。  

    0 : 電源断を行います。  
    1 : 電源の再投入を行います。

  - restore-voltage  
    電源復旧電圧の値を表示します。  
    単位は [mV] です。  
    0 の場合は無効です。
 
  - ri-trigger  
    RI 信号の起動トリガの設定を表示します。  

    0 : エッジ検出で起動します。  
    1 : レベル検出で起動します。

  - sleep-timeout  
    スリープ動作へ移行するタイムアウト値を表示します。  
    単位は [秒] です。  
    0 の場合は無効です。

  - switch  
    プッシュスイッチの状態を表示します。  
    
    0 : プッシュスイッチはオフです。  
    1 : プッシュスイッチはオンです。

  - switch-count  
    プッシュスイッチが押下されている時間のカウント値を表示します。  
    単位は [秒] です。  
    最大値は 255 です。

  - uvlo  
    アラーム発生時の低電圧動作防止機能の状態を表示します。  

    0 : 低電圧動作防止機能は無効です。  
    1 : 低電圧動作防止機能は有効です。

  - voltage [1|2|active|all]  
    入力電圧の値を表示します。  
    単位は [mV] です。  
    オプションは省略できます。  
    省略した場合は active になります。  
    
    1 : CN1 の入力電圧の値を表示します。  
    2 : CN2 の入力電圧の値を表示します。  
    active : 現在の入力電圧の値を表示します。  
    all : CN1, CN2 の入力電圧の値を表示します。

  - wakeup-flag  
    起動ステータスレジスタのフラグを表示します。  

    poweron : 電源接続が起動要因です。  
    watchdog : ウォッチドッグタイマのタイムアウトによる再起動が起動要因です。  
    alarm : リアルタイムクロックのアラーム発生が起動要因です。  
    switch : プッシュスイッチの押下が起動要因です。  
    extin : 外部入力の検出が起動要因です。  
    ri : RI 信号の検出が起動要因です。  
    restoration : 電源復旧の検知が起動要因です。

  - wakeup-status  
    起動ステータスレジスタの値を表示します。

  - watchdog-timeout  
    システムの応答を検知するタイムアウト値を表示します。  
    単位は [秒] です。  
    0 の場合は無効です。

* -s, set PARAMETER \<VALUE\>  
  指定したパラメータに値を設定します。  
  パラメータには次のいずれかを指定します。

  - extin-powerdown \<0|1\>  
    外部入力による強制電源断を設定します。  

    0 : 外部入力による強制電源断が無効です。  
    1 : 外部入力による強制電源断が有効です。

  - extin-trigger \<0|1\>  
    外部入力の起動トリガを設定します。  

    0 : エッジ検出で起動します。  
    1 : レベル検出で起動します。

  - extout \<0|1\>  
    外部出力を設定します。  

    0 : 外部出力はオフです。  
    1 : 外部出力はオンです。

  - measurement-interval \<0..255\>  
    入力電圧の計測間隔を設定します。  
    単位は [秒] です。  
    最大値は 255 です。  
    0 を設定すると計測を行いません。

  - restart \<0|1\>  
    システムが無応答と判定された場合の動作を設定します。  

    0 : 電源断を行います。  
    1 : 電源の再投入を行います。

  - restore-voltage \<0..24000\>  
    スリープ時に計測された入力電圧が設定した値を上回った場合に起動します。  
    単位は [mV] です。  

  - ri-trigger \<0|1\>  
    RI 信号の起動トリガを設定します。  

    0 : エッジ検出で起動します。  
    1 : レベル検出で起動します。

  - sleep-timeout \<0..255\>  
    スリープ動作へ移行するタイムアウト値を設定します。  
    単位は [秒] です。  

    0 : 応答を検知しません。  
    1..255 : 設定値を超えても応答を検知できない場合は無応答と判定されます。

  - uvlo \<0|1\>  
    アラーム発生時の低電圧動作防止機能を設定します。  

    0 : 低電圧動作防止機能は無効です。  
    1 : 低電圧動作防止機能は有効です。

  - watchdog-timeout \<0..255\>  
    システムの応答を検知するタイムアウト値を設定します。  
    単位は [秒] です。  

    0 : 応答を検知しません。  
    1..255 : 設定値を超えても応答を検知できない場合は無応答と判定されます。

* -h, help  
  ヘルプを表示します。

* -v, version  
  バージョンを表示します。

### /etc/default/sleepi3   
slee-Pi 3 の設定を行うためのファイルです。  
設定可能な項目を次に示します。

* RTC_DEVICE  
  slee-Pi 3 のリアルタイムクロックモジュール部のデバイス名を記述します。  
  デフォルトは rtc0 です。

* HCTOSYS  
  リアルタイムクロックからシステムへ時刻を同期する動作の設定です。  
  デフォルトは yes です。  

  yes : 起動時に時刻を同期します。  
  no : 起動時に時刻を同期しません。  

* SYSTOHC  
  システムからリアルタイムクロックへ時刻を同期する動作の設定です。  
  デフォルトは yes です。  

  yes : 終了時に時刻を同期します。  
  no : 終了時に時刻を同期しません。  

* I2C_ADDRESS  
  slee-Pi 3 のパワーマネジメントモジュール部の I2C アドレスを指定します。  
  デフォルトは 0x6E です。

* HEARTBEAT_TIMEOUT  
  システムの応答がない場合に電源を再投入するまでの時間です。  
  単位は [秒] です。  
  無効にするには 0 を指定します。  
  デフォルトは 60 です。

* RETRY_BOOT  
  起動に失敗してシステムの応答がない場合に電源の再投入を行う設定です。  
  有効にするには 1 を指定します。  
  無効にするには 0 を指定します。  
  デフォルトは 1 です。

* BOOT_TIMEOUT  
  システムの起動時にウォッチドッグタイマが動作を開始するまでの時間です。  
  単位は [秒] です。  
  無効にするには 0 を指定します。  
  デフォルトは 90 です。

* POWEROFF_DELAY  
  システムのシャットダウンが完了してから電源断までの時間です。  
  単位は [秒] です。  
  無効にするには 0 を指定します。  
  デフォルトは 10 です。

* EXTIN_FORCED_SHUTDOWN  
  外部入力が 10 秒以上検出された場合に強制電源断を行う設定です。  
  有効にするには 1 を指定します。  
  無効にするには 0 を指定します。  
  デフォルトは 0 です。

### /usr/lib/sleepi3-utils/hctosys.sh  
リアルタイムクロックからシステムへ時刻を同期するスクリプトファイルです。

### /usr/lib/sleepi3-utils/heartbeat.py  
ソフトウェアハートビート動作を行うスクリプトファイルです。

### /usr/lib/sleepi3-utils/config.py  
slee-Pi 3 の起動、再起動、停止の設定を行うスクリプトファイルです。

### /usr/lib/sleepi3-utils/systohc.sh  
システムからリアルタイムクロックへ時刻を同期するスクリプトファイルです。

### /usr/lib/sleepi3-utils/timeoutpid.sh  
コマンド実行時にタイムアウト動作を行うスクリプトファイルです。

### /usr/share/bash-completion/completions/sleepi3alarm
sleepi3alarm のコマンド補完を行うための設定ファイルです。

### /usr/share/bash-completion/completions/sleepi3ctl
sleepi3ctl のコマンド補完を行うための設定ファイルです。

### /usr/share/sleepi3-utils/sleepi3_utils.py
slee-Pi 3 をコマンド操作するためのヘルパクラスが実装されたファイルです。

### /usr/share/doc/sleepi3-utils/changelog.gz  
パッケージの変更履歴を記録したファイルです。

### /usr/share/doc/sleepi3-utils/copyright  
ソースの著作権とライセンスを記載したファイルです。

### /lib/udev/rules.d/85-sleepi3-utils.rules  
slee-Pi 3 のデバイスを定義した設定ファイルです。

### /lib/systemd/system/sleepi3-halt.service  
slee-Pi 3 の終了設定を行うサービスです。

### /lib/systemd/system/sleepi3-hctosys.service  
リアルタイムクロックからシステムへ時刻を同期するサービスです。

### /lib/systemd/system/sleepi3-heartbeat.service  
ソフトウェアハートビートを実行するサービスです。

### /lib/systemd/system/sleepi3-restart.service  
slee-Pi 3 の再起動設定を行うサービスです。

### /lib/systemd/system/sleepi3-start.service  
slee-Pi 3 の起動設定を行うサービスです。

### /lib/systemd/system/sleepi3-stop.service  
slee-Pi 3 の終了設定を行うサービスです。

### /lib/systemd/system/sleepi3-systohc.service  
システムからリアルタイムクロックへ時刻を同期するサービスです。
