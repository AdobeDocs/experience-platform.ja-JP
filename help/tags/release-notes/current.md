---
title: タグとイベント転送のリリースノート
description: Adobe Experience Platform のタグおよびイベント転送に関する最新のリリースノート。
exl-id: 2ebeaa1e-64b8-48fd-b4e8-419663271a87
source-git-commit: f2f2f9abc50f2016e41fd23bfbb66553fadf6fce
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 74%

---

# タグとイベント転送のリリースノート

## 2023年3月29日（PT）

**クイックスタークワークフロー（ベータ版）**

データコレクションのホーム画面の「はじめに」の新しいクイックスタートワークフローにアクセスできます。 次のワークフローが、パブリックベータ版としてお客様が利用できるようになりました。
* **[メタ変換 API](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/meta/overview.html?lang=en#quick-start)**:イベント転送のお客様は、わずか数手順で、広告コンバージョン用にサーバー側から Meta へ迅速にイベントデータを収集し、転送できます。
* **[モバイル SDK](https://developer.adobe.com/client-sdks/documentation/)**:お客様は、わずか数の簡単な手順で、Mobile SDK を迅速に実装し、基本的なモバイルイベントを検証できます。

次の新しい拡張機能がリリースされました。

* **[!DNL Braze]イベント転送拡張機能**:この [[!DNL Braze Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html) イベント転送拡張機能を使用すると、Adobe Experience Platform Edge Network で取り込んだデータを活用し、に送信できます。 [!DNL Braze] を使用して、サーバー側のイベントの形式で [!DNL Braze] ユーザー追跡 API
* **[!DNL Mixpanel]イベント転送拡張機能**:この [[!DNL Mixpanel Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html) 拡張機能を使用すると、イベント転送を活用してAdobe Experience Platform Edge Network でイベント情報をキャプチャし、イベント追跡 API を使用して Mixpanel に送信できます。

## 2023年1月25日（PT）

* **新しいホーム画面**：データ収集 UI のホームページが更新されて、生産性を向上させるために役立つオンボーディング情報やリンクが含まれるようになりました。 これには以下が含まれます。
   1. 基本を学ぶためのドキュメントと推奨されるワークフロー
   1. 最近のプロパティ、ルールおよびデータ要素
   1. 人気の高い拡張機能
   1. クイックインストール機能を備えた新しい拡張機能のアップデート
* **イベント転送を使用した [!DNL Google Ads] へのデータ送信**：[[!DNL Google Ads Enhanced Conversions] API 拡張機能](../extensions/server/google-ads-enhanced-conversions/overview.md)を [Google OAuth2 シークレット](../ui/event-forwarding/secrets.md#google-oauth2)と組み合わせてイベント転送に使用して、サーバーサイドデータを [!DNL Google Ads] にリアルタイムで安全に送信できるようになりました。

## 2022年11月23日（PT）

* イベント転送用の **[!DNL AWS] 拡張機能**：[イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Amazon Web Services]（[!DNL AWS]）にデータを送信できるようになりました。詳しくは、[[!DNL AWS] 拡張機能の概要](../../tags/extensions/server/aws/overview.md)を参照してください。
* **[!DNL Google Ads Enhanced Conversions]のイベント転送用拡張機能**：[イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Google Ads] にコンバージョンデータを送信できるようになりました。詳しくは、[[!DNL Google Ads Enhanced Conversions] 拡張機能の概要](../../tags/extensions/server/google-ads-enhanced-conversions/overview.md)を参照してください。
* **[!DNL Microsoft Azure]のイベント転送用拡張機能**：[イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Microsoft Azure] にデータを送信できるようになりました。詳しくは、[[!DNL Microsoft Azure] 拡張機能の概要](../../tags/extensions/server/azure/overview.md)を参照してください。

## 2022年10月26日（PT）

* **データストリームの機密データ処理**：データストリームでは、複数の Platform テクノロジーを活用して、HIPAA（Health Insurance Portability and Accountability Act）などの規制に従って機密データを適切に処理するようになりました。詳しくは、[データストリームでの機密データの処理](../../edge/datastreams/overview.md#sensitive)を参照してください。
* **[!DNL Splunk]のイベント転送用拡張機能**：[イベント転送](../ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Splunk] にデータを送信できるようになりました。詳しくは、[[!DNL Splunk] 拡張機能の概要](../extensions/server/splunk/overview.md)を参照してください。
* **[!DNL Zendesk]のイベント転送用拡張機能**：[イベント転送](../ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Zendesk] にデータを送信できるようになりました。詳しくは、[[!DNL Zendesk] 拡張機能の概要](../extensions/server/zendesk/overview.md)を参照してください。

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