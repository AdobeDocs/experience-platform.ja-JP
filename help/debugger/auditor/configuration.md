---
title: 設定テストリファレンス
description: Adobe Experience Platform Debugger での設定に対する Auditor の機能テストの方法を説明します。
exl-id: 92b07224-57f1-4891-9923-aa079945e6bc
source-git-commit: 797d4f305b4a6884ada4e0619beadff6a45ab42d
workflow-type: tm+mt
source-wordcount: '740'
ht-degree: 66%

---

# 設定テストリファレンス

このリファレンスでは、Adobe Experience Platform Debugger の Auditor 機能が設定テストを実行する方法に関する詳細を提供します。

>[!NOTE]
>
>Platform Debugger での監査テストについて詳しくは、 [auditor 機能の概要](./overview.md).

構成テストでは、実装内の特定の設定や値、または競合の可能性をスキャンします。Platform Auditor は、タグを他のルールおよび推奨ベストプラクティスと比較して評価します。

| テスト | 重み付け | 条件 | 推奨 |
| --- | --- | --- | --- |
| Advertising Cloud - コンバージョン名は英数字のみを使用している | 3 | この `ev_conversion_property_name` パラメーターには、EXCEPT を除く数値と 10 進数のみを含める必要があります `ev_transid` パラメーター。テキスト値または数値を含めることができます。 `everesttech.net` _ で始まる URL パラメーターを含む、`ev_` ピクセルを探します。 | トランザクションプロパティのパラメーターには、数値と小数値のみを含めてください。<br><br>警告： その他の値タイプは、データが失われる原因となる場合があります。 |
| Advertising Cloud - コンバージョン名は URL で使用できる文字のみを使用している | 3 | コンバージョンプロパティ名に、アンパサンドや疑問符を含めることはできません。 | トランザクションプロパティのパラメーターに、エンコードされていないアンパサンドや疑問符が含まれていないことを確認してください。これらがあると、URL 形式が壊れます。<br><br>警告：エンコードされていないアンパサンドまたは疑問符が含まれるプロパティパラメーター ( 例：  `ev_formComplete?=1` または  `ev_formComplete&Submit=1`) を含めないと、データが失われる可能性があります。 |
| Advertising Cloud - トランザクション ID が正しく実装されている | 1 | プロパティ名  `ev_transid=` は空にできません。 | プロパティ名  `ev_transid=` 値なしでは残せません。 値が指定されていない場合、トランザクションデータが失われる可能性があります。値の割り当て先 `ev_transid=` または、ピクセルからパラメーターを削除します。 |
| Analytics - DOM でインスタンス化されている | 5 | Adobe Analytics コードがインストールされていないか、実行に失敗します。Web ページで Analytics コードが見つからない場合、0 を返します。 | ページで Analytics タグが実装され、後続のスクリプトアクティビティによってブロックされていないことを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Analytics - インスタンス化は 1 回のみ | 5 | Adobe Analytics コードがページで複数回検出されました。Web ページに A-Analytics コードが見つからない場合、0 を返します。 | ページ上の Analytics タグが 1 つだけであることを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html) |
| Analytics - 最新バージョン | 3 | ページで Analytics コードライブラリの最新バージョンが実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。Web ページで Analytics コードが見つからない場合、0 を返します。 | 最新バージョンの Analytics ライブラリをインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) |
| Launch - DOM Ready 後、サードパーティタグが非同期で読み込まれる | 3 | 優れたユーザーエクスペリエンスと正確なデータ収集のバランスを取るには、DOM Ready 時にサードパーティタグをトリガーする必要があります。 これにより、サイトの機能に影響を与えずに、これらのトラッキングスクリプトを確実に実行することができます。 | DOM Ready 時に実行されるサードパーティのピクセルを実行するすべてのルールを調整して、この問題を解決します。<br><br>[追加情報](../../tags/ui/managing-resources/rules.md) |
| Experience Cloud ID サービス - 最新バージョン | 2 | 訪問者 ID サービスコードライブラリ（ visitorAPI.js）の最新バージョンがページで実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | 最新バージョンの訪問者 ID サービスライブラリをインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/library.html) |
| Launch - 最新バージョン | 2 | これらのページで、タグコードライブラリ (Turbine) の最新バージョンが実行されていません。 Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | タグライブラリを再構築して公開します。<br><br>[追加情報](../../tags/quick-start/quick-start.md) |
| Target - 最新バージョン | 2 | Target コードライブラリの最新バージョンがページで実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | 最新バージョンの Target ライブラリをインストールしてください。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/) |
| Target - mboxDefault は mboxCreate よりも優先される | 5 |  mboxCreate の適切な使用方法は次のようになります。<br><br> `<div class="mboxDefault"><!-Customer content--></div><script>mboxCreate('myMboxName')</script>` | 必ず  `<div class="mboxDefault"></div>` タグを使用して、mboxCreate() を呼び出す前に貼り付けます。 at.js による追加はおこなわれません。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/) |
| Target - 有効な DOCTYPE | 5 | 無効な DOCTYPE が検出されました。このシナリオでは、mbox は起動されません。at.js の場合、DOCTYPE は標準モードである必要があります。そうしないと、Target は動作しません。 | ページ上の DOCTYPE を更新します。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/atjs/target-atjs-faq/) |

{style=&quot;table-layout:auto&quot;}
