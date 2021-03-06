---
keywords: Experience Platform；トラブルシューティング；ガードレール；ガイドライン；
title: データ取り込みのガードレール
description: このドキュメントでは、Adobe Experience Platformでのデータ取り込みのガードレールに関するガイダンスを提供します
exl-id: f07751cb-f9d3-49ab-bda6-8e6fec59c337
source-git-commit: 4fd26078017ae13e22ebb02f98335094c8e0581b
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 1%

---

# データ取り込みのガードレール

Guardrail は、データとシステムの使用状況、パフォーマンスの最適化、Adobe Experience Platformでのエラーや予期しない結果の回避に関するガイダンスを提供するしきい値です。 Guardrail は、ライセンスの使用権限に関連して、データや処理の使用状況や使用状況を指す場合があります。

このドキュメントでは、Adobe Experience Platformでのデータ取り込みのガードレールに関するガイダンスを提供します。

## バッチ取り込み用のガードレール

次の表に、 [バッチ取得 API](./batch-ingestion/overview.md) またはソース：

| 取り込みのタイプ | ガイドライン | 備考 |
| --- | --- | --- |
| バッチ取得 API を使用したデータレイクの取得 | <ul><li>バッチ取得 API を使用して、データレイクに 1 時間に最大 20 GB のデータを取り込むことができます。</li><li>バッチあたりの最大ファイル数は 1500 です。</li><li>最大バッチサイズは 100 GB です。</li><li>1 行あたりのプロパティまたはフィールドの最大数は10000です。</li><li>1 ユーザーあたりの 1 分あたりのバッチの最大数は 138 です。</li></ul> |
| バッチソースを使用したデータレイクの取り込み | <ul><li>次のようなバッチ取り込みソースを使用して、1 時間に最大 200 GB のデータをデータレイクに取り込むことができます。 [!DNL Azure Blob], [!DNL Amazon S3]、および [!DNL SFTP].</li><li>バッチサイズは 256 MB から 100 GB の間である必要があります。</li><li>バッチあたりの最大ファイル数は 1500 です。</li></ul> | 詳しくは、 [ソースの概要](../sources/home.md) ソースのカタログの場合は、データ取り込みに使用できます。 |
| プロファイルへのバッチ取り込み | <ul><li>1 時間に最大 120 GB のデータを取り込むことができます。</li><li>レコードクラスの最大サイズは 100 KB（ソフト）です。</li><li>ExperienceEvent クラスの最大サイズは 10 KB（ソフト）です。</li><li>1 レコードの最大サイズは 1 MB です。</li></ul> |

## ストリーミング取り込み用の Guardrail

次の表に、 [ストリーミング取得 API](./streaming-ingestion/overview.md) またはストリーミングソース：

| 取り込みのタイプ | ガイドライン | 備考 |
| --- | --- | --- |
| ストリーミング取得 | <ul><li>最大レコードサイズは 1 MB で、推奨サイズは 10 KB です。</li><li>1 分以内にプロファイルに対する20000秒あたりの要求を処理できます。</li><li>15 分以内に、データレイクに対する 1 秒あたり最大20000件のリクエストを処理できます。</li></ul> | より高いデータスループットが必要な場合は、バッチ取得 API を使用します。 |
| ストリーミングソース | <ul><li>最大レコードサイズは 1 MB で、推奨サイズは 10 KB です。</li><li>ストリーミングソースは、新しいソース接続の作成時に 1 秒あたり 4,000 ～ 5000 個のリクエストをサポートします。 **注意**:ストリーミングデータがデータレイクに完全に処理されるまでに、最大 30 分かかる場合があります。</li><li>データレイクに対する 1 秒あたり 4,000 件から 5,000 件のリクエストを処理できます。 **注意**:ストリーミングデータがデータレイクに完全に処理されるまでに、最大 30 分かかる場合があります。</li></ul> | ストリーミングソース： [!DNL Kafka], [!DNL Azure Event Hubs]、および [!DNL Amazon Kinesis] 使用しない [!DNL Data Collection Core Service] (DCCS) ルートとは異なるスループット制限を持つことができます。 詳しくは、 [ソースの概要](../sources/home.md) ソースのカタログの場合は、データ取り込みに使用できます。 |

## 次の手順

Experience Platformのデータと処理ガードレールの詳細については、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルデータのガードレール](../profile/guardrails.md)
* [ID サービスデータのガードレール](../identity-service/guardrails.md)
