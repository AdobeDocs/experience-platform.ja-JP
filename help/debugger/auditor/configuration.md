---
title: 構成テストのリファレンス
description: Adobe Experience Platform Debuggerの設定に対する監査機能のテスト方法について説明します。
exl-id: 92b07224-57f1-4891-9923-aa079945e6bc
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 50%

---

# 構成テストのリファレンス

このリファレンスでは、Adobe Experience Platform Debuggerの監査機能がコンフィギュレーションテストを実行する方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debuggerでの監査テストについて詳しくは、[監査機能の概要](./overview.md)を参照してください。

構成テストでは、実装内の特定の設定や値、または競合の可能性をスキャンします。Experience Platform Auditorは、タグを他のルールと比較し、推奨されるベストプラクティスと比較して評価します。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Adobe Advertising - コンバージョン名は英数字のみを使用します | 3 | `ev_conversion_property_name` パラメーターには、テキストまたは数値を含めることができる`ev_transid` パラメーターを除いて、数値と小数点以下の値のみを含める必要があります。 `everesttech.net`で始まるURL パラメーターを含む`ev_` ピクセルを探します。 | トランザクションプロパティのパラメーターには、数値と小数値のみを含めてください。<br><br>警告：その他の値タイプでは、データが失われる可能性があります。 |
| Adobe Advertising - コンバージョン名はURL セーフ文字を使用します | 3 | コンバージョンプロパティ名に、アンパサンドや疑問符を含めることはできません。 | トランザクションプロパティのパラメーターに、エンコードされていないアンパサンドや疑問符が含まれていないことを確認してください。これらがあると、URL 形式が壊れます。<br><br>警告：エンコードされていないアンパサンドまたは疑問符を含むプロパティ パラメーター（例：`ev_formComplete?=1`または`ev_formComplete&Submit=1`）は、データが失われる可能性があります。 |
| Adobe Advertising - トランザクション IDが正しく実装されている | 1 | プロパティ名`ev_transid=`は空にできません。 | プロパティ名`ev_transid=`を値なしで残すことはできません。 値が指定されていない場合、トランザクションデータが失われる可能性があります。値を`ev_transid=`に割り当てるか、ピクセルからパラメーターを削除します。 |
| Analytics - DOMでインスタンス化 | 5 | Adobe Analytics コードがインストールされていないか、実行に失敗します。Web ページにAnalytics コードが見つからない場合は0を返します。 | ページで Analytics タグが実装され、後続のスクリプトアクティビティによってブロックされていないことを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Analytics - インスタンス化は1回 | 5 | Adobe Analytics コードがページで複数回検出されました。Web ページにA-Analytics コードが見つからない場合は0を返します。 | ページ上の Analytics タグが 1 つだけであることを確認します。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Analytics – 最新バージョン | 3 | ページで Analytics コードライブラリの最新バージョンが実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。Web ページにAnalytics コードが見つからない場合は0を返します。 | 最新バージョンの Analytics ライブラリをインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) |
| Launch - DOMの準備後にサードパーティ製タグが非同期で読み込まれます | 3 | 優れたユーザーエクスペリエンスと正確なデータ収集のバランスを取るには、DOMの準備が整った状態でサードパーティタグをトリガーする必要があります。 これにより、サイトの機能に影響を与えずに、これらのトラッキングスクリプトを確実に実行することができます。 | この問題を解決するには、サードパーティピクセルを実行するすべてのルールを調整して、DOM準備状態で起動します。<br><br>[追加情報](../../tags/ui/managing-resources/rules.md) |
| Experience Cloud ID サービス – 最新バージョン | 2 | ページで最新バージョンの訪問者ID サービスコードライブラリであるvisitorAPI.jsが実行されていません。 Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | 最新バージョンの訪問者 ID サービスライブラリをインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/library.html) |
| Launch – 最新バージョン | 2 | これらのページでは、最新バージョンのタグコードライブラリ（Turbine）が実行されていません。 Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | タグライブラリを再構築して公開します。<br><br>[追加情報](../../tags/quick-start/quick-start.md) |
| Target – 最新バージョン | 2 | Target コードライブラリの最新バージョンがページで実行されていません。Experience Cloud テクノロジーの土台となるコードライブラリは、パフォーマンスの向上を活用し、最新の機能を提供できるよう、常に更新および調整されています。 | 最新バージョンの Target ライブラリをインストールしてください。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/) |
| Target - mboxDefaultはmboxCreateの前にあります | 5 | mboxCreateの適切な使用は、次のようになります。<br><br> `<div class="mboxDefault"><!-Customer content--></div><script>mboxCreate('myMboxName')</script>` | mboxCreate （）を呼び出す前に、必ず`<div class="mboxDefault"></div>` タグを含めてください。 at.js による追加はおこなわれません。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/) |
| Target – 有効なDOCTYPE | 5 | 無効な DOCTYPE が検出されました。このシナリオではmboxは起動されません。  at.js の場合、DOCTYPE は標準モードである必要があります。そうしないと、Target は動作しません。 | ページ上の DOCTYPE を更新します。<br><br>[追加情報](https://developer.adobe.com/target/implement/client-side/atjs/target-atjs-faq/) |

{style="table-layout:auto"}
