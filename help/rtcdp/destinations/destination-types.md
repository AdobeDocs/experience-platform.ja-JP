---
title: 宛先のタイプとカテゴリ
seo-title: 宛先のタイプとカテゴリ
description: 'Adobe Real-time Customer Data Platformでは、プロファイル/セグメントエクスポート先はイベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメントと適格なプロファイルをエクスポート先に適用します。 起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。 '
seo-description: Adobe Real-time Customer Data Platformでは、プロファイル/セグメントエクスポート先はイベントデータを取得し、他のデータソースと組み合わせて、セグメント化を適用し、セグメントと適格なプロファイルをエクスポート先に適用します。 起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。
translation-type: tm+mt
source-git-commit: bfcbc56f05fa1c3b5fafd57b1166e50130b6007d

---


# 宛先のタイプとカテゴリ

このページを読んで、Adobe Real-time Customer Data Platformの様々なタイプとカテゴリを理解してください。

## 宛先のタイプ

アドビのリアルタイム顧客データプラットフォームでは、接続と拡張の2種類の宛先を区別します。 接続先には、接続エクスポート先とセグメントエクスポートプロファイルの2種類があります。

![宛先のタイプ](/help/rtcdp/destinations/assets/types-of-destinations.png)

<br> 

### 接続

**プロファイルエクスポート** /セグメントエクスポート先Adobe Real-time Customer Data Platformのセグメントエクスポート先は **、イベントデータを取り込み、他のデータソースと組み合わせて、リアルタイム顧客プロファイルを形成し、セグメント化**[](https://docs.adobe.com/content/help/en/experience-platform/profile/home.html)、セグメント化を適用し、セグメントと正規プロファイルを宛先に適用します。

<br> 

#### プロファイルの書き出し先

プロファイル書き出し先は、プロファイルや属性を含むファイルを生成します。これらの宛先は生データを使用し、多くの場合、電子メールアドレスを主キーとして使用します。宛先は [Amazon S3クラウドストレージ](/help/rtcdp/destinations/amazon-s3-destination.md) （宛先）の一例で、エクスポートを含むファイルをデポジットできます。

#### セグメントの書き出し先

セグメントのエクスポート先は、プロファイルと、それらが資格を持つセグメントを宛先プラットフォームに送信します。 これらの宛先では、セグメント ID またはユーザー ID を使用します。このような表示先の例と [しては、](/help/rtcdp/destinations/google-dv360-destination.md) Google Display [、Video 360](/help/rtcdp/destinations/google-ads-destination.md) 、Google Adsなどの広告の表示先が挙げられます。

#### プロファイルの書き出しとセグメントの書き出し先 — ビデオの概要

次のビデオでは、2種類の宛先の特定の状況を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29707?quality=12)

<br> 

### 拡張機能

Adobe Real-time CDPは、Experience Platform Launchの機能と柔軟性を活用して、Adobe Real-time CDPインターフェイスにLaunch拡張機能を組み込みます。

起動拡張機能は、生のイベントデータを複数のタイプの宛先に転送します。 拡張子は、宛先の **イベント転送** (Destination Forwarding)タイプと考えます。 これは、生のプラットフォームデータの転送のみを行う宛先プラットフォームとの統合のより簡単なタイプでイベントします。 例えば、勝者パーソナライゼ [ーション拡張や](/help/rtcdp/destinations/gainsight-extension.md) 、顧客拡 [張の確認音声などがあります](/help/rtcdp/destinations/confirmit-digital-feedback-extension.md)。

エクスペリエンスプラットフォーム起動の拡張機能について詳しくは、起動の拡張機能の概要 [を参照してくださ](/help/rtcdp/destinations/experience-platform-launch-extensions.md)い。


![エクスペリエンスプラットフォーム起動の拡張と他の宛先との比較](/help/rtcdp/destinations/assets/launch-and-other-destinations.png)

<br> 

### 接続と拡張機能を使用する場合

マーケティング担当者は、接続と拡張の組み合わせを使用して、使用事例に対処できます。

接続は、完全な一元化された顧客プロファイルまたは顧客セグメントをアクティベーションに活用する必要がある場合に役立ちます。 例えば、アップロードされたCRMデータを含むAnalyticsシステムの行動データに結合して、特定のセグメントのユーザーをそのユーザーにパーソナライズされたメッセージを配信する前に資格を得る場合は、接続を使用します。

拡張機能は、イベントデータを使用してアクションをトリガーしたり、外部環境でセグメント化を行う場合に役立ちます。 例えば、行動データを、特定のユーザーのファイル上の他のデータソースに結合せずに、外部システムに転送する必要がある場合です。

<br> 

## 宛先カテゴリ

宛先カタログ内の宛先と拡張子は、目的の [カテゴリ](https://platform.adobe.com/destination/catalog) (**Advertising**、 **Extensions** Platforms、 **Email** marketing Cloud、Email marketing Marketing Cloud、Email **marketing** Marketing Cloudなど)に応じて、目的のカタログ別にグループ化されます。 各カテゴリと各カテゴリに含まれる宛先について詳しくは、宛先カタログのドキュメントを参照 [してください](/help/rtcdp/destinations/destinations-catalog.md)。

![宛先カテゴリ](/help/rtcdp/destinations/assets/destination-categories.png)

