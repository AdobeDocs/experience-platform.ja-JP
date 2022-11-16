---
title: Adobe Experience Platformでのデータ暗号化
topic-legacy: data protection
description: 送信時およびAdobe Experience Platformでの保存時にデータを暗号化する方法を説明します。
exl-id: 184b2b2d-8cd7-4299-83f8-f992f585c336
source-git-commit: d99a9081edc483831d56af3d838b67d9aba25bea
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 5%

---

# Adobe Experience Platformでのデータ暗号化

Adobe Experience Platformは、企業のソリューション全体で顧客体験データを一元化および標準化する、強力で拡張可能なシステムです。 Platform で使用されるすべてのデータは、送信時および保存時に暗号化され、データのセキュリティが確保されます。 このドキュメントでは、Platform の暗号化プロセスを大まかに説明します。

次のプロセスフロー図は、データが [!DNL Experience Platform]:

![](../images/governance-privacy-security/encryption/flow.png)

## 送信中のデータ {#in-transit}

Platform と外部コンポーネントとの間で転送されるすべてのデータは、HTTPS を使用した安全な暗号化された接続を通じて実行されます [TLS v1.2](https://datatracker.ietf.org/doc/html/rfc5246).

一般に、データは次の 3 つの方法で Platform に取り込まれます。

* [データ収集](../../collection/home.md) の機能を使用すると、Web サイトやモバイルアプリケーションは、取得のステージングと準備のために Platform Edge Network にデータを送信できます。
* [ソースコネクタ](../../sources/home.md) Adobe Experience Cloudアプリケーションやその他のエンタープライズデータソースから Platform に直接データをストリーミングします。
* 非AdobeETL（抽出、変換、読み込み）ツールは、 [バッチ取得 API](../../ingestion/batch-ingestion/overview.md) 消費に関して

データがシステムに取り込まれ、 [保存時に暗号化される](#at-rest)その後、次の方法で Platform サービスによって強化し、システムから取り出すことができます。

* [宛先](../../destinations/home.md) を使用すると、データをAdobe・アプリケーションやパートナー・アプリケーションに対してアクティブ化できます。
* 次のようなネイティブ Platform アプリケーション [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja) および [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja) また、データを使用することもできます。

## 保存中のデータ {#at-rest}

Platform で取り込まれ、使用されるデータは、接触チャネルやファイル形式に関係なく、システムで管理されるすべてのデータを含む、非常に精度の高いデータストアであるデータレイクに保存されます。 データレイクに保持されるすべてのデータは、分離された [[!DNL Microsoft Azure Data Lake] ストレージ](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) 組織に固有のインスタンス。

Azure Data Lake Storage での保存データの暗号化方法について詳しくは、 [Azure の公式ドキュメント](https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption).

## 次の手順

このドキュメントでは、Platform でのデータの暗号化方法の概要を説明しました。 Platform でのセキュリティ手順について詳しくは、「 [ガバナンス、プライバシー、セキュリティ](./overview.md) Experience Leagueで [Platform セキュリティに関するホワイトペーパー](https://www.adobe.com/content/dam/cc/en/security/pdfs/AEP_SecurityOverview.pdf).
