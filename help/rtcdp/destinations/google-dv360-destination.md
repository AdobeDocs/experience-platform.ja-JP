---
title: Google Display と Video 360 の宛先
seo-title: Google Display と Video 360 の宛先
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
seo-description: 'Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。 '
translation-type: tm+mt
source-git-commit: a251d843401d2f092e368a4cdac217171fa4687f
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 56%

---


# [!DNL Google Display & Video 360] 宛先

## 概要

[!DNL Display & Video 360]（旧称 ）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。[!DNL DoubleClick Bid Manager]

## 宛先の仕様

Note the following details that are specific to [!DNL Google Display & Video 360] destinations:

* You can send the following [identities](../../identity-service/namespaces.md) to [!DNL Google Display & Video 360] destinations: **Google cookie ID, IDFA, GAID, Roku IDs, Microsoft IDs, Amazon Fire TV IDs**.
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* アドビのリアルタイム CDP には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲティングの規模を理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>Google Display &amp; Video 360 を使用して最初の宛先を作成する場合で、以前に Experience Cloud ID サービスにおいて、Adobe Audience Manager や他のアプリケーションとの間で [ID 同期機能](https://docs.adobe.com/content/help/ja-JP/id-service/using/id-service-api/methods/idsync.html)を有効にしていないときには、アドビコンサルティングまたはカスタマーケアに連絡して、ID 同期を有効にしてください。Audience Manager で以前に Google インテグレーションを設定していた場合、設定した ID 同期は アドビのリアルタイム CDP に引き継がれます。

## 前提条件

### 許可リスト

>[!NOTE]
>
>許可リストは、AdobeReal-time CDPで最初の [!DNL Google Display & Video 360] 宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

AdobeReal-time CDPで [!DNL Google Display & Video 360] 宛先を作成する前に、Googleに連絡して、許可されているデータプロバイダのリストにAdobeし、許可リストにアカウントを追加するように求める必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先の設定

1. **[!UICONTROL 接続]** / **[!UICONTROL 宛先]**、を選択し、「 [!DNL Google Display & Video 360]設定 ****」を選択します。
   ![Google Display &amp; Video 360 の宛先への接続](/help/rtcdp/destinations/assets/google-dv360-destination.png)

   >[!NOTE]
   >
   >このリンク先との接続が既に存在する場合は、リンク先のカードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](/help/rtcdp/destinations/destinations-workspace.md#catalog) 」セクションを参照してください。

2. 宛先を作成ワークフローの **セットアップ** 手順で、宛先の [!UICONTROL 基本情報] 、およびこの宛先に適用するマーケティングの使用例を入力します。 <br>

   ![Google Display &amp; Video 360 の基本情報](/help/rtcdp/destinations/assets/dv360-setup-step.png)
* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL アカウント ID]**：**[!DNL Invite partner]** または **[!DNL Invite advertiser]** のアカウント ID に Google アカウントの ID を入力します。通常、これは 6 桁または 7 桁の ID です。
* **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。

>[!NOTE]
>
>When setting up a [!DNL Google Display & Video 360] destination please work with your [!DNL Google Account Manager] or Adobe representative to understand which account type you have.

## Activate segments to [!DNL Google Display & Video 360]

For instructions on how to activate segments to [!DNL Google Display & Video 360], see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).

## 書き出されたデータ

データが正常に宛先にエクスポートされたかどうかを確認するには、アカウントを確認して [!DNL Google Display & Video 360] く [!DNL Google Display & Video 360] ださい。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。