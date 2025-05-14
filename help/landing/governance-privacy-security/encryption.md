---
title: Adobe Experience Platformでのデータ暗号化
description: Adobe Experience Platformでの転送時および保存時のデータの暗号化の仕組みを説明します。
exl-id: 184b2b2d-8cd7-4299-83f8-f992f585c336
source-git-commit: f6eaba4c0622318ba713c562ba0a4c20bba02338
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 6%

---

# Adobe Experience Platformでのデータ暗号化

Adobe Experience Platformは、エンタープライズソリューション全体でカスタマーエクスペリエンスデータを一元化および標準化する、強力で拡張性の高いシステムです。 Experience Platformで使用されるすべてのデータは、転送時および保存時に暗号化され、データのセキュリティが維持されます。 このドキュメントでは、Experience Platformの暗号化プロセスを大まかに説明します。

次のプロセスフロー図は、Experience Platformによるデータの取得、暗号化、保持の仕組みを示しています。

![Experience Platformによるデータの取得、暗号化、保持の方法を示す図 ](../images/governance-privacy-security/encryption/flow.png)

## 転送中のデータ {#in-transit}

Experience Platformと外部コンポーネント間のすべてのデータは、HTTPS [TLS v1.2](https://datatracker.ietf.org/doc/html/rfc5246) を使用した安全な暗号化接続で転送されます。

通常、データは次の 3 つの方法でExperience Platformに取り込まれます。

- [ データ収集 ](../../collection/home.md) 機能を使用すると、web サイトやモバイルアプリケーションからExperience Platform Edge Networkにデータを送信して、ステージングや取り込みの準備を行うことができます。
- [Source コネクタ ](../../sources/home.md)Adobe Experience Cloud アプリケーションやその他のエンタープライズデータソースからExperience Platformに直接データをストリーミングします。
- 非Adobe ETL （抽出、変換、読み込み）ツールは、データを使用するために [ バッチ取得 API](../../ingestion/batch-ingestion/overview.md) に送信します。

データがシステムに取り込まれ、[ 保存時に暗号化 ](#at-rest) た後、Experience Platform サービスは以下の方法でデータを強化および書き出します。

- [ 宛先 ](../../destinations/home.md) を使用すると、Adobe アプリケーションおよびパートナーアプリケーションに対してデータをアクティブ化できます。
- [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja) や [Adobe Journey Optimizer&rbrace; などのネイティブなExperience Platform アプリケ ](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home) ションも、データを利用できます。

### mTLS プロトコルのサポート {#mtls-protocol-support}

相互トランスポート層セキュリティ（mTLS）を使用して、[HTTP API 宛先 ](../../destinations/catalog/streaming/http-destination.md) およびAdobe Journey Optimizer[ カスタムアクション ](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/using-custom-actions) への送信接続のセキュリティを強化できるようになりました。 mTLS は、データが共有される前に情報を共有する両者が本人であることを確認する、相互認証のためのエンドツーエンドのセキュリティ方式です。mTLS には TLS と比較して追加の手順が含まれており、サーバーはクライアントの証明書を要求し、クライアント側でそれを検証します。

[Adobe Journey Optimizer カスタムアクションとExperience Platform HTTP API 宛先ワークフローで mTLS を使用する ](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/configuration/configure-journeys/action-journeys/about-custom-action-configuration) 場合、Adobe Journey Optimizer カスタマーアクション UI または宛先 UI に入力するサーバーアドレスでは、TLS プロトコルが無効になっており、mTLS のみが有効になっている必要があります。 そのエンドポイントで TLS 1.2 プロトコルがまだ有効になっている場合、クライアント認証に対して証明書は送信されません。 つまり、これらのワークフローで mTLS を使用するには、「受信」サーバーエンドポイントが mTLS **のみ** 有効な接続エンドポイントである必要があります。

>[!IMPORTANT]
>
>mTLS を有効化するために、Adobe Journey Optimizerのカスタムアクションや HTTP API の宛先で必要な追加設定はありません。このプロセスは、mTLS が有効なエンドポイントが検出された場合に自動的に実行されます。 各証明書の共通名（CN）とサブジェクト代替名（SAN）は、証明書の一部としてドキュメントで入手でき、これを使用する場合は追加の所有権検証レイヤーとして使用できます。
>
>2000 年 5 月に公開された RFC 2818 では、サブジェクト名の検証のために、HTTPS 証明書での共通名（CN）フィールドの使用が非推奨（廃止予定）になっています。 代わりに、「dns 名」タイプの「サブジェクトの代替名」拡張機能（SAN）を使用することをお勧めします。

### 証明書をダウンロード {#download-certificates}

>[!NOTE]
>
>システムが有効な公開証明書を使用していることを確認する責任があります。 特に有効期限が近づいたら、証明書を定期的に確認してください。 API を使用して、有効期限が切れる前に証明書を取得および更新します。

公開 mTLS 証明書の直接ダウンロードリンクが提供されなくなりました。 代わりに、[ 公開証明書エンドポイント ](../../data-governance/mtls-api/public-certificate-endpoint.md) を使用して証明書を取得します。 これは、現在の公開証明書にアクセスするためにサポートされている唯一の方法です。 これにより、統合に対して常に有効な最新の証明書を受け取ることができます。

証明書ベースの暗号化に依存する統合では、API を使用した自動証明書取得をサポートするために、ワークフローを更新する必要があります。 静的リンクや手動の更新に依存すると、期限切れの証明書や失効した証明書が使用され、統合に失敗する場合があります。

#### 証明書のライフサイクルの自動化 {#certificate-lifecycle-automation}

Adobeは、mTLS 統合の証明書のライフサイクルを自動化して、信頼性を向上し、サービスの中断を防ぐようになりました。 公開証明書は次のとおりです。

- 有効期限の 60 日前に再発行しました。
- 有効期限の 30 日前に失効しました。

これらの間隔は、証明書の有効期間を最大 47 日に短縮することを目的とした [ 進化する CA/B フォーラムのガイドライン ](https://www.digicert.com/blog/tls-certificate-lifetimes-will-officially-reduce-to-47-days) に従って、引き続き短くなります。

このページのリンクを以前に使用して証明書をダウンロードした場合は、API を通じて排他的に取得するようにプロセスを更新します。

## 保存中のデータ {#at-rest}

Experience Platformで取得され、使用されるデータは、接触チャネルやファイルフォーマットに関係なく、システムで管理されるすべてのデータを含む非常にきめ細かいデータストアであるデータレイクに保存されます。 データレイクに保持されるすべてのデータは、組織に固有の分離された [[!DNL Microsoft Azure Data Lake]  ストレージ ](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) インスタンスで暗号化、保存、管理されます。

保存データの Azure Data Lake Storage での暗号化の仕組みについて詳しくは、[Azure の公式ドキュメント ](https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption) を参照してください。

## 次の手順

このドキュメントでは、Experience Platformでのデータの暗号化の概要を説明しました。 Experience Platformのセキュリティ手順について詳しくは、Experience Leagueの [ ガバナンス、プライバシー、セキュリティ ](./overview.md) に関する概要を参照するか、[Experience Platform セキュリティホワイトペーパー ](https://www.adobe.com/content/dam/cc/en/security/pdfs/AEP_SecurityOverview.pdf) をご覧ください。
