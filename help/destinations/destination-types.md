---
keywords: 宛先;宛先タイプ
title: 宛先のタイプとカテゴリ
description: Adobe Experience Platform の宛先の様々なタイプとカテゴリについて説明します。
exl-id: 7826d1e2-bd6b-4f65-9da9-0a3b3e8bb93b
source-git-commit: 322510055bd8b8803292a2b4af9df9e1dbee7ffb
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 54%

---

# 宛先のタイプとカテゴリ

このページでは、Adobe Experience Platform の宛先の様々なタイプとカテゴリについて説明します。

## 宛先のタイプ {#destination-types}

Adobe Experience Platformでは、接続、データセット書き出し、拡張機能など、様々な宛先タイプを区別します。 接続宛先には、API ベースの宛先、ソーシャル宛先、CRM プラットフォームなど、様々なタイプがあり、データを書き出すことができます。

最後に、接続は、宛先カタログ内のすべての組織で使用可能なパブリック宛先と、Real-Time CDP Ultimate のお客様が特定の書き出しのユースケースを満たすために作成できるプライベート宛先を区別することもできます。

>[!BEGINSHADEBOX]

![ 宛先のタイプを示します。](./assets/destination-types/types-of-destinations-no-highlight.png " 宛先のタイプを示す図。"){zoomable="yes"}

>[!ENDSHADEBOX]

## 接続 {#connections}

Adobe Experience Platformの **[!UICONTROL プロファイル書き出し]**、**[!UICONTROL ストリーミングオーディエンス書き出し]**、および **[!DNL Edge Personalization]** の宛先は、イベントデータを取り込み、他のデータソースと組み合わせて [ リアルタイム顧客プロファイル ](../profile/home.md) を作成し、セグメント化を適用したあと、オーディエンスと対象プロファイルを宛先に書き出します。

## プロファイル書き出し宛先 {#profile-export}

プロファイル書き出し宛先は生データを受け取ります。その際、多くの場合、メールアドレスがプライマリキーとなります。Experience Platform では、現在、次の 2 種類のプロファイル書き出し宛先をサポートしています。

* [バッチ（ファイルベース）の宛先](#file-based)
* [高度なエンタープライズ宛先（ストリーミングプロファイル書き出し宛先）](#advanced-enterprise-destinations)

### 高度なエンタープライズ宛先（ストリーミングプロファイル書き出し宛先） {#advanced-enterprise-destinations}

>[!IMPORTANT]
>
>高度なエンタープライズ宛先（ストリーミングプロファイル書き出し宛先）は、[Adobe Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) のお客様のみが利用できます。

高度なエンタープライズ宛先データコネクタを使用すると、Adobe Real-time Customer Data Platform プロファイルをほぼリアルタイムで社内システムや他のサードパーティシステムに配信して、データの同期、分析、さらにはプロファイルエンリッチメントのユースケースを実現できます。

これらの宛先は、オーディエンスデータとプロファイルデータをExperience Platformデータストリームとして受け取ります。

高度なエンタープライズ宛先は次のとおりです。

* [HTTP API 宛先](catalog/streaming/http-destination.md)
* [Amazon Kinesis](catalog/cloud-storage/amazon-kinesis.md)
* [Azure Event Hubs](catalog/cloud-storage/azure-event-hubs.md)

### バッチ（ファイルベース）宛先 {#file-based}

ファイルベース宛先は、プロファイルや属性を含んだ `.csv` ファイルを受け取ります。[Amazon S3](catalog/cloud-storage/amazon-s3.md) は、プロファイルの書き出しを含んだファイルを書き出せる宛先の例です。

## ストリーミングオーディエンス書き出し宛先 {#streaming-destinations}

オーディエンス書き出し宛先は、Experience Platformオーディエンスデータを受け取ります。 これらの宛先では、オーディエンス ID またはユーザー ID を使用します。 広告宛先やソーシャル宛先（[[!DNL Google Display & Video 360]](catalog/advertising/google-dv360.md)、[[!DNL Google Ads]](catalog/advertising/google-ads-destination.md)、[Facebook](catalog/social/facebook.md) など）は、このような宛先の例です。

## エッジパーソナライゼーション宛先 {#edge-personalization-destinations}

Experience Platform のエッジパーソナライゼーション宛先には、[Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) や[カスタムパーソナライゼーション宛先](/help/destinations/catalog/personalization/custom-personalization.md)が含まれます。これらの宛先を使用すると、同じページや次のページのパーソナライゼーションのユースケースを顧客に対して実現できます。

詳しくは、[同じページと次のページのパーソナライゼーション用にパーソナライゼーション宛先を設定](/help/destinations/ui/activate-edge-personalization-destinations.md)する方法を参照してください。

## プロファイル書き出し宛先とオーディエンス書き出し宛先 – 概要ビデオ {#video}

次のビデオでは、次の 2 種類の宛先の詳細について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

## 書き出すオーディエンスのタイプ {#exported-audiences-types}

Experience Platformから様々な宛先に、次の 3 種類のオーディエンスを書き出すことができます。

* 人物オーディエンス
* アカウントオーディエンス
* 見込み客オーディエンス

[ 様々なオーディエンスタイプ ](/help/segmentation/ui/account-audiences.md#terminology) の詳細をご覧ください。

宛先カードの記号は、各宛先に書き出すことができるオーディエンスのタイプを示します。

![ 書き出せるオーディエンスタイプを示す記号が付いた宛先カードの例。](/help/destinations/assets/destination-types/types-of-audiences.png " 書き出せるオーディエンスタイプを示す記号が付いた宛先カードの例 "){zoomable="yes"}


## データセット書き出し宛先 {#dataset-export-destinations}

宛先カタログ内の一部のクラウドストレージ宛先では、データセット書き出しをサポートしています。これらの宛先を使用すると、生のデータセットをクラウドストレージの場所に書き出すことができます。

詳しくは、[データセットを書き出す](/help/destinations/ui/export-datasets.md)方法を参照してください。

## 拡張機能 {#extensions}

Platform では、タグ管理の機能と柔軟性を活用しているので、UI でタグ拡張機能を設定できるようになっています。

>[!TIP]
>
>ユースケースやインターフェイスでタグ拡張機能を見つける方法など、タグ拡張機能について詳しくは、[タグ拡張機能の概要](./catalog/launch-extensions/overview.md)を参照してください。

タグ拡張機能では、生のイベントデータを複数の種類の宛先に転送します。この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](./catalog/personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](./catalog/voice/confirmit-digital-feedback.md)などがあります。

![タグ拡張機能と他の宛先の比較](./assets/common/launch-and-other-destinations.png)

## 接続と拡張機能を使用するタイミング {#when-to-use}

マーケターは、接続と拡張機能の組み合わせを使用して、お客様の使用例に対処できます。

接続は、一元化された完全な顧客プロファイルや顧客オーディエンスをアクティベーションに活用する必要がある場合に役立ちます。 例えば、Analytics システムの行動データを、アップロードされた CRM データと結合して、特定のオーディエンスのユーザーを選定してから、パーソナライズされたメッセージをそのユーザーに配信する場合は、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化をおこなう場合に役立ちます。例えば、特定のユーザーについて、行動データをファイル上の他のデータソースと結合せずに、外部システムに転送する必要がある場合です。

## 宛先のカテゴリ {#categories}

[宛先カテゴリ](https://platform.adobe.com/destination/catalog)内の接続と拡張機能は、実現しようとしているマーケティングアクションに応じて、宛先カテゴリ（**広告**、**クラウドストレージ**、**調査プラットフォーム**、**メールマーケティング**&#x200B;など）別にグループ化されています。各カテゴリについて、および各カテゴリに含まれる宛先について詳しくは、[宛先カタログのドキュメント](./catalog/overview.md)を参照してください。

![ カタログページでハイライト表示された宛先カテゴリ。](./assets/destination-types/destination-categories-menu.png " カタログページでハイライト表示された宛先カテゴリ。"){zoomable="yes"}
