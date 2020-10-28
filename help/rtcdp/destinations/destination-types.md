---
keywords: destinations;destination;destination types
title: 宛先のタイプとカテゴリ
seo-title: 宛先のタイプとカテゴリ
description: 'アドビのリアルタイム顧客データプラットフォームでは、プロファイル／セグメントの書き出し先はイベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメントと適格なプロファイルを宛先へと書き出します。Experience Platform Launch拡張は、生のイベントデータを複数のタイプの宛先に転送します。 '
seo-description: アドビのリアルタイム顧客データプラットフォームでは、プロファイル／セグメントの書き出し先はイベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメントと適格なプロファイルを宛先へと書き出します。Experience Platform Launch拡張は、生のイベントデータを複数のタイプの宛先に転送します。
translation-type: tm+mt
source-git-commit: e6276eac05f0a20a668051034e83831002a464f0
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 82%

---


# 宛先のタイプとカテゴリ

このページを読んで、アドビのリアルタイム顧客データプラットフォームの様々なタイプとカテゴリについて理解してください。

## 宛先のタイプ

アドビのリアルタイム顧客データプラットフォームには、接続と拡張機能という 2 つのタイプの宛先があります。接続の宛先には、プロファイルの書き出しの宛先と、セグメントの書き出しの宛先があります。

![宛先のタイプ](/help/rtcdp/destinations/assets/types-of-destinations.png)

<br> 

### 接続 {#connections}

アドビのリアルタイム顧客データプラットフォームのキャプチャイベントデータでは、**[!UICONTROL プロファイル書き出し]**&#x200B;および&#x200B;**[!UICONTROL セグメント書き出し]**&#x200B;の宛先は、イベントデータを取り込み、他のデータソースと組み合わせて[リアルタイム顧客プロファイル](/help/profile/home.md)を形成、セグメント化を適用し、セグメントと適格なプロファイルを宛先に適用します。

<br> 

#### プロファイルの書き出し先

プロファイル書き出し先は、プロファイルや属性を含むファイルを生成します。これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。[Amazon S3 クラウドストレージの宛先](/help/rtcdp/destinations/amazon-s3-destination.md)は、プロファイルの書き出しを含むファイルを配置できる、宛先の一例です。

#### セグメントの書き出し先

セグメントの書き出し先は、プロファイルとそれらが宛先プラットフォームとして認定したセグメントを送信します。これらの宛先では、セグメント ID またはユーザー ID を使用します。Advertising destinations such as [[!DNL Google Display & Video 360]](/help/rtcdp/destinations/google-dv360-destination.md) or [[!DNL Google Ads]](/help/rtcdp/destinations/google-ads-destination.md) are examples of these types of destinations.

#### プロファイルの書き出しとセグメントの書き出し先 - ビデオの概要

次のビデオでは、2 種類の宛先の詳細について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

<br> 

### 拡張機能 {#extensions}

AdobeReal-time CDPは、AdobeReal-time CDPインターフェイスにPlatform Launch拡張機能を組み込むため、Adobe Experience Platform Launchの機能と柔軟性を活用します。

>[!TIP]
>
>使用例やインターフェイス内での検索方法など、Adobe Experience Platform Launch拡張機能について詳しくは、 [Adobe Experience Platform Launch拡張機能の概要を参照してください](/help/rtcdp/destinations/experience-platform-launch-extensions.md)。

Platform Launch拡張は、生のイベントデータを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](/help/rtcdp/destinations/gainsight-extension.md)や [Confirmit Voice of the Customer 拡張機能](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)などがあります。

![Experience Platform Launch 拡張機能と他の宛先との比較](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

<br> 

### 接続と拡張機能を使用するタイミング

マーケターは、接続と拡張機能の組み合わせを使用して、お客様の使用例に対処できます。

接続は、完全な一元化された顧客プロファイルまたは顧客セグメントをアクティベーションに活用する必要がある場合に役立ちます。例えば、アップロードされた CRM データを含む Analytics システムの行動データを結合して、ユーザーにパーソナライズされたメッセージを配信する前に、特定のセグメントに対してそのユーザーを評価する場合、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化をおこなう場合に役立ちます。例えば、特定のユーザーについて、行動データをファイル上の他のデータソースと結合せずに、外部システムに転送する必要がある場合です。

<br> 

## 宛先カテゴリ

[宛先カテゴリ](https://platform.adobe.com/destination/catalog)内の接続と拡張機能は、達成できるよう、マーケティングの使用例に応じて、宛先カテゴリ（**広告**、**クラウドストレージ**、**調査プラットフォーム**、**電子メールマーケティング**&#x200B;など）でグループ化されます。各カテゴリについて、および各カテゴリに含まれる宛先について詳しくは、[宛先カタログのドキュメント](/help/rtcdp/destinations/destinations-catalog.md)を参照してください。

![宛先カテゴリ](/help/rtcdp/destinations/assets/destination-categories-menu.png)

