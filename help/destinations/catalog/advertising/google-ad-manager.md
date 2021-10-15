---
keywords: google ad manager;google ad;doubleclick;DoubleClick AdX;DoubleClick;Google Ad Manager;Google ad manager;DFP
title: Google Ad Manager 接続
description: Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。
exl-id: e93f1bd5-9d29-43a1-a9a6-8933f9d85150
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 27%

---

# [!DNL Google Ad Manager] 接続

## 概要 {#overview}

[!DNL Google Ad Manager]（旧称：DFP） [!DNL DoubleClick for Publishers] や ( [!DNL DoubleClick AdX]DFP) は、パブリッシャーがビデオやモバイルアプリを通じて Web サイト上の広告の表示を管理する手段を提供する、の広告提供プラットフォームで [!DNL Google] す。

## 宛先の詳細 {#specifics}

次の [!DNL Google Ad Manager] の宛先に固有の詳細に注意してください。

* アクティブ化されたオーディエンスは、[!DNL Google] プラットフォームでプログラムによって作成されます。
* [!DNL Platform] 現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

## サポートされる ID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明する ID のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html)（別名） [!DNL Device ID]。数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Googleは [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en) を使用してカリフォルニアのユーザーをターゲットし、他のすべてのユーザーのGoogle Cookie ID を使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア以外のユーザーをターゲット設定します。 |
| RIDA | 広告用の Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。 この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  — セグメント（オーディエンス）のすべてのメンバーをGoogleの宛先に書き出します。

## 前提条件

[!DNL Google Ad Manager] で最初の宛先を作成したい場合で、以前に (Audience Managerや他のアプリケーションで )Experience CloudID サービスで [ID 同期機能 ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに問い合わせて ID 同期を有効にしてください。 以前にAudience Managerで [!DNL Google] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

## 許可リスト

>[!NOTE]
>
>許可リストは、Platform で最初の [!DNL Google Ad Manager] 宛先を設定する前に必須です。 宛先を作成する前に、[!DNL Google] が以下に説明する許可リストプロセスを完了していることを確認してください。

Platform で [!DNL Google Ad Manager] の宛先を作成する前に、[!DNL Google] に問い合わせて、許可されたデータプロバイダーのリストにAdobeを登録し、お使いのアカウントを許可リストに追加する必要があります。 [!DNL Google] に問い合わせ、次の情報を入力します。

* **アカウント ID**:AdobeのGoogleアカウント ID。アカウント ID:87933855.
* **顧客 ID**:Adobeの顧客アカウント ID とGoogle。顧客 ID:89690775.
* **ネットワーク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* **オーディエンスリンク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* アカウントの種類。Google DFP または AdX 購入者。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   *  for Publishers - `DFP by Google` を使用[!DNL DoubleClick]
   * [!DNL Google AdX] には `AdX buyer` を使用します
* **[!UICONTROL アカウント ID]**:アカウント ID にを入力しま [!DNL Google]す。これは、ネットワーク ID またはオーディエンスリンク ID です。通常、これは 8 桁の ID です。

>[!NOTE]
>
>[!DNL Google Ad Manager] の宛先を設定する際は、[!DNL Google Account Manager] またはAdobeの担当者にお問い合わせのうえ、お持ちのアカウントの種類をご確認ください。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## エクスポートされたデータ

データが [!DNL Google Ad Manager] の宛先に正常に書き出されたかどうかを確認するには、[!DNL Google Ad Manager] アカウントを確認します。 アクティブ化に成功した場合は、オーディエンスがアカウントに入力されます。
