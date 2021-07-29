---
keywords: 宛先；宛先；宛先タイプ
title: 宛先のタイプとカテゴリ
seo-title: 宛先のタイプとカテゴリ
description: Adobe Experience Platformの宛先の様々なタイプとカテゴリについて説明します。
exl-id: 7826d1e2-bd6b-4f65-9da9-0a3b3e8bb93b
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 63%

---

# 宛先のタイプとカテゴリ

Adobe Experience Platformの宛先の様々なタイプとカテゴリについては、このページを参照してください。

## 宛先のタイプ

Adobe Experience Platformでは、接続と拡張機能の2つの宛先タイプを区別します。 接続の宛先には、プロファイルの書き出しの宛先と、セグメントの書き出しの宛先があります。

![宛先のタイプ](./assets/destination-types/types-of-destinations.png)

## 接続 {#connections}

**[!UICONTROL Adobe Experience Platformのプ]** ロファイ **[!UICONTROL ルの書き出しとセグメントの書き出しでは、イベントデータを取り込み、他のデータソースと組み合わせてリアルタイム顧客プロファイルを形成]**  [](../profile/home.md)し、セグメント化を適用し、セグメントと認定済みプロファイルを宛先に書き出します。

## プロファイルの書き出し先

プロファイル書き出し先は、プロファイルや属性を含むファイルを生成します。これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。[Amazon S3 クラウドストレージの宛先](./catalog/cloud-storage/amazon-s3.md)は、プロファイルの書き出しを含むファイルを配置できる、宛先の一例です。

## セグメントの書き出し先

セグメントの書き出し先は、プロファイルとそれらが宛先プラットフォームとして認定したセグメントを送信します。これらの宛先では、セグメント ID またはユーザー ID を使用します。[[!DNL Google Display & Video 360]](./catalog/advertising/google-dv360.md)や[[!DNL Google Ads]](./catalog/advertising/google-ads-destination.md)などの広告の宛先が、このタイプの宛先の一例です。

## プロファイルの書き出しとセグメントの書き出し先 - ビデオの概要

次のビデオでは、2 種類の宛先の詳細について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

## 拡張機能 {#extensions}

Platformは、タグ管理の機能と柔軟性を活用し、データ収集UIでタグ拡張を設定できます。

>[!TIP]
>
>使用例を含むタグ拡張について詳しくは、[タグ拡張の概要](./catalog/launch-extensions/overview.md)を参照してください。

タグ拡張は、生のイベントデータを複数のタイプの宛先に転送します。 この拡張機能は、**イベント転送**&#x200B;タイプの宛先であると考えることができます。これは、宛先プラットフォームと単純に統合された機能で、イベントの生データを転送するだけです。例としては、[Gainsight パーソナライズ機能拡張機能](./catalog/personalization/gainsight.md)や [Confirmit Voice of the Customer 拡張機能](./catalog/voice/confirmit-digital-feedback.md)などがあります。

![他の宛先とのタグ拡張](./assets/common/launch-and-other-destinations.png)

## 接続と拡張機能を使用するタイミング

マーケターは、接続と拡張機能の組み合わせを使用して、お客様の使用例に対処できます。

接続は、完全な一元化された顧客プロファイルまたは顧客セグメントをアクティベーションに活用する必要がある場合に役立ちます。例えば、アップロードされた CRM データを含む Analytics システムの行動データを結合して、ユーザーにパーソナライズされたメッセージを配信する前に、特定のセグメントに対してそのユーザーを評価する場合、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化をおこなう場合に役立ちます。例えば、特定のユーザーについて、行動データをファイル上の他のデータソースと結合せずに、外部システムに転送する必要がある場合です。

## 宛先カテゴリ

[宛先カタログ](https://platform.adobe.com/destination/catalog)の接続と拡張は、達成に役立つマーケティングアクションに応じて、宛先カテゴリ（**Advertising**、**Cloud storage**、**Surveyプラットフォーム**、**Email marketing**&#x200B;など）でグループ化されます。 各カテゴリについて、および各カテゴリに含まれる宛先について詳しくは、[宛先カタログのドキュメント](./catalog/overview.md)を参照してください。

![宛先カテゴリ](./assets/destination-types/destination-categories-menu.png)
