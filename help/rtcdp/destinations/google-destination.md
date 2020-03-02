---
title: Google の宛先
seo-title: Google の宛先
description: Adobe Real-time CDP は Google と統合され、DV360、Google Ad Manager、Google AdWords、Google AdX でデータを実行およびアクティブ化できます。
seo-description: Adobe Real-time CDP は Google と統合され、DV360、Google Ad Manager、Google AdWords、Google AdX でデータを実行およびアクティブ化できます。
translation-type: ht
source-git-commit: 5396bee00044e5046bd768a863fceca0aec1d24e

---


# Google の宛先

## 概要

Adobe Real-time CDP は、Google と統合され、DV360、Google Ad Manager、Google AdWords Display、Google AdX でデータを実行およびアクティブ化できます。

## 宛先の仕様

次の Google の宛先に固有な詳細に注意ください。

* Google の宛先に送信できる [ID](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_namespace_overview/identity_namespace_overview.md) は、**Google cookie ID、IDFA、GAID** です。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、データのドロップオフについて理解するには、Google でのオーディエンス数を参照します。

## 前提条件

### ホワイトリスト

Adobe Real-time CDP で Google の宛先に接続する前に、Google に連絡して、ホワイトリストに登録するアカウントを依頼する必要があります。Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。この ID を取得するには、アドビサポートにお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。この ID を取得するには、アドビサポートにお問い合わせください。
* **パートナー ID**：これは、3 桁の Google パートナー ID です。
* **ネットワーク ID**：これは Google のアカウントです。
* **オーディエンスリンク ID**：これは Google のアカウントです。
* アカウントの種類。**広告主招待**、**パートナー招待**、**DFP**、**AdWords**,、**AdX** から選択します。


## 宛先の接続

1. **[!UICONTROL 接続／宛先]**&#x200B;で「Google」を選択し、「**[!UICONTROL 宛先を作成]**」を選択します。
   ![Google の宛先への接続](/help/rtcdp/destinations/assets/google-destination.png)

2. 「宛先の接続」ウィザードで、宛先の基本情報を入力します。
   ![基本情報 Google](/help/rtcdp/destinations/assets/google-basic-information.png)
* **名前**:この宛先の名前を入力します。
* **説明**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **アカウントの種類**：Google のアカウントに応じて、次のオプションを選択します。
   * Google DV360 - `Invite advertiser` を使用
   * Google DV360 - `Invite partner` を使用
   * Google Ad Manager - `DFP by Google` を使用
   * Google AdWords Display - `AdWords` を使用
   * Google AdX - `AdX buyer` を使用
* **アカウント ID**：Google アカウントの ID を入力します。

>[!NOTE]
>
>Google の宛先を設定する際は、Google のアカウントマネージャーまたは Adobe 担当者に問い合わせて、アカウントが該当する製品の種類を確認してください。Google DV360 の場合は、Google のアカウントマネージャーに、アカウントに該当する製品の種類を尋ねてください。 

## Google へのセグメントのアクティブ化

Google に対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。