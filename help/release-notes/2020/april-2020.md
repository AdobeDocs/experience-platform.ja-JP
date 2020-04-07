---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年4月9日）
doc-type: release notes
last-update: March 4, 2020
author: ens71067
translation-type: tm+mt
source-git-commit: b3ee2839412c9949d67c2ae976e3df32fea7731e

---


# Adobe Experience Platform リリースノート

## リリース日：2020 年 4 月 8 日

## プライバシーサービス

新しい法規制や組織の規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり、個人データを削除したりする権利が与えられます。 Adobe Experience Platform Privacy Serviceは、RESTful APIとユーザーインターフェイスを提供し、顧客からのこれらのデータリクエストを管理するのに役立ちます。 プライバシーサービスを使用すると、Adobe Experience Cloudアプリケーションから個人または個人の顧客データにアクセスする要求を送信したり、データを削除したりでき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| --- | --- |
| PDPAのサポート | タイの個人データ保護法(PDPA)に基づいて、プライバシー要求を作成し、追跡できるようになりました。 APIでプライバシーリクエストを行う場合、 `regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UIの名前空間タイプ | プライバシーサービスのUIのリクエスト名前空間で様々な種類のプライバシーを指定できるようになりました。 詳しくは、ユー [ザガイド](../../privacy-service/ui/user-guide.md) を参照してください。 |
| 古いエンドポイントの廃止 | 古いAPIエンドポイント(`data/privacy/gdpr`)は非推奨となりました。 |

既知の問題

* None

プライバシーサービスの詳細については、開始サービスの概要を [参照してください](../../privacy-service/home.md)。

## ソース

Adobe Experience Platformは、外部ソースからデータを取り込み、Platformサービスを使用してデータの構造、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、お使いのCRMシステムなど、様々なソースからデータを取り込むことができます。

エクスペリエンスプラットフォームは、様々なデータプロバイダーのソース接続を簡単に設定できるRESTful APIとインタラクティブUIを提供します。 これらのソース接続を使用すると、外部ストレージシステムおよびCRMサービスの認証と接続、取り込みの実行時間の設定、データ取り込みのスループットの管理を行うことができます。

### 新機能

| 機能 | 説明 |
| ------- | ----------- |
| データベースのAPIとUIのサポート | Apache Spark(HDInsights)、Azure Synapse Analytics、Azure Tableストレージ、Hive(HDInsights)およびPhoenix用の新しいソースコネクタ。 |
| 支払いベースのアプリケーションのAPIとUIのサポート | PayPal用の新しいソースコネクタ。 |
| プロトコルベースのアプリケーションのAPIとUIのサポート | 汎用OData用の新しいソースコネクタ。 |

### 既知の問題

* None

For more information about sources, see the [sources overview](../../source-connectors/home.md).

<!-- ## Access control

Experience Platform leverages [Adobe Admin Console](https://adminconsole.adobe.com) product profiles to link users with permissions and sandboxes. Permissions control access to a variety of Platform capabilities, including data modeling, profile management, and sandbox administration.

### Key features

|Feature | Description|
|--- | ---|
|Permissions | In the Admin Console, the _Permissions_ tab within a Platform product profile allows you customize which Platform capabilities are available for the users attached to that profile. Available permission categories include: Data Modeling, Data Management, Profile Management, Identities, Data Monitoring, Sandbox Administration, Destinations, Sources.|
|Access to sandboxes | The _Permissions_ tab within a Platform product profile can grant users access to specific sandboxes. See the section on [sandboxes](#sandboxes) below for more information.|

For more information, please see the [access control overview](../../access-control/home.md).

## Sandboxes

Experience Platform is built to enrich digital experience applications on a global scale. Companies often run multiple digital experience applications in parallel and need to cater for the development, testing, and deployment of these applications while ensuring operational compliance. In order to address this need, Experience Platform provides sandboxes which partition a single Platform instance into separate virtual environments to help develop and evolve digital experience applications.

### Key features

|Feature | Description|
|--- | ---|
|Production sandbox | Experience Platform provides a single production sandbox, which cannot be deleted or reset.|
|Non-production sandboxes | Multiple non-production sandboxes can be created for a single Platform instance, allowing you to test features, run experiments, and make custom configurations without impacting your production sandbox.|
|Sandbox switcher | In the Experience Platform user interface, the sandbox switcher in the top-left corner of the screen allows you to switch between available sandboxes through a dropdown menu.|
|`x-sandbox-name` header | All calls to Experience Platform APIs must now include the new `x-sandbox-name` header, whose value references the `name` attribute of the sandbox the operation will take place in.|

For more information, please see the [sandboxes overview](../../sandboxes/home.md). -->