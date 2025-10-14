---
title: 設定テストリファレンス
description: auditor 機能を使用してAdobe Experience Platform Debuggerの設定をテストする方法を説明します。
exl-id: 92b07224-57f1-4891-9923-aa079945e6bc
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 50%

---

# 設定テストリファレンス

このリファレンスでは、Adobe Experience Platform Debuggerの auditor 機能で設定テストを実行する方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debugger の auditor テストについて詳しくは、[auditor 機能の概要 &#x200B;](./overview.md) を参照してください。

構成テストでは、実装内の特定の設定や値、または競合の可能性をスキャンします。Experience Platform Auditor は、他のルールや推奨されるベストプラクティスに照らしてタグを評価します。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Advertising Cloud - コンバージョン名には英数字のみを使用します | 3 | `ev_conversion_property_name` パラメーターには、テキストまたは数値を含めることができる `ev_transid` パラメーターを除いて、数値と小数値のみを含めることができます。 `ev_` で始まる URL パラメーターを含む `everesttech.net` ピクセルを探します。 | トランザクションプロパティのパラメーターには、数値と小数値のみを含めてください。<br><br> 警告：他の値タイプを使用すると、データが失われる可能性があります。 |
| Advertising Cloud - コンバージョン名には、URL で安全な文字を使用します | 3 | コンバージョンプロパティ名に、アンパサンドや疑問符を含めることはできません。 | トランザクションプロパティのパラメーターに、エンコードされていないアンパサンドや疑問符が含まれていないことを確認してください。これらがあると、URL 形式が壊れます。<br><br> 警告：エンコードされていないアンパサンドまたは疑問符を含むプロパティ パラメーター（例：`ev_formComplete?=1` または `ev_formComplete&Submit=1`）を使用すると、データが失われる可能性があります。 |
| Advertising Cloud - トランザクション ID が正しく実装されている | 1 | プロパティ名 `ev_transid=` を空にすることはできません。 | プロパティ名 `ev_transid=` を値なしで残すことはできません。 値が指定されていない場合、トランザクションデータが失われる可能性があります。値を `ev_transid=` に割り当てるか、パラメーターをピクセルから削除します。 |
| Analytics - DOM でインスタンス化 | 5 | Adobe Analytics コードがインストールされていないか、実行に失敗します。Analytics コードが web ページで見つからない場合は、0 を返します。 | ページで Analytics タグが実装され、後続のスクリプトアクティビティによってブロックされていないことを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Analytics - 1 回インスタンス化 | 5 | Adobe Analytics コードがページで複数回検出されました。A-Analytics コードが Web ページで見つからない場合、0 を返します。 | ページ上の Analytics タグが 1 つだけであることを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Analytics – 最新バージョン | 3 | ページで Analytics コードライブラリの最新バージョンが実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。Analytics コードが web ページで見つからない場合は、0 を返します。 | 最新バージョンの Analytics ライブラリをインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) |
| Launch - DOM の準備が完了した後に、サードパーティタグが非同期で読み込まれる | 3 | 適切なユーザーエクスペリエンスと正確なデータの収集のバランスを取るには、DOM の準備が整った時点でサードパーティタグをトリガーする必要があります。 これにより、サイトの機能に影響を与えずに、これらのトラッキングスクリプトを確実に実行することができます。 | この問題を解決するには、DOM Ready で起動するようにサードパーティピクセルを実行するすべてのルールを調整します。<br><br>[追加情報](../../tags/ui/managing-resources/rules.md) |
| Experience Cloud ID サービス – 最新バージョン | 2 | ページで最新バージョンの訪問者 ID サービスコードライブラリ、visitorAPI.js が実行されていません。 Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | 最新バージョンの訪問者 ID サービスライブラリをインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/library.html?lang=ja) |
| Launch – 最新バージョン | 2 | これらのページでは、最新バージョンのタグコードライブラリ（Turbine）が実行されていません。 Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | タグライブラリを再構築して公開します。<br><br>[追加情報](../../tags/quick-start/quick-start.md) |
| Target – 最新バージョン | 2 | Target コードライブラリの最新バージョンがページで実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | 最新バージョンの Target ライブラリをインストールしてください。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/) |
| Target - mboxDefault は mboxCreate の前に配置されます | 5 | mboxCreate を適切に使用する方法は、<br><br> のようになります。 `<div class="mboxDefault"><!-Customer content--></div><script>mboxCreate('myMboxName')</script>` | mboxCreate （）を呼び出す前に必ず `<div class="mboxDefault"></div>` タグを含めてください。 at.js による追加はおこなわれません。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/) |
| ターゲット – 有効な DOCTYPE | 5 | 無効な DOCTYPE が検出されました。このシナリオでは、mbox は実行されません。  at.js の場合、DOCTYPE は標準モードである必要があります。そうしないと、Target は動作しません。 | ページ上の DOCTYPE を更新します。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/atjs/target-atjs-faq/) |

{style="table-layout:auto"}
