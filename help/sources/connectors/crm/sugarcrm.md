---
title: SugarCRM ソースの概要
description: API またはユーザーインターフェイスを使用して SugarCRM をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 666767d45ca24f1c3fbbc0265d5f5c26a904e4b2
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 43%

---

# （ベータ版）[!DNL SugarCRM]

>[!NOTE]
>
>[!DNL SugarCRM] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティの CRM アプリケーションからデータを取り込む機能を提供しています。 CRM プロバイダーのサポートは [!DNL SugarCRM] を含みます。

[[!DNL SugarCRM]](https://www.sugarcrm.com/) は、顧客関係管理 (CRM) システムです。 [!DNL SugarCRM]の機能には、営業活動の自動化、マーケティングキャンペーン、カスタマーサポート、コラボレーション、モバイル CRM、Social CRM、レポートなどが含まれます。

この [!DNL SugarCRM] ソースを使用すると、次の API エンドポイントからアカウント、連絡先およびイベントのデータを取り込むことができます。

* [アカウント](https://market.apidocs.sugarcrm.com/#b0aeb0cd-80ea-4688-8474-54e4873f32f3)
* [連絡先](https://market.apidocs.sugarcrm.com/#308c5025-9478-4de3-8a41-1fc3cff1d8d1)
* [イベント](https://market.apidocs.sugarcrm.com/#516ec3b1-8e70-43d4-8bf2-38a2ae74c0a5)


[!DNL SugarCRM] は、bearer トークンを認証メカニズムとして使用し、 [!DNL SugarCRM] アカウントおよび連絡先 API と [!DNL SugarCRM] イベント API。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件

事前に [!DNL SugarCRM] ソース接続を使用する場合は、まず次の点を確認する必要があります。

* A [!DNL SugarMarket] アカウント 次に連絡する必要があります： [!DNL SugarCRM] 有効なアカウントマネージャーを取得する [!DNL SugarMarket] アカウントを作成します。

* マーケティングまたは販売プロセスに関連付けられたユーザーアカウントとは別の一意の API ユーザー名およびアカウント。 この一意のユーザー名とアカウントの組み合わせには、API アクセス権限が必要です。 アカウントの設定手順について詳しくは、 [[!DNL SugarMarket RESTFUL API]](https://market.apidocs.sugarcrm.com/#intro) ドキュメント。

## [!DNL SugarCRM Accounts & Contacts] を Platform に接続する

* [ソース接続を作成し [!DNL SugarCRM Accounts & Contacts] 、API を使用して Platform にデータを取得します](../../tutorials/api/create/crm/sugarcrm-accounts-contacts.md)。
* [ソース接続を作成して [!DNL SugarCRM Accounts & Contacts] ユーザーインターフェイスを使用した Platform へのデータの取得](../../tutorials/ui/create/crm/sugarcrm-accounts-contacts.md).
* [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)


## [!DNL SugarCRM Events] を Platform に接続する

* [ソース接続を作成し [!DNL SugarCRM Events] 、API を使用して Platform にデータを取得します](../../tutorials/api/create/crm/sugarcrm-events.md)。
* [ソース接続を作成して [!DNL SugarCRM Events] ユーザーインターフェイスを使用した Platform へのデータの取得](../../tutorials/ui/create/crm/sugarcrm-events.md).
* [UI での CRM ソース接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
