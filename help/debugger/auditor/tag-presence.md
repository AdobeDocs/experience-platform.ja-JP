---
title: タグの有無のテストリファレンス
description: Adobe Experience Platform Debugger でのタグの有無をテストする Auditor の機能について説明します。
exl-id: 8f01f89e-2a3b-41bc-b971-f3c60d0ae3fa
source-git-commit: 10a5605c40143b58f6ba0108cc087956aa929866
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 34%

---

# タグの有無のテストに関するリファレンス

このリファレンスでは、Adobe Experience Platform Debugger の Auditor 機能がタグの有無をテストする方法の詳細を説明します。

>[!NOTE]
>
>Platform Debugger での監査テストについて詳しくは、 [auditor 機能の概要](./overview.md).

タグの有無テストでは、特定のタグがページ上に存在するかどうか、およびタグがページコード内の適切な場所に配置されているかどうかを評価します。

| テスト | 重み付け | 条件 | 推奨 |
| --- | --- | --- | --- |
| Advertising Cloud - コードの有無 | 5 | Advertising Cloud タグは DOM では使用できません。 | を使用したAdvertising Cloudタグの実装 [Advertising Cloudタグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md). |
| Advertising Cloud - セグメントピクセルが実装されている | 5 | Advertising Cloud セグメントピクセルを新しい Advertising Cloud の画像専用コンバージョンタグにアップグレードしてください。非推奨の AMO セグメントタグを使用すると、データが失われる可能性があります。 | を使用したAdvertising Cloudセグメントピクセルの実装 [Advertising Cloudタグ拡張機能](../../destinations/catalog/advertising/adobe-advertising-cloud.md). |
| Analytics - DOM に読み込まれている | 5 | Adobe Analytics タグが検出されませんでした。 | 最新バージョンの Analytics をインストールしてください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Launch - ライブラリが読み込まれている | 5 | A `global _satellite` オブジェクトが DOM 内に見つかりませんでした。つまり、タグライブラリがインストールされていないか、実行に失敗します。 | ページでタグライブラリが実装され、後続のスクリプトアクティビティによってブロックされていないことを確認します。 |
| Launch - 複数の埋め込みスクリプトがない | 5 | 実稼動サイトでは、ページごとに 1 つの埋め込みコードのみを読み込みます。 | 実稼動ライブラリのみがページに読み込まれていることを確認してください。 |
| 起動 — `pageBottom` コールバックが次に存在する： `<body>` | 5 | 必須 `_satellite.pageBottom()` 内にコールバックが見つかりませんでした `<body>` 」と入力します。 このテストは、 `pageBottom` の呼び出しがページで見つからない、またはページ内にある場合は `<head>` タグ（または他の予期しない場所）に貼り付けます。 次の場合にのみ通過します。 `pageBottom` が `<body>` タグを使用します。 | 終了の直前にインラインスクリプトを追加する `</body>` タグを使用して、適切なタグ機能を確保します。<br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| 起動 — `pageBottom` 非同期でデプロイした場合はコールバックを存在させない | 5 | この `_satellite.pageBottom()` コールバックがページで見つかりました。タグが非同期でデプロイされている場合は、このようにはなりません。 | を削除します。 `_satellite.pageBottom()` スクリプトを使用して、適切なタグ機能を有効にします。 <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Experience Cloud ID サービス - コードの有無 | 5 | Experience Cloud ID サービスコードが見つかりませんでした。Experience CloudID(ECID) の使用は、Experience Cloudソリューションから最大限の価値を引き出すために強く推奨され、Experience Cloudソリューション全体の ID 管理にとって重要です。 | 最新バージョンの ECID をインストールします。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) |
| Experience Cloud ID サービス - Cookie の有無 | 5 | この `AMCV_` cookie が見つかりませんでした。 訪問者オブジェクトは、`VisitorAPI.js` コードからインスタンス化する必要があります。 | これがタグ実装の場合は、AdobeOrg ID が ECID ツールに正しく入力されていることを確認します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=ja) |
| Experience Cloud ID サービス - MID 値が存在する | 5 | MID 値がに見つかりませんでした `AMCV_` cookie. | もう一度テストして、ECID API の遅延を確認してください。 問題が解決しない場合は、アドビカスタマーケアにお問い合わせください。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=ja) |
| Target - コードの有無 | 5 | Adobe Targetは DOM で定義する必要があります。 | 最新バージョンの Target（at.js）をインストールします。<br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html) |
| Target — でライブラリが読み込まれました `<head>` | 4 | Target ライブラリは、 `<head>` タグを使用します。 | Target ライブラリが `<head>` タグを使用します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html) |

{style="table-layout:auto"}
