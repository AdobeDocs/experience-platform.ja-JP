---
title: タグの有無のテストリファレンス
description: auditor 機能がAdobe Experience Platform Debuggerでタグの有無をテストする方法について説明します。
exl-id: 8f01f89e-2a3b-41bc-b971-f3c60d0ae3fa
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 17%

---

# タグの有無に関するテストリファレンス

このリファレンスでは、Adobe Experience Platform Debuggerの auditor 機能によるタグの有無のテスト方法について詳しく説明します。

>[!NOTE]
>
>Experience Platform Debugger の auditor テストについて詳しくは、[auditor 機能の概要 &#x200B;](./overview.md) を参照してください。

タグの有無テストは、特定のタグがページに存在するかどうか、およびページのコード内でそれらが適切な場所にあるかどうかを評価します。

| テスト | 重み | 条件 | レコメンデーション |
| --- | --- | --- | --- |
| Advertising Cloud - コードプレゼンス | 5 | Advertising Cloud タグは DOM では使用できません。 | [Advertising Cloud タグ拡張機能 &#x200B;](../../destinations/catalog/advertising/adobe-advertising-cloud.md) を使用して、Advertising Cloud タグを実装します。 |
| Advertising Cloud - セグメントピクセルが実装されました | 5 | Advertising Cloud セグメントピクセルを新しい Advertising Cloud の画像専用コンバージョンタグにアップグレードしてください。非推奨の AMO セグメントタグを使用すると、データが失われる可能性があります。 | [Advertising Cloud タグ拡張機能 &#x200B;](../../destinations/catalog/advertising/adobe-advertising-cloud.md) を使用して、Advertising Cloud セグメントのピクセルを実装します。 |
| Analytics - DOM にロード済み | 5 | Adobe Analytics タグが検出されませんでした。 | 最新バージョンの Analytics をインストールします。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=ja) |
| Launch - ライブラリが読み込まれました | 5 | `global _satellite` オブジェクトが DOM に見つかりませんでした。つまり、タグライブラリがインストールされていないか、実行に失敗しています。 | タグライブラリがページに実装されており、後続のスクリプトアクティビティでブロックされていないことを確認します。 |
| Launch – 複数の埋め込みスクリプトを持たない | 5 | 実稼動サイトでは、ページごとに 1 つの埋め込みコードのみを読み込む必要があります。 | 実稼動ライブラリのみがページに読み込まれていることを確認してください。 |
| Launch - `pageBottom` コールバックが `<body>` に存在する | 5 | 必要な `_satellite.pageBottom()` コールバックがページの `<body>` 内に見つかりませんでした。 このテストは、`pageBottom` 呼び出しがページでまったく見つからない場合、または `<head>` タグ（または他の予期しない場所）にある場合は失敗します。 `<body>` タグ内のどこか `pageBottom` 見つかった場合にのみ渡されます。 | タグが適切に機能するように、`</body>` タグを閉じる直前にインラインスクリプトを追加します。<br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Launch – 非同期 `pageBottom` プロイする場合、コールバックは存在できません | 5 | ページで `_satellite.pageBottom()` コールバックが見つかりました。これは、タグが非同期でデプロイされる場合には当てはまりません。 | `_satellite.pageBottom()` スクリプトを削除して、適切なタグ機能を有効にします。 <br><br>[追加情報](../../tags/ui/client-side/asynchronous-deployment.md) |
| Experience Cloud ID サービス – コードの有無 | 5 | Experience Cloud ID サービスコードが見つかりませんでした。Experience Cloud ソリューションから最大限の価値を引き出すために、Experience Cloud ID （ECID）の使用を強くお勧めします。これは、Experience Cloud ソリューション全体の ID 管理にとって重要です。 | 最新バージョンの ECID をインストールします。<br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) |
| Experience Cloud ID サービス - Cookie の存在 | 5 | `AMCV_` の Cookie が見つかりませんでした。 `VisitorAPI.js` コードから訪問者オブジェクトをインスタンス化する必要があります。 | タグ実装の場合は、AdobeOrg ID が ECID ツールに正しく入力されていることを確認します。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=ja) |
| Experience Cloud ID サービス - MID 値が存在する | 5 | MID 値が `AMCV_` Cookie に見つかりませんでした。 | もう一度テストして、ECID API の待ち時間を確認します。 問題が解決しない場合は、Adobe カスタマーケアにお問い合わせください。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html?lang=ja) |
| Target - コードプレゼンス | 5 | Adobe Targetは DOM で定義する必要があります。 | Target （at.js）の最新バージョンをインストールします。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html?lang=ja) |
| Target - `<head>` に読み込まれたライブラリ | 4 | Target ライブラリを `<head>` タグに読み込む必要があります。 | Target ライブラリが `<head>` タグに読み込まれていることを確認してください。 <br><br>[追加情報](https://experienceleague.adobe.com/docs/target/using/implement-target/implementing-target.html?lang=ja) |

{style="table-layout:auto"}
