---
keywords: Experience Platform;トラブルシューティング;ガードレール;ガイドライン;
title: データ取り込みのガードレール
description: このドキュメントでは、Adobe Experience Platform でのデータ取り込みのガードレールに関するガイダンスを説明します。
exl-id: f07751cb-f9d3-49ab-bda6-8e6fec59c337
source-git-commit: 0e609ce278af0c93503f05778887ad1bd881524a
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 79%

---

# データ取り込みのガードレール

ガードレールとは、Adobe Experience Platform でのデータやシステムの使用状況、パフォーマンスの最適化、エラーや予期しない結果の回避に関するガイダンスを提供するしきい値のことです。 ガードレールは、データの使用状況や消費量、ライセンスのエンタイトルメントに関連する処理方法を参照できます。

このドキュメントでは、Adobe Experience Platform でのデータ取り込みのガードレールに関するガイダンスを説明します。

## バッチ取り込み用のガードレール

次の表に、[バッチ取り込み API](./batch-ingestion/overview.md) またはソースを使用する際に検討するガードレールの概要を示します。

| 取り込みのタイプ | ガイドライン | 備考 |
| --- | --- | --- |
| バッチ取り込み API を使用したデータレイクの取り込み | <ul><li>バッチ取り込み API を使用して、データレイクに 1 時間あたり最大 20 GB のデータを取り込むことができます。</li><li>バッチあたりの最大ファイル数は 1500 個です。</li><li>最大バッチサイズは 100 GB です。</li><li>1 行あたりのプロパティまたはフィールドの最大数は 10000 個です。</li><li>ユーザーあたりの 1 分ごとの最大バッチ数は 138 個です。</li></ul> |
| バッチソースを使用したデータレイクの取り込み | <ul><li>[!DNL Azure Blob]、[!DNL Amazon S3]、[!DNL SFTP] などのバッチ取り込みソースを使用して、データレイクに1 時間あたり最大 200 GB のデータを取り込むことができます。</li><li>バッチサイズは 256 MB から 100 GB の間である必要があります。</li><li>バッチあたりの最大ファイル数は 1500 個です。</li></ul> | データ取り込みに使用できるソースのカタログについて詳しくは、[ソースの概要](../sources/home.md)を参照してください。 |
| プロファイルへのバッチ取り込み | <ul><li>レコードクラスの最大サイズは 100 KB（ソフト）です。</li><li>ExperienceEvent クラスの最大サイズは 10 KB（ソフト）です。</li><li>単一のレコードの最大サイズは 1 MB です。</li></ul> |
| 1 日に取り込まれるプロファイルバッチまたは ExperienceEvent バッチの数 | **1 日に取り込まれるプロファイルバッチまたは ExperienceEvent バッチの最大数は 90 です。** つまり、1 日に取り込まれるプロファイルバッチと ExperienceEvent バッチを合わせた合計数は 90 を超えることはできないということです。追加のバッチを取り込むと、システムのパフォーマンスに影響します。 | これはソフトリミットです。 ソフトリミットを超えることは可能ですが、ソフトリミットはシステムパフォーマンスの推奨ガイドラインを示すものです。 |

## ストリーミング取り込み用のガードレール

詳しくは、 [ストリーミング取り込みの概要](./streaming-ingestion/overview.md) ストリーミング取り込み用の guardrail に関する情報を参照してください。

## ストリーミングソース用のガードレール

次の表に、ストリーミングソースを使用する際に考慮すべきガードレールの概要を示します。

| 取り込みのタイプ | ガイドライン | 備考 |
| --- | --- | --- |
| ストリーミングソース | <ul><li>最大レコードサイズは 1 MB、推奨サイズは 10 KB です。</li><li>ストリーミングソースは、新規ソース接続の作成時に 1 秒あたり 4,000～5,000 件のリクエストをサポートします。**注意**：ストリーミングデータがデータレイクへと完全に処理されるまでに、最大 30 分かかる場合があります。</li><li>データレイクに対するリクエストを 1 秒あたり 4,000～5,000 件処理できます。**注意**：ストリーミングデータがデータレイクへと完全に処理されるまでに、最大 30 分かかる場合があります。</li></ul> | [!DNL Kafka]、[!DNL Azure Event Hubs]、[!DNL Amazon Kinesis] などのストリーミングソースは [!DNL Data Collection Core Service]（DCCS）ルートを使用せず、スループットの制限が異なる場合があります。データ取り込みに使用できるソースのカタログについて詳しくは、[ソースの概要](../sources/home.md)を参照してください。 |

## 次の手順

その他のExperience Platformサービスガードレール、エンドツーエンドの遅延情報、およびReal-Time CDP製品説明ドキュメントのライセンス情報の詳細については、次のドキュメントを参照してください。

* [Real-Time CDP Guardrails](/help/rtcdp/guardrails/overview.md)
* [エンドツーエンドの待ち時間図](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=en#end-to-end-latency-diagrams) 様々なExperience Platformサービス。
* [Real-time Customer Data Platform（B2C 版 — プライムパッケージおよび究極パッケージ）](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2P — プライムおよび究極のパッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2B — プライムおよび究極のパッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)
