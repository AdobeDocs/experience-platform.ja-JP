---
title: 宛先のタイプとカテゴリ
seo-title: 宛先のタイプとカテゴリ
description: 'Adobe Real-time Customer Data Platformでは、プロファイル/セグメントエクスポート先は、イベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメント化を適用し、条件を満たすプロファイルを宛先に適用します。 起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。 '
seo-description: Adobe Real-time Customer Data Platformでは、プロファイル/セグメントエクスポート先は、イベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメント化を適用し、条件を満たすプロファイルを宛先に適用します。 起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。
translation-type: tm+mt
source-git-commit: 617cf1934402b9001647d7704fb24d6256069ff3
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 7%

---


# 宛先のタイプとカテゴリ

このページを読んで、Adobe Real-time Customer Data Platformの様々なタイプとカテゴリについて理解してください。

## 送信先のタイプ

Adobe Real-time Customer Data Platformでは、接続と拡張の2つの宛先タイプを区別します。 接続先には、プロファイルエクスポート先とセグメントエクスポート先の2種類があります。

![宛先のタイプ](/help/rtcdp/destinations/assets/types-of-destinations.png)

<br> 

### 接続

**Adobe Real-time Customer Data Platformのプロファイルエクスポート** と **セグメントエクスポートのイベントデータを取得し、他のデータソースと組み合わせて、リアルタイム顧客プロファイルを形成し、セグメント化を適用し**[](/help/profile/home.md)、セグメント化を行い、条件を満たすプロファイルを宛先に適用します。

<br> 

#### プロファイルの書き出し先

プロファイル書き出し先は、プロファイルや属性を含むファイルを生成します。これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。宛先は [Amazon S3クラウドストレージ](/help/rtcdp/destinations/amazon-s3-destination.md) 、プロファイルエクスポートを含むファイルをデポジットできる宛先の例です。

#### セグメントの書き出し先

セグメントのエクスポート先は、プロファイルと、それらが資格を得たセグメントを宛先プラットフォームに送信します。 これらの宛先では、セグメント ID またはユーザー ID を使用します。このような表示先には、 [Google DisplayやVideo 360](/help/rtcdp/destinations/google-dv360-destination.md) 、 [Google Adsなどの広告先があります](/help/rtcdp/destinations/google-ads-destination.md) 。

#### プロファイルのエクスポート先とセグメントのエクスポート先 — ビデオの概要

次のビデオでは、2種類の宛先に関する特別な点を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

<br> 

### 拡張機能

Adobe Real-time CDPは、Experience Platform Launchの機能と柔軟性を活用して、Adobe Real-time CDPインターフェイスにLaunch拡張機能を組み込みます。

>[!TIP]
>
>使用例やインターフェイスでの検索方法など、エクスペリエンスプラットフォーム起動拡張機能について詳しくは、 [起動拡張機能の概要を参照してください](/help/rtcdp/destinations/experience-platform-launch-extensions.md)。

起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。 拡張機能は、宛先の **イベント転送** (Destination)タイプと考えてください。 これは、生のイベントデータのみを転送する、宛先プラットフォームとの統合のよりシンプルなタイプです。 例えば、 [Gainsightパーソナライゼーション拡張機能](/help/rtcdp/destinations/gainsight-extension.md) 、またはCustomer拡張機能の [Confirmit Voice](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)。

![他の宛先と比較したエクスペリエンスプラットフォーム起動の拡張](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

<br> 

### 接続と拡張を使用するタイミング

マーケティング担当者は、接続と拡張の組み合わせを使用して、使用事例に対処できます。

接続は、アクティベーションのために完全な一元化された顧客プロファイルまたは顧客セグメントを活用する必要がある場合に役立ちます。 例えば、パーソナライズされたメッセージをそのユーザーに配信する前に、Analyticsシステムの行動データとアップロードされたCRMデータを結合して、特定のセグメントのユーザーを絞り込む場合は、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化を行う場合に便利です。 例えば、行動データを外部システムに転送する必要がある場合、特定のユーザーのファイル上の他のデータソースに結合する必要はありません。

<br> 

## 宛先カテゴリ

リンク先カタログのカテゴリと拡張 [子は、目的のマーケティングの用途に応じて、リンク先のストレージ(](https://platform.adobe.com/destination/catalog) Advertising ****、 **Cloudプラットフォーム、調査プラットフォーム、電子メールマーケティング**********、email marketingなど)別にグループ化されます。 各カテゴリおよび各カテゴリに含まれる宛先について詳しくは、 [Destinationsカタログドキュメントを参照してください](/help/rtcdp/destinations/destinations-catalog.md)。

![宛先カテゴリ](/help/rtcdp/destinations/assets/destination-categories-menu.png)

