---
title: Google Display と Video 360 の宛先
seo-title: Google Display と Video 360 の宛先
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
seo-description: 'Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。 '
translation-type: tm+mt
source-git-commit: 6f680a60c88bc5fee6ce9cb5a4f314c4b9d02249
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 25%

---


# [!DNL Google Display & Video 360] 宛先

## 概要

[!DNL Display & Video 360](旧称 [!DNL DoubleClick Bid Manager])は、ディスプレイ、ビデオ、モバイルの在庫ソースでデジタルキャンペーンをターゲットにした再ターゲット化とオーディエンスを実行するためのツールです。

## 宛先の仕様

Note the following details that are specific to [!DNL Google Display & Video 360] destinations:

* 次の [IDを宛先に送信でき](../../identity-service/namespaces.md)[!DNL Google Display & Video 360] ます。 **Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV**。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラム的に作成されます。
* Adobe Real-time CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。Googleでオーディエンス数を参照して統合を検証し、オーディエンスのターゲット設定サイズを把握する。

>[!IMPORTANT]
>
>Google Display &amp; Video 360での最初のリンク先を作成する予定で、以前(Adobe Audience Managerや他のExperience Cloudを含む)IDサービスで [](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html) ID同期機能を有効にしていない場合は、アドビコンサルティングまたはカスタマーケアにご連絡ください。 以前にAudience ManagerでGoogle統合を設定した場合、設定したID同期はAdobe Real-time CDPに繰り越します。

## 前提条件

### 許可リスト

>[!NOTE]
>
>許可リストは、Adobe Real-time CDPで最初の [!DNL Google Display & Video 360] 宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

Adobe Real-time CDPで [!DNL Google Display & Video 360] 宛先を作成する前に、Googleに連絡して、許可されているデータプロバイダーのリストにアドビを配置し、お客様のアカウントを許可リストに追加するように依頼する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。このIDを取得するには、アドビカスタマーケアまたはアドビの担当者にお問い合わせください。
* **アカウントの種類**: オーディエンス **[!DNL Invite advertiser]** をDisplay &amp; Video 360アカウント内の特定のブランドに対してのみ共有できるようにする場合、またはを使用して、オーディエンスをDisplay &amp; Video 360アカウント内のすべてのブランドに共有でき **[!DNL Invite partner]** るようにします。

## 宛先の作成

1. **[!UICONTROL 接続/宛先]**&#x200B;を選択し、「宛先を [!DNL Google Display & Video 360]作成 ****」を選択します。
   ![Googleディスプレイとビデオ360の宛先の接続](/help/rtcdp/destinations/assets/google-dv360-destination.png)

2. 宛先を作成ワークフローの **セットアップ** 手順で、宛先の [!UICONTROL 基本情報] 、およびこの宛先に適用するマーケティングの使用例を入力します。 <br>

   ![Googleディスプレイ&amp;ビデオの基本情報360](/help/rtcdp/destinations/assets/dv360-setup-step.png)
* **[!UICONTROL 名前]**:この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360アカウント `Invite Advertiser` で、オーディエンスを特定のブランドにのみ共有できるようにする場合に使用します。
   * を使用 `Invite Partner` すると、Display &amp; Video 360アカウント内のすべてのブランドにオーディエンスを共有できます。
* **[!UICONTROL アカウントID]**: Googleで、またはア **[!DNL Invite partner]****[!DNL Invite advertiser]** カウントIDを入力します。 通常、これは6 ～ 7桁のIDです。
* **[!UICONTROL マーケティングの使用例]**: マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 アドビ定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 アドビが定義した個々のマーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

>[!NOTE]
>
>発行先を設定する場合は、担当のアドビ担当者 [!DNL Google Display & Video 360][!DNL Google Account Manager] またはアドビ担当者にご相談のうえ、お持ちのアカウントのタイプをご確認ください。

## Activate segments to [!DNL Google Display & Video 360]

For instructions on how to activate segments to [!DNL Google Display & Video 360], see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).