---
keywords: google ad manager;google ad;doubleclick;DoubleClick AdX;DoubleClick;Google Ad Manager;Google ad manager
title: Google Ad Manager接続
description: 'Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。  '
translation-type: tm+mt
source-git-commit: 950dc24e44a32cfd3e0cdde0fee967cb687c572e
workflow-type: tm+mt
source-wordcount: '754'
ht-degree: 30%

---


# [!DNL Google Ad Manager] connection

[!DNL Google Ad Manager]（以前は for Publishers または と呼ばれていました）は の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。[!DNL DoubleClick][!DNL DoubleClick AdX][!DNL Google]

## 宛先の仕様

[!DNL Google Ad Manager]宛先に固有の次の詳細を確認します。

* アクティブ化されたオーディエンスは、プラットフォーム[!DNL Google]でプログラム的に作成されます。
* 現在、プラットフォームには、アクティベーションの成功を検証するための測定指標は含まれていません。 統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>[!DNL Google Ad Manager]で最初の宛先を作成する場合で、以前(Audience Managerや他のアプリケーションで)Experience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングかカスタマーケアにご連絡ください。 以前にAudience Managerで[!DNL Google]統合を設定していた場合、設定したID同期はPlatformに持ち越します。

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

### 許可リスト

>[!NOTE]
>
>この許可リストは、Platformで最初の[!DNL Google Ad Manager]宛先を設定する前に必須です。 宛先を作成する前に、[!DNL Google]が以下に説明する許可リストプロセスを完了していることを確認してください。

Platformで[!DNL Google Ad Manager]宛先を作成する前に、[!DNL Google]に問い合わせて、許可されているデータプロバイダーのリストにAdobeを送信し、許可リストにアカウントを追加する必要があります。 [!DNL Google]に連絡し、次の情報を入力します。

* **アカウントID** :これは、AdobeのアカウントID [!DNL Google]です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **顧客ID** :これは、Adobeの顧客アカウントID [!DNL Google]です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **ネットワーク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* **オーディエンスリンク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* アカウントの種類。Google DFP または AdX 購入者。

## 宛先の設定

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、**[!DNL Google Ad Manager]**&#x200B;を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![Google Ad Manager の宛先の接続](../../assets/catalog/advertising/google-ad-manager/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

宛先を作成ワークフローの&#x200B;**セットアップ**&#x200B;手順で、宛先の[!UICONTROL 基本情報]を入力します。

![Google Ad Manager の基本情報](../../assets/catalog/advertising/google-ad-manager/setup.png)

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   *  for Publishers - `DFP by Google` を使用[!DNL DoubleClick]
   * `AdX buyer`を[!DNL Google AdX]に使用
* **[!UICONTROL アカウントID]**:アカウントIDをに入力し [!DNL Google]ます。これは、ネットワーク ID またはオーディエンスリンク ID です。通常、これは 8 桁の ID です。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

>[!NOTE]
>
>[!DNL Google Ad Manager]宛先を設定する際は、[!DNL Google Account Manager]またはAdobeの担当者に相談して、お持ちのアカウントのタイプをご確認ください。

## セグメントを[!DNL Google Ad Manager]にアクティブ化

[!DNL Google Ad Manager]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。

## 書き出されたデータ

データが[!DNL Google Ad Manager]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Google Ad Manager]アカウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。