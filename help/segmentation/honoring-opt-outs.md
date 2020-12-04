---
keywords: Experience Platform;home;popular topics;opt-out;Segmentation;Segmentation service;segmentation service;honor opt-outs;opt-outs;opt out;opt outs;
solution: Experience Platform
title: オプトアウトの遵守
topic: overview
description: 'Experience Platformを使用すると、顧客は、データの使用状況やストレージに関するオプトアウトリクエストをリアルタイム顧客プロファイル内で送信できます。 これらのオプトアウトリクエストは、カリフォルニア州消費者プライバシー法（CCPA）に含まれています。CCPA は、カリフォルニア州の在住者に対して、個人データにアクセスし削除する権利や、個人データが販売または開示されたか（そして誰に対して）を知る権利を付与しています。 '
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '1006'
ht-degree: 64%

---


# セグメントでのオプトアウトリクエストの遵守

[!DNL Experience Platform] 顧客が内部でのデータの使用状況やストレージに関するオプトアウトリクエストを送信でき [!DNL Real-time Customer Profile]ます。 These opt-out requests are part of the [!DNL California Consumer Privacy Act] (CCPA), which provides California residents with the right to access and delete their personal data and to know whether their personal data is sold or disclosed (and to whom).

顧客がオプトアウトしたら、組織はマーケティング活動のオーディエンスを生成する際に、これらのオプトアウトを遵守する必要があります。このドキュメントでは、オプトアウトリクエストの遵守に関する重要な詳細について説明します。

## はじめに

Honoring opt-out requests requires an understanding of the various [!DNL Adobe Experience Platform] services involved. オプトアウトリクエストを処理する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](./home.md):データからオーディエンスセグメントを作成でき [!DNL Real-time Customer Profile] ます。
- [[!DNL Experience Data Model (XDM)]](../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [[!DNL Adobe Experience Platform Privacy Service]](../privacy-service/home.md):組織が、内部の顧客データに関するデータのプライバシー規制へのコンプライアンスを自動化できるよう支援 [!DNL Platform]します。

## オプトアウト mixin 

In order to honor CCPA opt-out requests, one of the schemas that is a part of the union schema must contain the necessary [!DNL Experience Data Model] (XDM) opt-out fields. スキーマにオプトアウトフィールドを追加するために使用できる mixin が 2 つあります。それぞれの mixin については、以下の節で詳しく説明します。

- [プロファイルプライバシー](#profile-privacy)：様々なオプトアウトタイプ（一般または販売 / 共有）を取得するために使用します。
- [プロファイル環境設定の詳細](#profile-preferences-details)：特定の XDM チャネルのオプトアウトリクエストを取得するために使用します。

スキーマに mixin を追加するための詳細な手順については、次の XDM ドキュメントの「mixin の追加」の節を参照してください。
- [スキーマレジストリ API のチュートリアル](../xdm/api/getting-started.md)：スキーマレジストリ API を使用したスキーマの構築。
- [スキーマエディタのチュートリアル](../xdm/tutorials/create-schema-ui.md)：Platform のユーザーインターフェイスを使用したスキーマの構築。

ユーザーインターフェイスでオプトアウト mixin をスキーマに追加した例を次に示します。

![](images/opt-outs/opt-out-mixins-user-interface.png)

各 mixin の構造と、スキーマに寄与するフィールドについては、以下の節で概要を説明します。

### [!DNL Profile Privacy] {#profile-privacy}

The [!DNL Profile Privacy] mixin allows you to capture two kinds of CCPA opt-out requests from customers:

1. 一般的なオプトアウト
2. 販売 / 共有のオプトアウト

![](images/opt-outs/profile-privacy.png)

この [!DNL Profile Privacy] ミックスインには次のフィールドが含まれています。

- プライバシーオプトアウト（`privacyOptOuts`）：オプトアウトオブジェクトのリストを含む配列。
- オプトアウトタイプ（`optOutType`）：オプトアウトのタイプ。このフィールドは、次の 2 つの値を持つ可能性がある列挙型です。
   - 一般的なオプトアウト（`general_opt_out`）
   - 販売 / 共有のオプトアウト（`sales_sharing_opt_out`）
- オプトアウト値（`optOutValue`）：指定したオプトアウトタイプに基づいた、オプトアウトのアクティブ状態（オプトアウト信号の値とも呼ばれる）。このフィールドは、次の 4 つの値を持つ可能性がある列挙型です。
   - 指定なし（`not_provided`）：オプトアウトリクエストが指定されていません。
   - 保留中の検証（`pending`）：オプトアウトリクエストが検証待ちです。
   - オプトアウト（`out`）：顧客がオプトアウトしました。
   - オプトイン（`in`）：顧客がオプトインしました。
- オプトアウトタイムスタンプ（`timestamp`）：受信したオプトアウト信号のタイムスタンプ。

To view the full structure of the [!DNL Profile Privacy] mixin, please refer to the [XDM public GitHub repository](https://github.com/adobe/xdm/blob/master/schemas/context/profile-privacy.schema.json) or preview the mixin using the Platform UI.

### [!DNL Profile Preferences Details] {#profile-preferences-details}

The [!DNL Profile Preferences Details] mixin provides several fields that represent preferences for customer profiles (such as email format, preferred language, and time zone). この mixin に含まれるフィールドの 1 つである OptInOut （`optInOut`）を使用すると、個々のチャネルに対してオプトアウト値を設定できます。

![](images/opt-outs/profile-preferences-details.png)

ミックスインには、オプトアウトに関連する次のフィールドが含まれ [!DNL Profile Preferences Details] ます。

- OptInOut （`optInOut`）：各キーが通信チャネルの有効な既知の URI と、各チャネルのオプトアウトのアクティブ状態を表しているオブジェクト。各チャネルには、次の 4 つの値のいずれかを指定できます。
   - 指定なし（`not_provided`）：このチャネルにオプトアウトリクエストが指定されていません。
   - 保留中の検証（`pending`）：このチャネルのオプトアウトリクエストが検証待ちです。
   - オプトアウト（`out`）：顧客がこのチャネルをオプトアウトしました。
   - オプトイン（`in`）：顧客がこのチャネルをオプトインしました。
- グローバルオプトアウト（`globalOptout`）：true に設定されている場合、プロファイルのグローバルオプトアウトの上書きを設定するブール値です。このフィールドのデフォルト値は false です。

次の例の JSON は、OptInOut オブジェクトが様々な通信チャネルの複数のオプトアウト信号を取得する方法を示しています。

```json
{
  "xdm:optInOut": {
    "https://ns.adobe.com/xdm/channels/email": "pending",
    "https://ns.adobe.com/xdm/channels/phone": "out",
    "https://ns.adobe.com/xdm/channels/sms": "in",
    "https://ns.adobe.com/xdm/channels/fax": "not_provided",
    "https://ns.adobe.com/xdm/channels/direct-mail": "not_provided",
    "https://ns.adobe.com/xdm/channels/apns": "not_provided",
    "xdm:globalOptout": false
  }
}
```

プロファイル環境設定の詳細の mixin の完全な構造を把握するには、[XDM パブリック GitHub リポジトリー](https://github.com/adobe/xdm/blob/master/schemas/context/profile-preferences-details.schema.json)を参照するか、 UI を使用した mixin を確認してください。[!DNL Platform]

## セグメント化でのオプトアウトの処理

CCPA オプトアウトフラグが付いたプロファイルをセグメントに含めないようにするには、特別なフィールドを既存のセグメントに追加するか、セグメントの作成時に含める必要があります。

以下の節では、2 種類のオプトアウトフラグ用に適切なフィールドを追加する方法を示します。
1. 一般的なオプトアウト
2. 販売 / 共有のオプトアウト

### 一般的なオプトアウト

[!DNL Segmentation] は、「[!UICONTROL 一般的なオプトアウト]」フラグを含むすべてのプロファイルに対して自動的に優先します。つまり、これらのプロファイルは、デフォルトでオーディエンスやエクスポートに含まれません。 ただし、オプトアウトプロファイルがオーディエンスやマーケティング活動に含まれないようにするために、適切なフィールドを追加することをお勧めします。

This can be done using the user interface by adding **[!UICONTROL Privacy Opt-Outs]** attributes. この場合、セグメントには、オプトインしたユーザーのみが含まれるように設定されます(つまり、プロファイルに一般的なオプトアウトフラグが設定されていません)。 This is done by declaring that the &quot;[!UICONTROL Opt-Out Type]&quot; equals &quot;[!UICONTROL General Opt-Out]&quot; and the &quot;[!UICONTROL Opt-Out Value]&quot; equals &quot;[!UICONTROL Opt-in]&quot;.

![](images/opt-outs/segment-general-opt-out.png)

### 販売 / 共有のオプトアウト

ユーザーがプロファイルに販売 / 共有のオプトアウトフラグを設定している場合、このプロファイルは、セグメントの作成やマーケティング活動に使用できなくなります。To ensure this flag is honored, the &quot;[!UICONTROL Opt-Out Type]&quot; must equal &quot;[!UICONTROL Sales Sharing Opt-Out]&quot; and the &quot;[!UICONTROL Opt-Out Value]&quot; must equal &quot;[!UICONTROL Opt-in]&quot;.

![](images/opt-outs/segment-sales-sharing-opt-out.png)

<!-- ### Overriding default exclusions

In some instances, such as building a segment of people who have opted out, it may be necessary to override the default exclusion of opted-out profiles. This override can be done via the API or in the Segment Builder user interface. -->

## 次の手順

API やユーザーインターフェイスを使用したセグメント定義やオーディエンスの操作など、セグメント化の詳細については、最初に[セグメント化の概要](./home.md)に関するドキュメントを参照してください。

To learn more about data privacy within [!DNL Platform], including how [!DNL Privacy Service] helps to facilitate automated compliance with legal and organizational privacy regulations, please refer to the documentation on [[!DNL Privacy Service]](../privacy-service/home.md).
