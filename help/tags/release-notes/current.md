---
title: タグとイベント転送のリリースノート
description: Adobe Experience Platform のタグおよびイベント転送に関する最新のリリースノート。
exl-id: 2ebeaa1e-64b8-48fd-b4e8-419663271a87
source-git-commit: 18599d223733cb151c7517abb77b1745d2e634b7
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 75%

---

# タグとイベント転送のリリースノート

## 2022年1月25日（PT）

* **新しいホーム画面**:データ収集 UI のホームページが更新され、生産性を向上させるための便利なオンボーディング情報やリンクが含まれるようになりました。 これには以下が含まれます。
   1. ドキュメントおよび推奨ワークフローを参照してください
   1. 最近使用したプロパティ、ルール、およびデータ要素
   1. 一般的な拡張機能
   1. クイックインストール機能による新しい拡張機能の更新
* **データ送信先 [!DNL Google Ads] イベント転送の使用**:これで、 [[!DNL Google Ads Enhanced Conversions] API 拡張機能](../extensions/server/google-ads-enhanced-conversions/overview.md) イベント転送の場合は、 [Google OAUTH 2 の秘密鍵](../ui/event-forwarding/secrets.md#google-oauth2)（サーバー側のデータをに安全に送信するため） [!DNL Google Ads] リアルタイムで。

## 2022年11月23日（PT）

* イベント転送用の **[!DNL AWS] 拡張機能**：[イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Amazon Web Services]（[!DNL AWS]）にデータを送信できるようになりました。詳しくは、[[!DNL AWS] 拡張機能の概要](../../tags/extensions/server/aws/overview.md)を参照してください。
* **[!DNL Google Ads Enhanced Conversions]イベント転送の拡張**:これで、コンバージョンデータを [!DNL Google Ads] の使用 [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL Google Ads Enhanced Conversions] 拡張機能の概要](../../tags/extensions/server/google-ads-enhanced-conversions/overview.md)を参照してください。
* **[!DNL Microsoft Azure]イベント転送の拡張**:これで、次にデータを送信できます： [!DNL Microsoft Azure] の使用 [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL Microsoft Azure] 拡張機能の概要](../../tags/extensions/server/azure/overview.md)を参照してください。

## 2022年10月26日（PT）

* **データストリームの機密データ処理**：データストリームでは、複数の Platform テクノロジーを活用して、HIPAA（Health Insurance Portability and Accountability Act）などの規制に従って機密データを適切に処理するようになりました。詳しくは、[データストリームでの機密データの処理](../../edge/datastreams/overview.md#sensitive)を参照してください。
*  イベント転送用の **[!DNL Splunk] 拡張機能**：[!DNL Splunk]イベント転送[拡張機能を使用して、](../ui/event-forwarding/overview.md) にデータを送信できるようになりました。詳しくは、[[!DNL Splunk] 拡張機能の概要](../extensions/server/splunk/overview.md)を参照してください。
* **[!DNL Zendesk]イベント転送の拡張**:これで、次にデータを送信できます： [!DNL Zendesk] の使用 [イベント転送](../ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL Zendesk] 拡張機能の概要](../extensions/server/zendesk/overview.md)を参照してください。

## 2022年9月28日（PT）

* **Adobe Experience Platform 左ナビゲーション統合**：以前はデータ収集 UI 専用であったすべての機能（タグ、イベント転送など）が、 Experience Platform UI の&#x200B;**[!UICONTROL データ収集]**&#x200B;カテゴリ下の左ナビゲーションからも利用できるようになりました。これにより、Platform でデータ収集機能を使用する際に、UI を切り替える必要がなくなります。
* **タグとイベント転送のユーザー属性**：タグとイベント転送で使用可能なプロパティをリストすると、リストされた各プロパティに最終更新日と更新者が表示されるようになりました。
* イベント転送用の **[[!DNL Snap Conversions API] 拡張機能](https://exchange.adobe.com/apps/ec/108550)**：[イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Snapchat Conversions API] にデータを送信できるようになりました。認証方法と API の使用方法について詳しくは、[[!DNL Snapchat Marketing API] ドキュメント](https://marketingapi.snapchat.com/docs/conversion.html)を参照してください。

## 2022年7月27日（PT）

* タグとイベント転送機能へのアクセスは、Adobe Admin Console の Adobe Experience Platform データ収集カードで管理されるようになりました。詳しくは、[データ収集の権限](../../collection/permissions.md)に関するガイドを参照してください。
* Internet Explorer 10 および 11 のサポートは[非推奨（廃止予定）](../ie-deprecation.md)になりました。

## 2022年6月22日（PT）

次の新しい拡張機能がリリースされました。

* [Google データレイヤータグ拡張機能](../extensions/client/google-data-layer/overview.md)：タグ実装で Google データレイヤーを使用できます。
* [Google 広告拡張コンバージョンイベント転送拡張機能](https://partners.adobe.com/exchangeprogram/experiencecloud/exchange.details.108630.html)：Google 広告のコンバージョンをリアルタイムで強化できます。
* [Mailchimp イベント転送拡張機能](../extensions/server/mailchimp/overview.md)：Mailchimp マーケティングキャンペーン、ジャーニーまたはトランザクション用のメールをトリガーできる Mailchimp Marketing API にイベントを送信します。