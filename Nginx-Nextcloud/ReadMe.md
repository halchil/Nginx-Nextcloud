# はじめに
NginxとNextcloudを別々で立ち上げることに成功した。
nginx.confにlocationモジュールを追加し、Nextcloud立ち上げ時のポートを閉じることで可能となる。

# nginx.conf編集
`nginx.conf`

# Nextcloud立ち上げ時のポートを閉じる
