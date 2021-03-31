---
title: Adobe Experience Platform リリースノート
description: 2021年3月31日Experience Platformリリースノート
doc-type: release notes
last-update: March 31, 2021
author: ens72741
translation-type: tm+mt
source-git-commit: 58382528cc787e8d2005c8c322904266880ad0b9
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 42%

---


# Adobe Experience Platform リリースノート

**リリース日：2021年3月31日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Sandboxes]](#sandboxes)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] データエンジニアがエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行えるようにします。

| 機能 | 説明 |
| ------- | ----------- |
| `add_to_array` 関数 | パラメータとして配列をサポートする機能を更新。 |
| `to_array` 関数 | オブジェクトをパラメーターとしてサポートする機能を更新しました。 |

詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Sandboxes] {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

このニーズに対応するために、Experience Platform は、サンドボックスを提供します。サンドボックスでは、単一の Platform インスタンスを別々の仮想環境に分割することができ、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。

| 機能 | 説明 |
| ------- | ----------- |
| （ベータ版）複数の実稼働用サンドボックス | IMS組織で複数の実稼働サンドボックスを作成および管理でき、特定の実稼働サンドボックスを特定の業種、ブランド、プロジェクトまたは地域に対して専用にすることができるようになりました。 詳しくは、UI](../../sandboxes/ui/user-guide.md)または[でAPI](../../sandboxes/api/create-sandbox.md)を使用した実稼働用サンドボックス[の作成に関するチュートリアルを参照してください。 |

サンドボックスについて詳しくは、「[サンドボックスの概要](../../sandboxes/home.md)」を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platformセグメントサービスは、セグメントを作成して[!DNL Real-time Customer Profile]データからオーディエンスを生成するためのユーザーインターフェイスおよびRESTful APIを提供します。 これらのセグメントは[!DNL Platform]上で一元的に構成および管理され、どのAdobeアプリケーションでも容易にアクセスできます。

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| （ベータ版）エッジセグメント | エッジセグメント化では、セグメントをリアルタイムで評価するので、同じページと次のページのパーソナライゼーションで使用できます。 エッジセグメントについて詳しくは、[セグメントUIの概要](../../segmentation/ui/overview.md)を参照してください。 |
| （ベータ版）増分セグメント | バッチセグメントで評価される既存のセグメント定義の有効期限が1時間まで増加します。 |

[!DNL Segmentation Service]について詳しくは、[セグメントの概要](../../segmentation/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform には、様々なデータプロバイダーへのソース接続を簡単に設定できる RESTful API とインタラクティブ UI が用意されています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| ベータ版ソースをGAに移行 | 以下の資料はベータ版からGA版に進められている。 <ul><li>[[!DNL MySQL]](../../sources/connectors/databases/mysql.md)</li><li>[[!DNL PostGres]](../../sources/connectors/databases/postgres.md)</li><li>[[!DNL Salesforce Service Cloud]](../../sources/connectors/customer-success/salesforce-service-cloud.md)</li><li>[[!DNL SFTP]](../../sources/connectors/cloud-storage/sftp.md)</li><li>[[!DNL Shopify]](../../sources/connectors/ecommerce/shopify.md)</li></ul> |
| 圧縮ファイル取り込みのためのAPIのサポート | クラウドストレージソースを使用して、圧縮されたJSONまたは区切り形式のファイルをプレビューおよび取り込めるようになりました。 詳しくは、[API](../../sources/tutorials/api/collect/cloud-storage.md)を使用したクラウドストレージデータの収集に関するチュートリアルを参照してください。 |
| 再帰的なファイルのアップロードに対するUIのサポート | クラウドストレージソースを使用する場合に、フォルダー全体を再帰的に取り込めるようになりました。 フォルダー全体を取り込む場合は、そのコンテンツが同じスキーマーを共有していることを確認する必要があります。 詳しくは、UI](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md)で、[クラウドストレージコネクタ用のデータフローの設定に関するチュートリアルを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
