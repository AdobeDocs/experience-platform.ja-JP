---
keywords: 宛先、宛先、移動先の種類
title: 宛先のタイプとカテゴリ
seo-title: Destination types and categories
description: Adobe エクスペリエンスプラットフォームにおいて、様々な種類や移動先のカテゴリについて説明しています。
exl-id: 7826d1e2-bd6b-4f65-9da9-0a3b3e8bb93b
source-git-commit: a7c36f1a157b6020fede53e5c1074d966f26cf3d
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 45%

---

# 宛先のタイプとカテゴリ

このページでは、アドビシステムズ社のプラットフォームにおける各送信先の種類とカテゴリーについて説明します。

## 宛先のタイプ

Adobe エクスペリエンスプラットフォームでは、2つの宛先タイプ (接続と拡張) を区別しました。 接続の宛先には、プロファイルの書き出しの宛先と、セグメントの書き出しの宛先があります。

![宛先のタイプ](./assets/destination-types/types-of-destinations.png)

## 接続 {#connections}

**[!UICONTROL プロファイル]** の書き出しおよび **[!UICONTROL ストリーミングセグメント]** の書き出し先は、Adobe エクスペリエンス Platform でイベントデータを取得し、他のデータソースと組み合わせることによって、その他のデータソースを使用して、セグメントを作成し、 [ セグメントを書き出し、 ](../profile/home.md) 宛先にプロファイルを書き出すことができます。

## プロファイルの書き出し先

プロファイルの書き出し先には、主キーとして電子メールアドレスを使用して、未処理のデータが含まれている場合があります。 現在サポートされているエクスペリエンスプラットフォームは、次の2種類のプロファイルエクスポート先をサポートします。

* [プロファイルの書き出し先のストリーミング](#streaming-profile-export)
* [ファイルベースの宛先](#file-based)

### プロファイルの書き出し先のストリーミング {#streaming-profile-export}

ストリーミングプロファイルエクスポート先は、操作プラットフォームデータストリームとしてセグメントとプロファイルデータを受信します。 [Amazon Kinesis ](catalog/cloud-storage/amazon-kinesis.md) と [ Azure イベントハブ ](catalog/cloud-storage/azure-event-hubs.md) は、このような宛先にはありません。

### ファイルベースの宛先 {#file-based}

ファイルベースの宛先に `.csv` は、プロファイルと属性を含むファイルが含まれています。 [Amazon S3 ](catalog/cloud-storage/amazon-s3.md) は、プロファイルの書き出しが格納されているファイルを取り込むための送り先の例です。

## ストリーミングセグメントの書き出し先 {#streaming-destinations}

セグメントの書き出し先受信エクスペリエンスプラットフォームセグメントデータ。 このような宛先にはセグメント Id またはユーザー Id が使用されます。 [[!DNL Google Display & Video 360]](catalog/advertising/google-dv360.md)、 [[!DNL Google Ads]](catalog/advertising/google-ads-destination.md) 、などが挙げられます。

## プロファイルの書き出しおよびセグメントの書き出し先-ビデオ概要 {#video}

次のビデオでは、2 種類の宛先の詳細について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

## 拡張機能 {#extensions}

プラットフォームは、タグ管理の機能と柔軟性を活用して、データ収集 UI でタグの拡張機能を設定できます。

>[!TIP]
>
>タグの拡張について詳しくは、ユースケースとインターフェイスでの検索方法を参照してください [ ](./catalog/launch-extensions/overview.md) 。

タグの拡張機能では、未処理のイベントデータがいくつかの種類の宛先に転送されます。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](./catalog/personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](./catalog/voice/confirmit-digital-feedback.md)などがあります。

![タグの拡張機能 (他の宛先との比較)](./assets/common/launch-and-other-destinations.png)

## 接続と拡張機能を使用するタイミング {#when-to-use}

マーケターは、接続と拡張機能の組み合わせを使用して、お客様の使用例に対処できます。

接続は、完全な一元化された顧客プロファイルまたは顧客セグメントをアクティベーションに活用する必要がある場合に役立ちます。例えば、アップロードされた CRM データを含む Analytics システムの行動データを結合して、ユーザーにパーソナライズされたメッセージを配信する前に、特定のセグメントに対してそのユーザーを評価する場合、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化をおこなう場合に役立ちます。例えば、特定のユーザーについて、行動データをファイル上の他のデータソースと結合せずに、外部システムに転送する必要がある場合です。

## 宛先カテゴリ {#categories}

送信先カタログの接続と拡張機能 [ ](https://platform.adobe.com/destination/catalog) は、 **** **** **** **** お客様が達成できるマーケティングアクションによって、宛先のカテゴリー (広告、クラウドストレージ、アンケートプラットフォーム、電子メールマーケティングなど) ごとにグループ化されています。 各カテゴリについて、および各カテゴリに含まれる宛先について詳しくは、[宛先カタログのドキュメント](./catalog/overview.md)を参照してください。

![宛先カテゴリ](./assets/destination-types/destination-categories-menu.png)
