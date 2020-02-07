---
title: Googleの宛先
seo-title: Googleの宛先
description: Adobe Real-time CDPは、Googleと統合され、DV360、Google Ad Manager、Google adWordsおよびGoogle adXでデータを実行およびアクティブ化できます。
seo-description: Adobe Real-time CDPは、Googleと統合され、DV360、Google Ad Manager、Google adWordsおよびGoogle adXでデータを実行およびアクティブ化できます。
translation-type: tm+mt
source-git-commit: 5396bee00044e5046bd768a863fceca0aec1d24e

---


# Googleの宛先

## 概要

Adobe Real-time CDPは、Googleと統合され、DV360、Google Ad Manager、Google adWords DisplayおよびGoogle adXでデータを実行およびアクティブ化できます。

## 宛先の仕様

Googleの宛先に固有の次の詳細に注意してください。

* 次のIDをGoogleの送信先 [に送信できます](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_namespace_overview/identity_namespace_overview.md) 。 **Google cookie ID、IDFA、GAID**。
* アクティブ化されたオーディエンスは、Googleプラットフォームでプログラム的に作成されます。
* Adobe Real-time CDPには、現在、アクティブ化の成功を検証するための測定指標は含まれていません。 統合を検証し、データのドロップオフについて理解するには、Googleでのオーディエンス数を参照します。

## 前提条件

### ホワイトリスト

Adobe Real-time CDPでGoogleの宛先に接続する前に、Googleに連絡して、ホワイトリストに登録するアカウントを依頼する必要があります。 Googleに連絡し、次の情報を入力します。

* **アカウントID** :これは、Googleを使用するアドビのアカウントIDです。 このIDを取得するには、アドビサポートにお問い合わせください。
* **顧客ID** :これは、Googleを使用するアドビの顧客アカウントIDです。 このIDを取得するには、アドビサポートにお問い合わせください。
* **パートナーID** :これは、Googleを使用する3桁のパートナーIDです。
* **ネットワークID** :これはGoogleのアカウントです。
* **オーディエンスリンクID** :これはGoogleのアカウントです。
* アカウントの種類。 広告主の招待、 **DFP****、** Words **、AdX** Partner **の******&#x200B;招待、


## 宛先に接続

1. 接続/ **[!UICONTROL 宛先で]**、「Google」を選択し、「宛先を作成」を **[!UICONTROL 選択します]**。
   ![Googleの宛先に接続](/help/rtcdp/destinations/assets/google-destination.png)

2. 接続先ウィザードで、接続先の基本情報を入力します。
   ![基本情報Google](/help/rtcdp/destinations/assets/google-basic-information.png)
* **名前**:この宛先の推奨名を入力します。
* **説明**:オプション。 例えば、この宛先を使用しているキャンペーンを指定できます。
* **アカウントタイプ**:Googleのアカウントに応じて、次のオプションを選択します。
   * Google DV360 `Invite advertiser` に使用
   * Google DV360 `Invite partner` に使用
   * Google Ad `DFP by Google` Managerで使用
   * Google広告 `AdWords` ワード表示に使用
   * Google adX `AdX buyer` で使用
* **アカウントID**:GoogleでアカウントIDを入力します。

>[!NOTE]
>
>Googleの保存先を設定する際は、Googleのアカウントマネージャーまたはアドビの担当者に問い合わせて、アカウントが該当する製品のタイプを確認してください。 Google DV360の場合は、Googleのアカウントマネージャーに、アカウントに該当する製品の種類を尋ねてください。 

## Googleへのセグメントのアクティブ化

Googleに対してセグメントをアクティブ化する方法については、「宛先へのデータのアクテ [ィブ化」を参照してくださ](/help/rtcdp/destinations/activate-destinations.md)い。