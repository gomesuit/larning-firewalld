- サービスの確認
  - `firewall-cmd --get-services`
- ICMPタイプの確認
  - `firewall-cmd --get-icmptypes`
- デフォルトゾーン定義ファイル
  - /usr/lib/firewalld/zones
- 独自ゾーン定義ファイル
  - /etc/firewalld/zones
- 定義更新
  - `firewall-cmd --reload`
- 各ゾーンの設定確認
  - `firewall-cmd --list-all-zones`
- デフォルトゾーンの確認
  - `firewall-cmd --get-default-zone`
- デフォルトゾーンの変更
  - `firewall-cmd --set-default-zone=external`
- NICインターフェースにゾーンを適用
  - /etc/sysconfig/network-scripts/ 配下の設定ファイル
  - ZONE=externalなどを追記
  - `nmcli c reload`
  - `nmcli c down eth1; nmcli c up eth1`
- ゾーンの定義変更手順
  - 許可されているサービスを表示
    - `firewall-cmd --list-services --zone=public`
  - 許可するサービスを追加
    - `firewall-cmd --add-service=http --zone=public`
  - 指定のサービスが許可されているか確認
    - `firewall-cmd --query-service=http --zone=public`
  - 許可するサービスを削除
    - `firewall-cmd --remove-service=http --zone=public`
  - 禁止されているICMPタイプを表示
    - `firewall-cmd --list-icmp-blocks --zone=public`
  - 禁止するICMPタイプを追加
    - `firewall-cmd --add-icmp-block=echo-request --zone=public`
  - 指定のICMPタイプが禁止されているか確認
    - `firewall-cmd --query-icmp-block=echo-request --zone=public`
  - 禁止するICMPタイプを削除
    - `firewall-cmd --remove-icmp-block=echo-request --zone=public`
- IPマスカレードとDNATの設定
  - 現在の設定を確認
    - `firewall-cmd --query-masquerade --zone=public`
  - IPマスカレードを有効化
    - `firewall-cmd --add-masquerade --zone=public`
  - IPマスカレードを無効化
    - `firewall-cmd --remove-masquerade --zone=public`
- DNATの設定
  - 現在の設定を確認
    - `firewall-cmd --list-forward-ports --zone=public`
  - 変換ルールを追加
    - `firewall-cmd --add-forward-port=<変換ルール> --zone=public`
  - 変換ルールの存在を確認
    - `firewall-cmd --query-forward-port=<変換ルール> --zone=public`
  - 変換ルールを削除
    - `firewall-cmd --remove-forward-port=<変換ルール> --zone=public`
  - 変換ルール例
    - TCP80宛のパケットを192.168.1.10のWEBサーバに転送する場合
      - `port=80:proto=tcp:toaddr=192.168.1.10`
  - 起動時の設定を変更する場合は`--permanent`オプションを付与
  - 実行時と起動時の両方を変更する場合はオプションありなし両方必要
- ルータ設定例
  - `firewall-cmd --add-forward-port=port=80:proto=tcp:toaddr=192.168.1.10 --zone=external`
