---
title: SugarCRM Sourceの概要
description: API またはユーザーインターフェイスを使用して SugarCRM をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2023-08-23T00:00:00Z
exl-id: 03fbc4e9-974d-494e-8463-756c96665fd5
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 28%

---

# [!DNL SugarCRM]

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティの CRM アプリケーションからデータを取り込む機能を備えています。 CRM プロバイダーのサポートは [!DNL SugarCRM] を含みます。

[[!DNL SugarCRM]](https://www.sugarcrm.com/) は、顧客関係管理（CRM）システムです。 [!DNL SugarCRM] の機能には、営業部隊の自動化、マーケティングキャンペーン、カスタマーサポート、コラボレーション、モバイル CRM、ソーシャル CRM、レポートなどが含まれます。

[!DNL SugarCRM] ソースを使用すると、次の API エンドポイントからアカウント、連絡先、イベントデータを取り込むことができます。

* [アカウント](https://market.apidocs.sugarcrm.com/#b0aeb0cd-80ea-4688-8474-54e4873f32f3)
* [ 連絡先 ](https://market.apidocs.sugarcrm.com/#308c5025-9478-4de3-8a41-1fc3cff1d8d1)
* [イベント](https://market.apidocs.sugarcrm.com/#516ec3b1-8e70-43d4-8bf2-38a2ae74c0a5)

[!DNL SugarCRM] は、認証メカニズムとしてベアラートークンを使用して、[!DNL SugarCRM] アカウントおよび連絡先 API や [!DNL SugarCRM] Events API と通信します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件

[!DNL SugarCRM] ソース接続を作成する前に、次の点を確認する必要があります。

* [!DNL SugarMarket] アカウント。 有効な [!DNL SugarMarket] アカウントをお持ちでない場合は、[!DNL SugarCRM] アカウントマネージャーに問い合わせて、有効なアカウントを取得する必要があります。

* マーケティングまたは販売プロセスに関連付けられた任意のユーザーアカウントとは別の一意の API ユーザー名およびアカウント。 この一意のユーザー名とアカウントの組み合わせには、API アクセス権限が必要です。 アカウント設定プロセスについて詳しくは、[[!DNL SugarMarket RESTFUL API]](https://market.apidocs.sugarcrm.com/#intro) ドキュメントを参照してください。

## [!DNL SugarCRM Accounts & Contacts] をExperience Platformに接続

* [API を使用してデータをExperience Platformに取り込む  [!DNL SugarCRM Accounts & Contacts]  めのソース接続を作成します ](../../tutorials/api/create/crm/sugarcrm-accounts-contacts.md)。
* [ ユーザーインターフェイスを使用してデータをExperience Platformに取り込むためのソ  [!DNL SugarCRM Accounts & Contacts]  ス接続を作成します ](../../tutorials/ui/create/crm/sugarcrm-accounts-contacts.md)。
* [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)


## [!DNL SugarCRM Events] をExperience Platformに接続

* [API を使用してデータをExperience Platformに取り込む  [!DNL SugarCRM Events]  めのソース接続を作成します ](../../tutorials/ui/create/crm/sugarcrm-events.md)。
* [ ユーザーインターフェイスを使用してデータをExperience Platformに取り込むためのソ  [!DNL SugarCRM Events]  ス接続を作成します ](../../tutorials/ui/create/crm/sugarcrm-events.md)。
* [UI での CRM ソース接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
