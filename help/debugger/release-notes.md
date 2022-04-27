---
title: Adobe Experience Platform Debugger リリースノート
description: Adobe Experience Platform Debugger の最新のリリースノートです。
keywords: debugger;experience Platform Debugger 拡張機能；chrome；拡張機能；リリースノート
uuid: 47a5d6f3-c074-4ad5-ad4b-e6030496689b
exl-id: 3eed44da-5f85-413e-a783-3a0df03a2baf
source-git-commit: a15b5525d3a2fa034715803c83dc22a94915347e
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Adobe Experience Platform Debugger リリースノート

## バージョン 1.3.0 - 2022 年 1 月 28 日

* 最新のリリースバージョンとメモを表示するための「バージョン情報」リンクを追加しました。
* Analytics リクエストの後処理されたヒットを表示する切り替え機能を追加しました。 切り替えは、「Analytics」セクションで使用できます。
* デバッガー外でセッションが閉じられた際のリモートデバッグセッションの問題を修正しました。
* 「Web SDK Edge Transactions」タブに表示されるエラー通知を修正しました。
* デバッガーが_satellite オブジェクトにアクセスした際の、ページ廃止の警告に関するAdobeタグを修正しました。
* AppMeasurement インスタンスがページで見つからない場合があった問題を修正しました。
* デバッガーウィンドウを初めて開いたときに発生するページ接続の問題を修正しました。

## バージョン 1.2.0 - 2021 年 10 月 27 日

* ネットワーク表示のすべてのブラウザータブからイベントを表示します。 イベントを現在のタブからのみ表示するには、デバッガーの右下隅にあるロックアイコンを選択します。
* ブランディングを更新しました。

## バージョン 1.1.0 - 2021 年 10 月 6 日

* リモートデバッグビジュアライゼーション — Adobe Experience Platform Web SDK/「エッジトランザクション」セクションで、リモートデバッグイベントを視覚的なフローチャートに整理します。
* 新しいリモートデバッグセッションを開始する際に、ページで使用するAdobe Experience Platform Web SDK IMS 組織が、ログイン組織と一致する必要があります。
* 接続されたタブのエッジトランザクションのみを表示します。 Target のトレースログは、ログ/Edge セクションで引き続き利用できます。
* ページ上のAdobe Experience Platform Web SDK のインスタンスごとに、別々のデータストリーム ID 設定の上書きを許可します。 デバッグ対応切り替えを追加します。
* Adobe Experience Platform Web SDK のリモートデバッグセッションで、Adobe Targetトレーストークンが送信されない場合がある問題を修正しました。

## バージョン 1.0.0 2021 年 5 月 6 日

* Debugger の最初のメインリリースです。 Experience Cloud Debugger。
