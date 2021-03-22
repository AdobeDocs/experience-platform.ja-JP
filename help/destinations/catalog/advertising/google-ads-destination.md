---
keywords: Google広告；Google広告；Google広告；Google広告；Google広告；Google広告
title: Google Ads接続
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
translation-type: tm+mt
source-git-commit: 7d579d85d427c45f39d000288ed883c7ffd003bf
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 19%

---


# [!DNL Google Ads] connection

[!DNL Google Ads](旧称 [!DNL Google AdWords])は、企業がテキストベースの検索、グラフィックディスプレイ、 [!DNL YouTube] ビデオ、アプリ内モバイルディスプレイにわたってペイパークリック広告を行えるようにするオンライン広告サービスです。

## 宛先の詳細{#specifics}

[!DNL Google Ads]宛先に固有の次の詳細を確認します。

* アクティブ化されたオーディエンスは、プラットフォーム[!DNL Google]でプログラム的に作成されます。
* 現在、プラットフォームには、アクティベーションの成功を検証するための測定指標は含まれていません。 統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>[!DNL Google Ads]で最初の宛先を作成する場合で、以前(Audience Managerや他のアプリケーションで)Experience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングかカスタマーケアにご連絡ください。 以前にAudience ManagerでGoogle統合を設定していた場合、Platformへの繰り越しを設定したID同期。

## サポートされるID{#supported-identities}

[!DNL Google Ad Manager] は、次の表に示すIDのアクティベーションをサポートしています。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソースIDがGAID名前空間の場合は、このターゲットIDを選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソースIDがIDFA名前空間の場合は、このターゲットIDを選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html)、別名 [!DNL Device ID]。数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Googleは、カリフォルニア州のターゲットユーザーに対しては[AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en)を使用し、その他すべてのユーザーに対してはGoogle Cookie IDを使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] このIDを使用して、カリフォルニア以外のターゲットユーザーを対象にします。 |
| RIDA | 広告のRoku ID。 このIDはRokuデバイスを一意に識別します。 |  |
| MAID | Microsoft広告ID。 このIDは、Windows 10を実行するデバイスを一意に識別します。 |  |
| AmazonファイアテレビID | このIDは、Amazonファイアテレビを一意に識別します。 |  |

## エクスポートの種類{#export-type}

**セグメントエクスポート**  — セグメント(オーディエンス)のすべてのメンバーをGoogleのエクスポート先にエクスポートします。

## 前提条件

### 既存の[!DNL Google Ads]アカウント

>[!IMPORTANT]
>
> [!DNL Google] では、サードパーティベンダーとの新しい [!DNL Google Ads] cookie統合が廃止されました。次の節の許可リスト手順を実行するには、[!DNL Google Ads]との既存の統合が必要です。 その結果、[!DNL Google Ads]を使用する場合の推奨アプローチは、[!DNL Google Customer Match]統合を設定することです。 [!DNL Google Customer Match]統合の作成についての詳細は、[[!DNL Google Customer Match]](./google-customer-match.md)接続の作成に関するチュートリアルをお読みください。

### 許可リスト

>[!NOTE]
>
>この許可リストは、Platformで最初の[!DNL Google Ads]宛先を設定する前に必須です。 宛先を作成する前に、[!DNL Google]が以下に説明する許可リストプロセスを完了していることを確認してください。

Platformで[!DNL Google Ads]宛先を作成する前に、[!DNL Google]に問い合わせて、許可されているデータプロバイダーのリストにAdobeを送信し、許可リストにアカウントを追加する必要があります。 [!DNL Google]に連絡し、次の情報を入力します。

* **アカウントID** :これは、AdobeのアカウントID [!DNL Google]です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **顧客ID** :これは、Adobeの顧客アカウントID [!DNL Google]です。この ID を取得するには、アドビカスタマーケアまたはアドビの担当者に問い合わせてください。
* アカウントのタイプ：**AdWords**
* **Google AdWords ID** :これはお客様のIDで [!DNL Google]す。通常、ID の形式は 123-456-7890 です。

## 宛先の設定

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Google Ads]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![Google 広告の宛先への接続](../../assets/catalog/advertising/google-ads-destination/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

宛先を作成ワークフローの&#x200B;**セットアップ**&#x200B;手順で、宛先の[!UICONTROL 基本情報]を入力します。

![Google 広告の基本情報](../../assets/catalog/advertising/google-ads-destination/setup.png)

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントのタイプ]**：AdWords は利用可能な唯一のオプションです。
* **[!UICONTROL アカウントID]**:アカウントIDをに入力し [!DNL Google Ads]ます。通常、ID の形式は 123-456-7890 です。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

## セグメントを[!DNL Google Ads]にアクティブ化

[!DNL Google Ads]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。

## 書き出されたデータ

データが[!DNL Google Ads]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Google Ads]アカウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。