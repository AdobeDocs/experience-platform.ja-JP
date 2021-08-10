---
keywords: Google広告；google広告；google adwords;Google AdWords;Google Adwords
title: Google広告接続
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
exl-id: 7143f476-49a8-42aa-bfb4-b11fc2b8f5c3
source-git-commit: 7e2f6f54e754c52c8de7f98372d041b2a6520d46
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 17%

---

# [!DNL Google Ads] 接続

## 概要 {#overview}

[!DNL Google Ads]（旧称）は、テキ [!DNL Google AdWords]ストベースの検索、グラフィックディスプレイ、ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を可能にするオンライン広告サービスで [!DNL YouTube] す。

## 宛先の詳細 {#specifics}

次の[!DNL Google Ads]宛先に固有の詳細に注意してください。

* アクティブ化されたオーディエンスは、プラットフォーム[!DNL Google]でプログラムによって作成されます。
* [!DNL Platform] は、現在、アクティブ化の成功を検証するための測定指標を含んでいません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>[!DNL Google Ads]で最初の宛先を作成したい場合で、(Audience Managerや他のアプリケーションで)以前にExperience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに連絡してID同期を有効にしてください。 以前にGoogle統合をAudience Managerで設定していた場合、設定したID同期はPlatformに引き継がれます。

## サポートされるID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明するIDのアクティブ化をサポートしています。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソースIDがGAID名前空間の場合は、このターゲットIDを選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソースIDがIDFA名前空間の場合は、このターゲットIDを選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html)(別名 [!DNL Device ID])数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Googleは、[AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en)を使用してカリフォルニアのユーザーをターゲット設定し、他のすべてのユーザーのGoogle Cookie IDを使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、このIDを使用してカリフォルニア以外のユーザーをターゲット設定します。 |
| RIDA | 広告用のRoku ID。 このIDは、Rokuデバイスを一意に識別します。 |  |
| MAID | Microsoft広告ID。 このIDは、Windows 10を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | このIDは、Amazon Fire TVを一意に識別します。 |  |

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  — セグメント（オーディエンス）のすべてのメンバーをGoogleの宛先に書き出します。

## 前提条件

### 既存の[!DNL Google Ads]アカウント

>[!IMPORTANT]
>
> [!DNL Google] では、サードパーテ [!DNL Google Ads] ィベンダーとの新しいcookie統合が廃止されました。次の節の許可リスト手順を実行するには、[!DNL Google Ads]との既存の統合が必要です。 その結果、[!DNL Google Ads]を使用する場合の推奨アプローチは、[!DNL Google Customer Match]統合を設定することです。 [!DNL Google Customer Match]統合の作成について詳しくは、[[!DNL Google Customer Match]](./google-customer-match.md)接続の作成に関するチュートリアルを参照してください。

### 許可リスト

>[!NOTE]
>
>許可リストは、Platformで最初の[!DNL Google Ads]宛先を設定する前に必須です。 宛先を作成する前に、[!DNL Google]によって後述の許可リスト処理が完了していることを確認してください。

Platformで[!DNL Google Ads]の宛先を作成する前に、[!DNL Google]に連絡して、許可されたデータプロバイダーのリストにAdobeを登録し、お使いのアカウントを許可リストに追加する必要があります。 [!DNL Google]に連絡し、次の情報を提供します。

* **アカウントID**:AdobeのアカウントIDがGoogleに設定されている。アカウントID:87933855.
* **顧客ID**:Adobeの顧客アカウントID（Googleを使用）。顧客ID:89690775.
* アカウントのタイプ：**AdWords**
* **Google AdWords ID**:これは、を使用したIDで [!DNL Google]す。通常、ID の形式は 123-456-7890 です。

## 宛先の設定

**[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;で、「[!DNL Google Ads]」を選択し、「**[!UICONTROL 設定]**」を選択します。

![Google 広告の宛先への接続](../../assets/catalog/advertising/google-ads-destination/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL アクティブ化]**」ボタンが表示されます。 **[!UICONTROL アクティブ化]**&#x200B;と&#x200B;**[!UICONTROL 設定]**&#x200B;の違いについて詳しくは、宛先ワークスペースのドキュメントの[カタログ](../../ui/destinations-workspace.md#catalog)の節を参照してください。

宛先の作成ワークフローの&#x200B;**設定**&#x200B;手順で、宛先の[!UICONTROL 基本情報]を入力します。

![Google 広告の基本情報](../../assets/catalog/advertising/google-ads-destination/setup.png)

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントのタイプ]**：AdWords は利用可能な唯一のオプションです。
* **[!UICONTROL アカウントID]**:アカウントIDにを入力しま [!DNL Google Ads]す。通常、ID の形式は 123-456-7890 です。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、宛先にデータを書き出す目的を示します。Adobe定義のマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。 マーケティングアクションについて詳しくは、「[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)」を参照してください。

## [!DNL Google Ads]に対してセグメントをアクティブ化

[!DNL Google Ads]に対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## エクスポートされたデータ

データが[!DNL Google Ads]の宛先に正常に書き出されたかどうかを確認するには、[!DNL Google Ads]アカウントを確認します。 アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
