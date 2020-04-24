---
title: Google Display と Video 360 の宛先
seo-title: Google Display と Video 360 の宛先
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
seo-description: 'Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。 '
translation-type: tm+mt
source-git-commit: 50e6b39c1eb0bda4f3b30991515fb1c13fa9ff87

---


# Google Display と Video 360 の宛先

## 概要

ディスプレイ&amp;ビデオ360（旧称DoubleClick Bid Manager）は、ディスプレイ、ビデオ、およびモバイルの在庫ソースでデジタルキャンペーンをターゲットにした再ターゲット化とオーディエンスを実行するためのツールです。

## 宛先の仕様

Google Display &amp; Video 360の表示先に固有の次の詳細に注意してください。

* 次のIDをGoogle Display [](../../identity-service/namespaces.md) &amp; Video 360の送信先に送信できます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV ID**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスのターゲット設定のサイズを理解するには、Googleでのオーディエンス数を参照します。

>[!IMPORTANT]
>
>Google Display &amp; Video 360を使用して最初のリンク先を作成し、以前(Adobeオーディエンスマネージャーなどのアプリケーションを使用して)Experience Cloud IDサービスで [](https://docs.adobe.com/content/help/en/id-service/using/id-service-api/methods/idsync.html) ID同期機能を有効にしていない場合は、アドビのコンサルティングまたはカスタマーケアに連絡して、ID同期を有効にしてください。 以前にオーディエンスマネージャーでGoogle統合を設定していた場合、設定したID同期はAdobe Real-time CDPに引き継がれます。

## 前提条件

### ホワイトリスト

>[!NOTE]
>
>ホワイトリストは、Adobe Real-time CDPで最初のGoogle Display &amp; Video 360の宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明するホワイトリストのプロセスを完了していることを確認してください。

Adobe Real-time CDPでGoogle Display &amp; Video 360の宛先を作成する前に、Googleに連絡して、データプロバイダーとしてホワイトリストに登録するようアドビに求め、お客様のアカウントをホワイトリストに登録する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **アカウントの種類**:オーディエンス **[!DNL Invite advertiser]** をDisplay &amp; Video 360アカウント内の特定のブランドに対してのみ共有する場合、またはDisplay &amp; Video 360アカウント内のすべてのブランドに対してオーディエンスを共有する場合に **[!DNL Invite partner]** 使用します。

## 宛先の作成

1. In **[!UICONTROL Connections > Destinations]**, select Google Display &amp; Video 360, and select **[!UICONTROL Create destination]**.
   ![Googleディスプレイとビデオ360の接続](/help/rtcdp/destinations/assets/google-dv360-destination.png)

2. In the Create destination workflow, fill in the [!UICONTROL Basic Information] for the destination.
   ![基本情報Googleディスプレイ&amp;ビデオ360](/help/rtcdp/destinations/assets/google-dv360-basic-information.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * ディスプレイ `Invite Advertiser` およびビデオ360アカウントでオーディエンスを特定のブランドに対してのみ共有できるようにする場合に使用します。
   * ディスプレイ `Invite Partner` およびビデオ360アカウント内のすべてのブランドに対してオーディエンスを共有できるようにする場合に使用します。
* **[!UICONTROL アカウントID]**:またはアカウントID **[!DNL Invite partner]** にGoogle **[!DNL Invite advertiser]** を入力します。 通常、これは6桁または7桁のIDです。

>[!NOTE]
>
>Google Display &amp; Video 360の宛先を設定する際は、Googleのアカウントマネージャーまたはアドビの担当者にご相談のうえ、お持ちのアカウントのタイプをご理解ください。

## Google Display &amp; Video 360へのセグメントのアクティブ化

For instructions on how to activate segments to Google Display &amp; Video 360, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).