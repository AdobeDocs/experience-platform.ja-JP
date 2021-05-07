---
keywords: Experience Platform；ホーム；人気のあるトピック；オプトアウト；Segmentation;Segmentationサービス；Segmentationサービス；名誉のオプトアウト；オプトアウト；オプトアウト；オプトアウト；
solution: Experience Platform
title: セグメントでのオプトアウトリクエストの実行
topic-legacy: overview
description: Adobe Experience Platformでは、顧客がデータの使用状況やストレージに関するオプトアウトリクエストをリアルタイム顧客プロファイル内に送信することを許可しています。] これらのオプトアウトリクエストは、カリフォルニア州消費者プライバシー法（CCPA）に含まれています。CCPA は、カリフォルニア州の在住者に対して、個人データにアクセスし削除する権利や、個人データが販売または開示されたか（そして誰に対して）を知る権利を付与しています。
exl-id: fe851ce3-60db-4984-a73c-f9c5964bfbad
translation-type: tm+mt
source-git-commit: ab0798851e5f2b174d9f4241ad64ac8afa20a938
workflow-type: tm+mt
source-wordcount: '1049'
ht-degree: 48%

---

# セグメントでのオプトアウトリクエストの遵守

Adobe Experience Platformは、顧客が[!DNL Real-time Customer Profile]内でのデータの使用状況とストレージに関するオプトアウトリクエストを送信することを許可します。 これらのオプトアウト要求は[!DNL California Consumer Privacy Act] (CCPA)の一部であり、カリフォルニア州の住民に対して、個人データのアクセス権や削除権を与え、個人データの販売または公開（および公開）の有無を知ることができます。

顧客がオプトアウトしたら、組織はマーケティング活動のオーディエンスを生成する際に、これらのオプトアウトを遵守する必要があります。このドキュメントでは、オプトアウトリクエストの遵守に関する重要な詳細について説明します。

## はじめに

オプトアウト要求を実行するには、関連する様々な[!DNL Adobe Experience Platform]サービスに関する理解が必要です。 オプトアウトリクエストを処理する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](./home.md):デー [!DNL Real-time Customer Profile] タからオーディエンスセグメントを作成できます。
- [[!DNL Experience Data Model (XDM)]](../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [[!DNL Adobe Experience Platform Privacy Service]](../privacy-service/home.md):組織が、内部の顧客データに関するデータのプライバシー規制へのコンプライアンスを自動化できるよう支援 [!DNL Platform]します。

## オプトアウトスキーマフィールドグループ

CCPAオプトアウト要求を受け入れるために、和集合スキーマの一部であるスキーマの1つに、必要な[!DNL Experience Data Model](XDM)オプトアウトフィールドが含まれている必要があります。 スキーマにオプトアウトフィールドを追加するために使用できるスキーマフィールドグループは2つあります。それぞれ以下の節で詳しく説明します。

- [プロファイルプライバシー](#profile-privacy)：様々なオプトアウトタイプ（一般または販売 / 共有）を取得するために使用します。
- [プロファイル環境設定の詳細](#profile-preferences-details)：特定の XDM チャネルのオプトアウトリクエストを取得するために使用します。

フィールドグループをスキーマに追加する手順については、次のXDMドキュメントの「追加a field group」の節を参照してください。
- [スキーマレジストリ API のチュートリアル](../xdm/api/getting-started.md)：スキーマレジストリ API を使用したスキーマの構築。
- [スキーマエディタのチュートリアル](../xdm/tutorials/create-schema-ui.md)：Platform のユーザーインターフェイスを使用したスキーマの構築。

次の例は、ユーザーインターフェイスのスキーマに追加されたオプトアウトフィールドグループを示しています。

![](images/opt-outs/opt-out-field-groups-user-interface.png)

各フィールドグループの構造、および各フィールドグループがスキーマに貢献するフィールドの説明を、以下の節で詳しく説明します。

### [!DNL Profile Privacy] {#profile-privacy}

[!DNL Profile Privacy]フィールドグループを使用すると、顧客から2種類のCCPAオプトアウト要求を取得できます。

1. 一般的なオプトアウト
2. 販売 / 共有のオプトアウト

![](images/opt-outs/profile-privacy.png)

[!DNL Profile Privacy]フィールドグループには次のフィールドが含まれています。

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

[!DNL Profile Privacy]フィールドグループの完全な構造を表示するには、[XDM public GitHub repository](https://github.com/adobe/xdm/blob/master/schemas/context/profile-privacy.schema.json)を参照するか、Platform UIを使用してフィールドグループをプレビューしてください。

### [!DNL Profile Preferences Details] {#profile-preferences-details}

[!DNL Profile Preferences Details]フィールドグループには、顧客プロファイルに対する環境設定を表すフィールドがいくつか用意されています(E メールフォーマット、優先言語、タイムゾーンなど)。 このフィールドグループに含まれるフィールドの1つであるOptInOut (`optInOut`)を使用すると、個々のチャネルに対してオプトアウト値を設定できます。

![](images/opt-outs/profile-preferences-details.png)

[!DNL Profile Preferences Details]フィールドグループには、オプトアウトに関連する次のフィールドが含まれます。

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

プロファイルの環境設定の詳細フィールドグループの構造を完全に表示するには、[XDM public GitHub repository](https://github.com/adobe/xdm/blob/master/schemas/context/profile-preferences-details.schema.json)にアクセスするか、[!DNL Platform] UIを使用してフィールドグループをプレビューしてください。

## セグメント化でのオプトアウトの処理

CCPA オプトアウトフラグが付いたプロファイルをセグメントに含めないようにするには、特別なフィールドを既存のセグメントに追加するか、セグメントの作成時に含める必要があります。

以下の節では、2 種類のオプトアウトフラグ用に適切なフィールドを追加する方法を示します。
1. 一般的なオプトアウト
2. 販売 / 共有のオプトアウト

### 一般的なオプトアウト

[!DNL Segmentation] は、「[!UICONTROL 一般的なオプトアウト]」フラグを含むすべてのプロファイルに対して自動的に優先します。つまり、これらのプロファイルは、デフォルトでオーディエンスやエクスポートに含まれません。ただし、オプトアウトプロファイルがオーディエンスやマーケティング活動に含まれないようにするために、適切なフィールドを追加することをお勧めします。

これは、ユーザーインターフェイスを使用して、**[!UICONTROL プライバシーオプトアウト]**&#x200B;属性を追加することで行うことができます。 この場合、セグメントには、オプトインしたユーザーのみが含まれるように設定されます(つまり、プロファイルに一般的なオプトアウトフラグが設定されていません)。 これは、「[!UICONTROL オプトアウトタイプ]」が「[!UICONTROL 一般的なオプトアウト]」、「[!UICONTROL オプトアウト値]」が「[!UICONTROL オプトイン]」と等しいと宣言することで行います。

![](images/opt-outs/segment-general-opt-out.png)

### 販売 / 共有のオプトアウト

ユーザーがプロファイルに販売 / 共有のオプトアウトフラグを設定している場合、このプロファイルは、セグメントの作成やマーケティング活動に使用できなくなります。このフラグが確実に適用されるように、「[!UICONTROL オプトアウトタイプ]」が「[!UICONTROL 販売共有オプトアウト]」、「[!UICONTROL オプトアウト値]」が「[!UICONTROL オプトイン]」に等しい必要があります。

![](images/opt-outs/segment-sales-sharing-opt-out.png)

<!-- ### Overriding default exclusions

In some instances, such as building a segment of people who have opted out, it may be necessary to override the default exclusion of opted-out profiles. This override can be done via the API or in the Segment Builder user interface. -->

## 次の手順

API やユーザーインターフェイスを使用したセグメント定義やオーディエンスの操作など、セグメント化の詳細については、最初に[セグメント化の概要](./home.md)に関するドキュメントを参照してください。

[!DNL Platform]内のデータのプライバシーに関する詳細は、[!DNL Privacy Service]が法的および組織のプライバシー規制への自動コンプライアンスを支援する方法など、[[!DNL Privacy Service]](../privacy-service/home.md)内のデータのプライバシーに関する情報を参照してください。
