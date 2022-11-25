---
title: タグとイベント転送のリリースノート
description: Adobe Experience Platform のタグおよびイベント転送に関する最新のリリースノート。
exl-id: 2ebeaa1e-64b8-48fd-b4e8-419663271a87
source-git-commit: c7344d0ac5b65c6abae6a040304f27dc7cd77cbb
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 30%

---

# タグとイベント転送のリリースノート

## 2022年10月26日（PT）

* **データストリームの機密データ処理**:データストリームは、複数の Platform テクノロジーを活用して、HIPAA(Health Insurance Portability and Accountability Act) などの規制によって適用される機密データを適切に処理するようになりました。 詳しくは、[データストリームでの機密データの処理](../../edge/datastreams/overview.md#sensitive)を参照してください。
*  イベント転送用の **[!DNL Splunk]拡張機能**:これで、次にデータを送信できます： [!DNL Splunk] の使用 [イベント転送](../ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL Splunk] 拡張機能の概要](../extensions/server/splunk/overview.md)を参照してください。
*  イベント転送用の **[!DNL Zendesk]拡張機能**:これで、次にデータを送信できます： [!DNL Zendesk] の使用 [イベント転送](../ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL Zendesk] 拡張機能の概要](../extensions/server/zendesk/overview.md)を参照してください。

## 2022年9月28日（PT）

* **Adobe Experience Platform左ナビゲーション統合**:以前はデータ収集 UI にのみ使用できたすべての機能（タグやイベントの転送を含む）も、Experience PlatformUI の左側のナビゲーションから「 」カテゴリで使用できるようになりました。 **[!UICONTROL データ収集]**. これにより、Platform でデータ収集機能を使用する際に、UI を切り替える必要がなくなります。
* **タグとイベント転送のユーザー属性**:タグおよびイベント転送で使用可能なプロパティをリストする場合、リストされた各プロパティが最後に更新された日時と更新者を表示するようになりました。
* **[[!DNL Snap Conversions API] 拡張](https://exchange.adobe.com/apps/ec/108550) イベント転送の場合**:これで、 [!DNL Snapchat Conversions API] の使用 [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 認証方法と API の使用方法について詳しくは、[[!DNL Snapchat Marketing API] ドキュメント](https://marketingapi.snapchat.com/docs/conversion.html)を参照してください。

## 2022年7月27日（PT）

* タグおよびイベント転送機能へのアクセスは、Adobe Experience Platformデータ収集用カードのAdobe Admin Consoleで管理されるようになりました。 詳しくは、[データ収集の権限](../../collection/permissions.md)に関するガイドを参照してください。
* Internet Explorer 10 および 11 のサポートが追加されました。 [非推奨](../ie-deprecation.md).

## 2022年6月22日（PT）

新しい拡張機能がリリースされました。

* [Google Data Layer タグ拡張機能](../extensions/client/google-data-layer/overview.md):タグ実装でGoogleデータレイヤーを使用できます。
* [Google Ads 拡張コンバージョンイベント転送拡張機能](https://partners.adobe.com/exchangeprogram/experiencecloud/exchange.details.108630.html):Google Ads のコンバージョンをリアルタイムで強化できます。
* [Mailchimp イベント転送拡張機能](../extensions/server/mailchimp/overview.md):Mailchimp マーケティング API にイベントを送信します。この API は、Mailchimp マーケティングキャンペーン、ジャーニー、トランザクション用のメールをトリガーにすることができます。
