---
title: Google Display と Video 360 の宛先
seo-title: Google Display と Video 360 の宛先
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
seo-description: 'Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。 '
translation-type: tm+mt
source-git-commit: dc5ee796ca390d06fc8e08bd6c30e88a0d96dd53
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 27%

---


# Google Display と Video 360 の宛先

## 概要

ディスプレイ&amp;ビデオ360（旧称DoubleClick入札マネージャ）は、ディスプレイ、ビデオ、モバイルの在庫ソースにわたって、デジタルキャンペーンをターゲットにした再ターゲット化とオーディエンスを実行するためのツールです。

## 宛先の仕様

Google Display &amp; Video 360の表示先に固有の次の詳細に注意してください。

* 次の [IDをGoogle Display &amp; Video](../../identity-service/namespaces.md) 360の送信先に送信できます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。Googleでオーディエンス数を参照して統合を検証し、オーディエンスのターゲット設定サイズを把握する。

>[!IMPORTANT]
>
>Google Display &amp; Video 360での最初のリンク先を作成する予定で、 [ID同期機能を以前(Adobeオーディエンスマネージャーなどのアプリケーションを使用して](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) )Experience Cloud IDサービスで有効にしていない場合は、アドビコンサルティングまたはカスタマーケアにご連絡ください。 Google統合を以前にオーディエンスマネージャーで設定している場合、設定したID同期はAdobe Real-time CDPに繰り越されます。

## 前提条件

### リストを許可

>[!NOTE]
>
>許可リストは、Adobe Real-time CDPで最初のGoogle Display &amp; Video 360の宛先を設定する前に必須です。 宛先を作成する前に、以下に説明するリストの許可プロセスがGoogleによって完了していることを確認してください。

Adobe Real-time CDPでGoogle Display &amp; Video 360の宛先を作成する前に、Googleに連絡して、Adobeに対して、許可されているデータプロバイダーのリストに加え、お客様のアカウントを許可リストに追加するよう求める必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **アカウントの種類**: オーディエンス **[!DNL Invite advertiser]** をDisplay &amp; Video 360アカウント内の特定のブランドに対してのみ共有できるようにする場合、またはを使用して、オーディエンスをDisplay &amp; Video 360アカウント内のすべてのブランドに共有でき **[!DNL Invite partner]** るようにします。

## 宛先の作成

1. In **[!UICONTROL Connections > Destinations]**, select Google Display &amp; Video 360, and select **[!UICONTROL Create destination]**.
   ![Googleディスプレイとビデオ360の宛先の接続](/help/rtcdp/destinations/assets/google-dv360-destination.png)

2. In the Create destination workflow, fill in the [!UICONTROL Basic Information] for the destination. <br>

   ![Googleディスプレイ&amp;ビデオの基本情報360](/help/rtcdp/destinations/assets/google-dv360-basic-information.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360アカウント `Invite Advertiser` で、オーディエンスを特定のブランドにのみ共有できるようにする場合に使用します。
   * を使用 `Invite Partner` して、Display &amp; Video 360アカウント内のすべてのブランドにオーディエンスを共有できます。
* **[!UICONTROL アカウントID]**: Googleで、またはア **[!DNL Invite partner]****[!DNL Invite advertiser]** カウントIDを入力します。 通常、これは6 ～ 7桁のIDです。

>[!NOTE]
>
>Google Display &amp; Video 360の配信先を設定する場合は、Googleのアカウントマネージャーまたはアドビの担当者に問い合わせて、お持ちのアカウントのタイプをご確認ください。

## Google Display &amp; Video 360へのセグメントのアクティブ化

For instructions on how to activate segments to Google Display &amp; Video 360, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).