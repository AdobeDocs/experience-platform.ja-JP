---
title: Adobe Experience Platformでのデータ暗号化
description: Adobe Experience Platformでの転送時および保存時のデータの暗号化の仕組みを説明します。
exl-id: 184b2b2d-8cd7-4299-83f8-f992f585c336
source-git-commit: 4f67df5d3667218c79504535534de57f871b0650
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 0%

---

# Adobe Experience Platformでのデータ暗号化

Adobe Experience Platformは、エンタープライズソリューション全体でカスタマーエクスペリエンスデータを一元化および標準化する、強力で拡張性の高いシステムです。 Platform で使用されるすべてのデータは、転送時および保存時に暗号化され、データのセキュリティが維持されます。 このドキュメントでは、Platform の暗号化プロセスを大まかに説明します。

次のプロセスフロー図は、Experience Platformによるデータの取得、暗号化、保持の仕組みを示しています。

![Experience Platformによるデータの取得、暗号化、保持の方法を示す図。](../images/governance-privacy-security/encryption/flow.png)

## 転送中のデータ {#in-transit}

Platform と外部コンポーネント間のすべてのデータは、HTTPS を使用して暗号化された安全な接続で転送されます [TLS v1.2](https://datatracker.ietf.org/doc/html/rfc5246).

通常、データは次の 3 つの方法で Platform に取り込まれます。

- [データ収集](../../collection/home.md) の機能により、web サイトやモバイルアプリケーションはデータを Platform Edge Networkに送信して、ステージングや取り込みの準備を行うことができます。
- [ソースコネクタ](../../sources/home.md) Adobe Experience Cloud アプリケーションやその他のエンタープライズデータソースから Platform に直接データをストリーミングします。
- 非Adobe ETL （抽出、変換、読み込み）ツールは、データをに送信します。 [バッチ取得 API](../../ingestion/batch-ingestion/overview.md) （消費用）。

データがシステムに取り込まれた後、 [保存時に暗号化](#at-rest)、Platform サービスは、次の方法でデータを強化および書き出します。

- [宛先](../../destinations/home.md) を使用すると、Adobeアプリケーションおよびパートナーアプリケーションに対してデータをアクティブ化できます。
- 次のようなネイティブプラットフォームアプリケーション [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja) および [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home) は、データを利用することもできます。

### mTLS プロトコルのサポート {#mtls-protocol-support}

相互トランスポート層セキュリティ（mTLS）を使用して、への送信接続のセキュリティを強化できるようになりました [HTTP API 宛先](../../destinations/catalog/streaming/http-destination.md) とAdobe Journey Optimizer [カスタムアクション](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/using-custom-actions). mTLS は、相互認証のためのエンドツーエンドのセキュリティ方法であり、情報を共有する両方の関係者が、データが共有される前に主張している人物であることを保証します。 mTLS には、TLS と比較して追加の手順が含まれています。この手順では、サーバーもクライアントの証明書を要求し、最後に確認します。

必要に応じて、 [Adobe Journey Optimizerのカスタムアクションでの mTLS の使用](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/configuration/configure-journeys/action-journeys/about-custom-action-configuration) およびExperience PlatformHTTP API 宛先ワークフロー、Adobe Journey Optimizer カスタマーアクション UI または宛先 UI に入力するサーバーアドレスでは、TLS プロトコルを無効にし、mTLS のみを有効にする必要があります。 そのエンドポイントで TLS 1.2 プロトコルがまだ有効になっている場合、クライアント認証に対して証明書は送信されません。 つまり、これらのワークフローで mTLS を使用するには、「受信」サーバーエンドポイントが mTLS である必要があります **のみ** 接続エンドポイントを有効にしました。

>[!IMPORTANT]
>
>mTLS を有効化するために、Adobe Journey Optimizerのカスタムアクションや HTTP API の宛先で必要な追加設定はありません。このプロセスは、mTLS が有効なエンドポイントが検出された場合に自動的に実行されます。 各証明書の共通名（CN）とサブジェクト代替名（SAN）は、証明書の一部としてドキュメントで入手でき、これを使用する場合は追加の所有権検証レイヤーとして使用できます。
>
>2000 年 5 月に公開された RFC 2818 では、サブジェクト名の検証のために、HTTPS 証明書での共通名（CN）フィールドの使用が非推奨（廃止予定）になっています。 代わりに、「dns 名」タイプの「サブジェクトの代替名」拡張機能（SAN）を使用することをお勧めします。

### 証明書をダウンロード {#download-certificates}

>[!NOTE]
>
>公開証明書を最新の状態に保つのは、お客様の責任です。 特に有効期限が近づいたら、証明書を定期的に確認してください。 環境の最新のコピーを保持するために、このページをブックマークに追加する必要があります。

CN または SAN をチェックしてサードパーティの検証を追加する場合は、関連する証明書をこちらからダウンロードできます。

- [Adobe Journey Optimizer公開証明書](../images/governance-privacy-security/encryption/AJO-public-certificate.pem)
- [宛先サービスの公開証明書](../images/governance-privacy-security/encryption/destinations-public-cert.pem).

## 保存中のデータ {#at-rest}

Platform で取得され、使用されるデータは、出所やファイル形式に関係なく、システムで管理されるすべてのデータを含む非常にきめ細かいデータストアであるデータレイクに保存されます。 データレイクに保持されるすべてのデータは、分離された形式で暗号化、保存、管理されます [[!DNL Microsoft Azure Data Lake] ストレージ](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) 組織に固有のインスタンス。

保存データの Azure Data Lake Storage での暗号化の仕組みについて詳しくは、を参照してください。 [azure の公式ドキュメント](https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption).

## 次の手順

このドキュメントでは、Platform でのデータの暗号化の概要を説明しました。 Platform のセキュリティ手順について詳しくは、の概要を参照してください。 [ガバナンス、プライバシー、セキュリティ](./overview.md) Experience League時、またはを参照 [Platform セキュリティのホワイトペーパー](https://www.adobe.com/content/dam/cc/en/security/pdfs/AEP_SecurityOverview.pdf).
