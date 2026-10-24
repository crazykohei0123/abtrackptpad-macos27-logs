ABTrackPTPad — macOS 27 (27.0.1 / 26A434) PTP 切替失敗の調査ログ一式
（Issue: https://github.com/knkz1114/ABTrackPTPad/issues への添付用）

ファイル:

1. ABTrackPTPad-app.log
   アプリのログ本体（~/Library/Logs/ABTrackPTPad.log のコピー）。
   全時間帯の THQA 読み出し / feature 3 書き込み / 初期レジスタ値 /
   唯一の切替成功 (01:44:07) と、その直後の切断・マウスモード復帰が含まれる。

2. unified-log-0144-only-successful-switch.log   ← 最重要
   唯一 PTP 切替に成功した瞬間 (2026-10-25 01:43:40〜01:44:20) の unified log
   (bluetoothd + BTLEServer)。report id=2 到達の 21ms 後にパッド自身がリンクを
   切断していること（"remotely-initiated", reason 719）が確認できる。

3. system-log-0206-powercycle-bluetoothd.log
   02:06 の電源サイクル時の bluetoothd 全キャプチャ。
   GATT statedump（Report characteristic の handle/Report Reference/CCCD 状態）、
   "no session is subscribed" エラー、BTLEServer の "Detected mouse LE HID" 等。

4. system-log-0237-bluetoothd-BTLEServer.log
   02:37 セッション（値スイープ実行時）。feature 3 書き込みが実デバイスへ
   届いていることの傍証（レジスタ読み戻しが 0/1/2 を反映）。

5. system-log-0240-hid-subsystem.log
   HID サブシステム側のキャプチャ。

6. system-log-0241-repair-bluetoothd-BTLEServer.log
   02:41 再ペアリングテスト（immediate write / スイープ実行版）。

7. system-log-0255-repair-channel2-bluetoothd-BTLEServer.log
   02:55-02:57 の再ペアリング（チャンネル1）→ チャンネル2 切替テスト。
   クリーンな著者シーケンスでの失敗が含まれる。

注意: システムログには周辺機器の情報（他のBluetooth機器名など）が含まれる場合が
あります。気になる場合は添付前に確認してください。
