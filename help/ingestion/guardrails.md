---
keywords: Experience Platform;トラブルシューティング;ガードレール;ガイドライン;
title: データ取り込みのガードレール
description: Adobe Experience Platformでのデータ取り込み用のガードレールについて説明します。
exl-id: f07751cb-f9d3-49ab-bda6-8e6fec59c337
source-git-commit: a862e532382472eadf29aee2568c550b1a71211a
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 37%

---

# データ取り込みのガードレール

>[!IMPORTANT]
>
>バッチおよびストリーミング取り込みのガードレールは、通常、サンドボックスレベルではなく組織レベルで計算されます。 つまり、サンドボックスごとのデータ使用状況は、組織全体に対応するライセンス使用権限の合計にバインドされます。 さらに、開発用サンドボックスでのデータ使用は、プロファイル全体の 10% に制限されています。 ライセンス使用権限について詳しくは、[ データ管理のベストプラクティスガイド ](../landing/license-usage-and-guardrails/data-management-best-practices.md) を参照してください。

ガードレールとは、Adobe Experience Platform でのデータやシステムの使用状況、パフォーマンスの最適化、エラーや予期しない結果の回避に関するガイダンスを提供するしきい値のことです。 ガードレールは、データの使用状況や消費量、ライセンスのエンタイトルメントに関連する処理方法を参照できます。

>[!IMPORTANT]
>
>このガードレール ページに加えて、販売注文と対応する [ 製品説明 ](https://helpx.adobe.com/jp/legal/product-descriptions.html) でライセンスの使用権限を確認してください。

このドキュメントでは、Adobe Experience Platform でのデータ取り込みのガードレールに関するガイダンスを説明します。

## バッチ取り込み用のガードレール

次の表に、[バッチ取り込み API](./batch-ingestion/overview.md) またはソースを使用する際に検討するガードレールの概要を示します。

| 取り込みのタイプ | ガイドライン | 備考 |
| --- | --- | --- |
| バッチ取り込み API を使用したデータレイクの取り込み | <ul><li>バッチ取り込み API を使用して、データレイクに 1 時間あたり最大 20 GB のデータを取り込むことができます。</li><li>バッチあたりの最大ファイル数は 1500 個です。</li><li>最大バッチサイズは 100 GB です。</li><li>1 行あたりのプロパティまたはフィールドの最大数は 10000 個です。</li><li>ユーザーあたりの 1 分ごとの最大バッチ数は 2000 個です。</li></ul> | |
| バッチソースを使用したデータレイクの取り込み | <ul><li>[!DNL Azure Blob]、[!DNL Amazon S3]、[!DNL SFTP] などのバッチ取り込みソースを使用して、データレイクに1 時間あたり最大 200 GB のデータを取り込むことができます。</li><li>バッチサイズは 256 MB から 100 GB の間である必要があります。 これは、非圧縮データと圧縮データの両方に適用されます。 圧縮されたデータがデータレイクで圧縮されていない場合、次の制限が適用されます。</li><li>バッチあたりの最大ファイル数は 1500 個です。</li><li>ファイルまたはフォルダの最小サイズは 1 バイトです。 0 バイトのサイズのファイルやフォルダーは取り込めません。</li></ul> | データ取り込みに使用できるソースのカタログについて詳しくは、[ ソースの概要 ](../sources/home.md) を参照してください。 |
| プロファイルへのバッチ取り込み | <ul><li>レコードクラスの最大サイズは 100 KB （ハード）です。</li><li>ExperienceEvent クラスの最大サイズは 10 KB （ハード）です。</li></ul> | |
| 1 日に取り込まれるプロファイルバッチまたは ExperienceEvent バッチの数 | **1 日に取り込まれるプロファイルバッチまたは ExperienceEvent バッチの最大数は、サンドボックスあたり 90 です。** つまり、1 日に取り込まれるプロファイルバッチと ExperienceEvent バッチを合わせた合計数は 90 を超えることはできないということです。追加のバッチを取り込むと、システムのパフォーマンスに影響します。 | これはソフトリミットです。 ソフトリミットを超えることは可能ですが、ソフトリミットはシステムのパフォーマンスに推奨されるガイドラインです。 さらに、このガードレールは組織ごとではなく **サンドボックスごと** に設定されます。 |
| 暗号化されたデータの取り込み | 1 つの暗号化ファイルのサポートされる最大サイズは 1 GB です。 例えば、1 回のデータフロー実行で 2 GB 以上のデータを取り込むことができますが、データフロー実行の個々のファイルは 1 GB を超えることはできません。 | 暗号化されたデータの取り込みプロセスは、通常のデータ取り込みよりも時間がかかる場合があります。 詳しくは、[ 暗号化されたデータ取り込み API ガイド ](../sources/tutorials/api/encrypt-data.md) を参照してください。 |
| バッチ取り込みをアップサート | アップサートバッチの取り込みは、通常のバッチよりも最大 10 倍遅くなる場合があります。そのため、効率的なランタイムを確保し、サンドボックスで他のバッチが処理されるのをブロックしないようにするには、**アップサートバッチを 200 万件未満のレコードに保つ** 必要があります。 | 200 万件を超えるバッチは間違いなく取り込むことができますが、小さなサンドボックスの制限により、取り込み時間が大幅に長くなります。 |

{style="table-layout:auto"}

## ストリーミング取り込み用のガードレール

ストリーミング取り込みのガードレールについて詳しくは、[ ストリーミング取り込みの概要 ](./streaming-ingestion/overview.md) を参照してください。

## ストリーミングソースのガードレール

次の表に、ストリーミングソースを使用する際に検討するガードレールの概要を示します。

| 取り込みのタイプ | ガイドライン | 備考 |
| --- | --- | --- |
| ストリーミングソース | <ul><li>最大レコードサイズは 1 MB、推奨サイズは 10 KB です。</li><li>ストリーミングソースは、データレイクに取り込む際に、1 秒あたり 4,000～5,000 件のリクエストをサポートします。 これは、既存のソース接続に加えて、新しく作成したソース接続の両方に適用されます。 **注意**：ストリーミングデータがデータレイクへと完全に処理されるまでに、最大 30 分かかる場合があります。</li><li>ストリーミングソースは、データをプロファイルまたはストリーミングセグメント化に取り込む際、1 秒あたり最大 1500 リクエストをサポートします。</li></ul> | [!DNL Kafka]、[!DNL Azure Event Hubs]、[!DNL Amazon Kinesis] などのストリーミングソースは [!DNL Data Collection Core Service]（DCCS）ルートを使用せず、スループットの制限が異なる場合があります。データ取り込みに使用できるソースのカタログについて詳しくは、[ソースの概要](../sources/home.md)を参照してください。 |

{style="table-layout:auto"}

## 次の手順

他のExperience Platform サービスのガードレール、エンドツーエンドの待ち時間の情報およびReal-Time CDP Product Description のドキュメントからのライセンス情報について詳しくは、次のドキュメントを参照してください。

* [Real-Time CDP ガードレール](/help/rtcdp/guardrails/overview.md)
* 様々なExperience Platform サービス用の [ エンドツーエンドの待ち時間の図 ](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=ja#end-to-end-latency-diagrams)。
* [Real-Time Customer Data Platform（B2C Edition - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2P - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2B - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)
