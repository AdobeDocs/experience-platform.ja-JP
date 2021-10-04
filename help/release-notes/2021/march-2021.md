---
title: Adobe Experience Platform リリースノート
description: 2021 年 3 月 31 日（PT）の Experience Platform リリースノート。
doc-type: release notes
last-update: March 31, 2021
author: ens72741
exl-id: 027cd7b1-1651-4939-bc97-968a41824117
source-git-commit: 0cbd5a933f8c67b26051012e9a5aa78cb06b055d
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 3 月 31 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| `add_to_array` 関数 | 配列をパラメーターとしてサポートする機能を更新しました。 |
| `to_array` 関数 | オブジェクトをパラメーターとしてサポートするよう機能を更新しました。 |

詳しくは、[[!DNL Data Prep]  の概要](../../data-prep/home.md)を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| （ベータ版）エッジのセグメント化 | エッジのセグメント化では、セグメントをリアルタイムに評価するので、同じページおよび次のページのパーソナライゼーションの使用例を考慮できます。エッジのセグメント化について詳しくは、[セグメント化 UI の概要](../../segmentation/ui/overview.md)を参照してください。 |
| （ベータ版）増分セグメント化 | バッチセグメント化で評価される既存のセグメント定義の鮮度を最大 1 時間に増やします。 |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL MySQL]](../../sources/connectors/databases/mysql.md)</li><li>[[!DNL PostGres]](../../sources/connectors/databases/postgres.md)</li><li>[[!DNL Salesforce Service Cloud]](../../sources/connectors/customer-success/salesforce-service-cloud.md)</li><li>[[!DNL SFTP]](../../sources/connectors/cloud-storage/sftp.md)</li><li>[[!DNL Shopify]](../../sources/connectors/ecommerce/shopify.md)</li></ul> |
| 圧縮ファイル取り込みの API のサポート | クラウドストレージソースを使用して、圧縮された JSON または区切り形式のファイルのプレビューや取り込みが可能になりました。詳しくは、[API を使用したクラウドストレージデータの収集](../../sources/tutorials/api/collect/cloud-storage.md)に関するチュートリアルを参照してください。 |
| 再帰的なファイルアップロードに対する UI のサポート | クラウドストレージソースを使用する際に、フォルダー全体を再帰的に取り込めるようになりました。フォルダー全体を取り込む場合は、そのフォルダーのコンテンツのスキーマが同じであることを確認する必要があります。詳しくは、[UI でのクラウドストレージコネクタ用のデータフローの設定](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md)に関するチュートリアルを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
