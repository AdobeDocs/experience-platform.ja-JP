---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: Experience PlatformでのIAB TCF 2.0のサポート
topic-legacy: privacy events
description: Adobe Experience Platformの宛先にセグメントをアクティブ化する際に、顧客の同意に基づく選択肢を伝えるためのデータ操作とスキーマの設定方法について説明します。
exl-id: af787adf-b46e-43cf-84ac-dfb0bc274025
source-git-commit: da7696d288543abd21ff8a1402e81dcea32efbc2
workflow-type: tm+mt
source-wordcount: '2559'
ht-degree: 2%

---

# Experience PlatformでのIAB TCF 2.0のサポート

[!DNL Transparency & Consent Framework](TCF)は、[!DNL Interactive Advertising Bureau](IAB)で概要を説明する、オープンスタンダードな技術フレームワークで、EUの[!DNL General Data Protection Regulation](GDPR)に準拠して、消費者の個人データの処理に対する同意を取得、記録、更新できます。 このフレームワークの2番目の反復であるTCF 2.0では、ベンダーが正確な位置情報などの特定の機能を使用するかどうかや方法など、消費者が同意を提供または放棄する方法に関して、より柔軟性を付与します。

>[!NOTE]
>
>TCF 2.0に関する詳細は、[IAB EuropeのWebサイト](https://iabeurope.eu/tcf-2-0/)に記載されています。サポート資料や技術仕様も含まれます。

Adobe Experience Platformは、ID **565**&#x200B;の下の登録済み[IAB TCF 2.0ベンダーリスト](https://iabeurope.eu/vendor-list-tcf-v2-0/)に含まれています。 TCF 2.0の要件に準拠したPlatformを使用すると、顧客の同意データを収集し、保存されている顧客プロファイルに統合できます。 その後、この同意データは、使用例に応じて、プロファイルがエクスポートされたオーディエンスセグメントに含まれるかどうかに考慮できます。

>[!IMPORTANT]
>
>Platformは、TCFのバージョン2.0以降にのみ準拠できます。 以前のバージョンのTCFはサポートされていません。

このドキュメントでは、CMPで生成される顧客の同意データを受け入れるようにデータ操作とプロファイルスキーマを設定する方法、およびPlatformがセグメントをエクスポートする際にユーザーの同意選択を伝達する方法の概要を説明します。

## 前提条件

このガイドに従うには、IAB TCFに統合され、準拠している、商用または独自の同意管理プラットフォーム(CMP)を使用する必要があります。 詳しくは、準拠するCMP](https://iabeurope.eu/cmp-list/)の[リストを参照してください。

>[!IMPORTANT]
>
>CMPのIDが無効な場合、Platformはデータをそのまま処理し続けます。 TCF 2.0を強制するには、データをPlatformに送信する前に、CMPにIAB TCF 2.0に登録された有効なIDがあることを確認する必要があります。

また、このガイドでは、次のPlatformサービスに関する十分な知識が必要です。

* [エクスペリエンスデータモデル(XDM)](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステム間でIDを結び付けることで、顧客体験データの断片化によって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):を活用し [!DNL Identity Service] て、データセットから詳細な顧客プロファイルをリアルタイムで作成します。[!DNL Real-time Customer Profile] は、データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform Web SDK](../../../../edge/home.md):様々なPlatformサービスを顧客向けWebサイトに統合できる、クライアントサイドJavaScriptライブラリ。
   * [SDKの同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す、同意関連のSDKコマンドの使用例の概要です。
* [Adobe Experience Platform Segmentation Service](../../../../segmentation/home.md):類似した特性を共有 [!DNL Real-time Customer Profile] し、マーケティング戦略と同様に対応する個人のグループにデータを分割できます。

上記のPlatformサービスに加えて、[宛先](../../../../data-governance/home.md)と、Platformエコシステムでのその役割についても理解しておく必要があります。

## 顧客の同意フローの概要 {#summary}

以下の節では、システムが適切に設定された後に同意データを収集し、適用する方法について説明します。

### 同意データ収集

Platformでは、次のプロセスを通じて顧客の同意データを収集できます。

1. 顧客がWebサイト上のダイアログを通じて、データ収集に関する同意設定を提供します。
1. CMPが同意設定の変更を検出し、それに応じてTCF同意データを生成します。
1. Platform Web SDKを使用して、生成された同意データ（CMPによって返される）がAdobe Experience Platformに送信されます。
1. 収集された同意データは、スキーマにTCF同意フィールドが含まれる[!DNL Profile]対応のデータセットに取り込まれます。

CMPの同意変更フックでトリガーされるSDKコマンドに加えて、同意データは、[!DNL Profile]対応のデータセットに直接アップロードされた、顧客が生成したXDMデータを通じてExperience Platformに送ることもできます。

適切なフィールドが[!DNL Experience Cloud Identity Service]を通じてこれらのセグメントに適用されている場合、Adobe Audience Managerによって（[!DNL Audience Manager]ソースコネクタを通じて）Platformと共有されるセグメントには、同意データも含まれる可能性があります。 [!DNL Audience Manager]での同意データの収集について詳しくは、IAB TCF用](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja)[Adobe Audience Managerプラグインのドキュメントを参照してください。

### ダウンストリームの同意の実施

TCFの同意データが正常に取り込まれると、ダウンストリームPlatformサービスで次のプロセスが実行されます。

1. [!DNL Real-time Customer Profile] 顧客のプロファイルに保存されている同意データを更新します。
1. Platformは、クラスター内の各IDに対してPlatform(565)のベンダー権限が指定されている場合にのみ、顧客IDを処理します。
1. TCF 2.0ベンダーリストのメンバーに属する宛先にセグメントを書き出す場合、Platformには、Platform(565)*と*&#x200B;の両方の個々の宛先に対するベンダー権限が、クラスター内のIDごとに指定されている場合にのみ、プロファイルが含まれます。

このドキュメントの残りの節では、前述の収集要件と実施要件を満たすようにPlatformとデータ操作を設定する方法に関するガイダンスを提供します。

## CMP内で顧客の同意データを生成する方法の決定 {#consent-data}

各CMPシステムは固有なので、顧客がサービスとやり取りする際に同意を得る最適な方法を決定する必要があります。 これを実現する一般的な方法は、次の例のようなCookie同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/iab/overview/cmp-dialog.png)

このダイアログでは、顧客が次のオプトインまたはオプトアウトを許可する必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的は、ブランドが顧客のデータを使用できる広告技術の目的を定義します。 プラットフォームで顧客IDを処理するには、次の目的をオプトインする必要があります。 <ul><li>**目的1**:デバイス上での情報の保存/アクセス</li><li>**目的10**:製品の開発と改善</li></ul> |
| **ベンダー権限** | 広告技術の目的に加えて、このダイアログでは、Adobe Experience Platform(565)を含む特定のベンダーが自社のデータを使用することを顧客がオプトインまたはオプトアウトできるようにする必要があります。 |

### 同意文字列 {#consent-strings}

データの収集に使用する方法に関係なく、目標は、お客様が選択した同意オプション（同意文字列）に基づいて文字列値を生成することです。

TCF仕様では、同意文字列を使用して、ポリシーやベンダーが定義した特定のマーケティング目的の観点から、顧客の同意設定に関する関連する詳細をエンコードします。 Platformでは、これらの文字列を使用して各顧客の同意設定を保存するので、これらの設定が変更されるたびに新しい同意文字列を生成する必要があります。

同意文字列は、IAB TCFに登録されているCMPによってのみ作成できます。 特定のCMPを使用して同意文字列を生成する方法について詳しくは、IAB TCF GitHubリポジトリの[同意文字列のフォーマットガイド](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md)を参照してください。

## TCF同意フィールドを使用したデータセットの作成 {#datasets}

顧客の同意データは、スキーマにTCF同意フィールドが含まれるデータセットに送信する必要があります。 このガイドに進む前に、必要なプロファイルデータセット（およびオプションのExperience Eventデータセット）の作成方法について、TCF 2.0同意](./dataset.md)を取得するための[データセットの作成に関するチュートリアルを参照してください。

## [!DNL Profile]結合ポリシーを更新して同意データを含める {#merge-policies}

同意データを収集するための[!DNL Profile]対応のデータセットを作成したら、結合ポリシーが顧客プロファイルに常にTCF同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットの優先順位を優先するように、データセットの優先順位を設定する必要があります。

結合ポリシーの操作方法の詳細については、「[結合ポリシーの概要](../../../../profile/merge-policies/overview.md)」を参照してください。 結合ポリシーを設定する場合、データセットの準備に関するガイドで説明されているように、[XDMプライバシースキーマフィールドグループ](./dataset.md#privacy-field-group)から提供される必要な同意属性がすべてセグメントに含まれていることを確認する必要があります。

## Experience PlatformWeb SDKを統合して顧客の同意データを収集する {#sdk}

>[!NOTE]
>
>Adobe Experience PlatformでExperience Platformデータを直接処理するには、同意Web SDKを使用する必要があります。 [!DNL Experience Cloud Identity Service] は現在サポートされていません。
>
>[!DNL Experience Cloud Identity Service] は、引き続きAdobe Audience Managerでの同意処理をサポートします。TCF 2.0への準拠には、ライブラリがバージョン5.0に更新されている [必要があります](https://github.com/Adobe-Marketing-Cloud/id-service/releases)。

同意文字列を生成するようにCMPを設定したら、Experience PlatformWeb SDKを統合してこれらの文字列を収集し、Platformに送信する必要があります。 Platform SDKは、TCFの同意データをPlatformに送信するために使用できる2つのコマンドを提供します（以下のサブセクションで説明）。顧客が初めて同意情報を提供する場合と、その後同意が変更された場合に使用します。

**SDKは、標準の** CMPとのインターフェイスを提供しません。SDKをWebサイトに統合する方法を決定し、CMPで同意の変更をリッスンして、適切なコマンドを呼び出す必要があります。

### 新しいデータストリームの作成

SDKがExperience Platformにデータを送信するには、まず[!DNL Adobe Experience Platform Launch]でPlatformの新しいデータストリームを作成する必要があります。 新しい設定の作成方法に関する具体的な手順は、SDKのドキュメント[に記載されています。](../../../../edge/fundamentals/datastreams.md)

設定に一意の名前を指定した後、**[!UICONTROL Adobe Experience Platform]**&#x200B;の横にある切り替えボタンを選択します。 次に、次の値を使用して、フォームの残りの部分を完了します。

| Datastreamフィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | 必要なストリーミング接続と、データストリームを設定するためのデータセットを含む、プラットフォーム[sandbox](../../../../sandboxes/home.md)の名前。 |
| [!UICONTROL ストリーミングインレット] | Experience Platformの有効なストリーミング接続。 既存のストリーミングインレットがない場合は、[ストリーミング接続](../../../../ingestion/tutorials/create-streaming-connection-ui.md)の作成に関するチュートリアルを参照してください。 |
| [!UICONTROL イベントデータセット] | [前の手順](#datasets)で作成した[!DNL XDM ExperienceEvent]データセットを選択します。 このデータセットのスキーマに[[!UICONTROL IAB TCF 2.0 Consent]フィールドグループ](../../../../xdm/field-groups/event/iab.md)を含めた場合は、 [`sendEvent`](#sendEvent)コマンドを使用して、経時的に同意変更イベントを追跡し、そのデータをこのデータセットに保存できます。 このデータセットに保存される同意の値は、自動強制ワークフローでは&#x200B;**使用**&#x200B;されないことに注意してください。 |
| [!UICONTROL プロファイルデータセット] | [前の手順](#datasets)で作成した[!DNL XDM Individual Profile]データセットを選択します。 [`setConsent`](#setConsent)コマンドを使用してCMP同意変更フックに応答する場合、収集されたデータはこのデータセットに保存されます。 このデータセットはプロファイル対応なので、自動実施ワークフローの間、このデータセットに保存される同意値は保持されます。 |

![](../../../images/governance-privacy-security/consent/iab/overview/edge-config.png)

終了したら、画面の下部にある「**[!UICONTROL 保存]**」を選択し、追加のプロンプトに従って設定を完了します。

### 同意変更コマンドの実行

前の節で説明したデータストリームを作成したら、SDKコマンドを使用して、Platformに同意データを送信できます。 以下の節では、各SDKコマンドが様々なシナリオでどのように使用できるかの例を示します。

>[!NOTE]
>
>すべてのPlatform SDKコマンドの一般的な構文の紹介については、[コマンド](../../../../edge/fundamentals/executing-commands.md)の実行に関するドキュメントを参照してください。

#### CMP同意変更フックの使用 {#setConsent}

多くのCMPには、同意変更イベントをリッスンする標準のフックが用意されています。 これらのイベントが発生したら、`setConsent`コマンドを使用して、その顧客の同意データを更新できます。

`setConsent`コマンドは、次の2つの引数を受け取ります。(1)コマンドタイプ（この場合は「setConsent」）、(2)`consent`配列を含むペイロードを示す文字列。この配列には、次に示すように、必要な同意フィールドを提供するオブジェクトを少なくとも1つ含める必要があります。

```js
alloy("setConsent", {
  consent: [{
    standard: "IAB TCF",
    version: "2.0",
    value: "CLcVDxRMWfGmWAVAHCENAXCkAKDAADnAABRgA5mdfCKZuYJez-NQm0TBMYA4oCAAGQYIAAAAAAEAIAEgAA.argAC0gAAAAAAAAAAAA",
    gdprApplies: "true"
  }]
});
```

| ペイロードプロパティ | 説明 |
| --- | --- |
| `standard` | 使用される同意標準。 TCF 2.0の同意処理を行うには、この値を`IAB`に設定する必要があります。 |
| `version` | `standard`に示されている同意標準のバージョン番号。 TCF 2.0の同意処理を行うには、この値を`2.0`に設定する必要があります。 |
| `value` | CMPによって生成される、base-64エンコードされたコンセントストリング。 |
| `gdprApplies` | GDPRが現在ログインしている顧客に適用されるかどうかを示すBoolean値。 この顧客にTCF 2.0を適用するには、値を`true`に設定する必要があります。 定義されていない場合は、デフォルトで`true`が使用されます。 |

`setConsent`コマンドは、同意設定の変更を検出するCMPフックの一部として使用する必要があります。 次のJavaScriptは、OneTrustの`OnConsentChanged`フックに`setConsent`コマンドを使用する例を示しています。

```js
OneTrust.OnConsentChanged(function () {
  // Retrieve the TCF 2.0 consent data generated by the CMP, and pass it to Alloy. 
  __tcfapi("getTCData", 2, function (data, success) {
    if (success) {
      var tcString = data.tcString;
      var gdpr = data.gdprApplies;

      alloy("setConsent", {
        consent: [{
          standard: "IAB TCF",
          version: "2.0",
          value: tcString,
          gdprApplies: gdpr
        }]
      });
    }
  });
});
```

#### イベントの使用 {#sendEvent}

また、`sendEvent`コマンドを使用して、Platformでトリガーされるすべてのイベントに関するTCF 2.0の同意データを収集できます。

>[!NOTE]
>
>この方法を使用するには、[!DNL Profile]が有効な[!DNL XDM ExperienceEvent]スキーマに「エクスペリエンスイベントプライバシー」フィールドグループを追加しておく必要があります。 この設定方法の手順については、データセット準備ガイドの[ExperienceEventスキーマ](./dataset.md#event-schema)の更新に関する節を参照してください。

`sendEvent`コマンドは、Webサイト上の適切なイベントリスナーでコールバックとして使用する必要があります。 コマンドは、次の2つの引数を受け取ります。(1)コマンドタイプ（この場合は`sendEvent`）と(2)必要な同意フィールドをJSONとして提供する`xdm`オブジェクトを含むペイロードを示す文字列。

```js
alloy("sendEvent", {
  xdm: {
    "consentStrings": [{
      "consentStandard": "IAB TCF",
      "consentStandardVersion": "2.0",
      "consentStringValue": "CLcVDxRMWfGmWAVAHCENAXCkAKDAADnAABRgA5mdfCKZuYJez-NQm0TBMYA4oCAAGQYIAAAAAAEAIAEgAA.argAC0gAAAAAAAAAAAA",
      "gdprApplies": true
    }]
  }
});
```

| ペイロードプロパティ | 説明 |
| --- | --- |
| `xdm.consentStrings` | 必須の同意フィールドを提供するオブジェクトを少なくとも1つ含む配列。 |
| `consentStandard` | 使用される同意標準。 TCF 2.0の同意処理を行うには、この値を`IAB`に設定する必要があります。 |
| `consentStandardVersion` | `standard`に示されている同意標準のバージョン番号。 TCF 2.0の同意処理を行うには、この値を`2.0`に設定する必要があります。 |
| `consentStringValue` | CMPによって生成される、base-64エンコードされたコンセントストリング。 |
| `gdprApplies` | GDPRが現在ログインしている顧客に適用されるかどうかを示すBoolean値。 この顧客にTCF 2.0を適用するには、値を`true`に設定する必要があります。 定義されていない場合は、デフォルトで`true`が使用されます。 |

### SDK応答の処理

すべての[!DNL Platform SDK]コマンドは、呼び出しが成功したか失敗したかを示すpromiseを返します。 その後、これらの応答を、顧客への確認メッセージの表示などの追加ロジックに使用できます。 具体的な例については、SDKコマンドの実行に関するガイドの[成功または失敗](../../../../edge/fundamentals/executing-commands.md#handling-success-or-failure)の処理に関する節を参照してください。

## セグメントの書き出し {#export}

>[!NOTE]
>
>セグメントのエクスポートを開始する前に、必要な同意フィールドがすべてセグメントに含まれていることを確認する必要があります。 詳しくは、[結合ポリシーの設定](#merge-policies)の節を参照してください。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリームの宛先にエクスポートする際に、TCF 2.0への準拠を実施できます。

一連の顧客プロファイルに対して同意設定`gdprApplies`を`true`に設定した場合、ダウンストリームの宛先に書き出されたこれらのプロファイルのデータは、各プロファイルのTCF同意設定に基づいてフィルタリングされます。 必要な同意設定を満たさないプロファイルは、エクスポートプロセス中にスキップされます。

プロファイルを宛先に書き出されるセグメントに含めるには、次の目的（[TCF 2.0ポリシー](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions)で説明）に同意する必要があります。

* **目的1**:デバイス上での情報の保存/アクセス
* **目的10**:製品の開発と改善

また、TCF 2.0では、データのソースが宛先にデータを送信する前に、宛先のベンダー権限を確認する必要があります。 そのため、Platformは、宛先にバインドされたデータを含める前に、宛先のベンダー権限がクラスター内のすべてのIDに対してオプトインされているかどうかを確認します。

>[!NOTE]
>
>Adobe Audience Managerと共有されるセグメントには、対応するプラットフォームと同じTCF 2.0同意値が含まれます。 [!DNL Audience Manager]はプラットフォーム(565)と同じベンダーIDを共有するので、同じ目的とベンダー権限が必要です。 詳しくは、IAB TCF](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)用[Adobe Audience Managerプラグインのドキュメントを参照してください。

## 実装のテスト {#test-implementation}

TCF 2.0実装を設定し、宛先にセグメントを書き出した後は、同意の要件を満たさないデータは書き出されません。 ただし、書き出し中に適切な顧客プロファイルがフィルターされたかどうかを確認するには、宛先のデータストアを手動で確認して、同意が適切に適用されたかどうかを確認する必要があります。

複数のIDがクラスターを構成し、TCF 2.0が適用される場合、1つのIDに正しい目的とベンダー権限が含まれていなくても、クラスター全体が除外されることに注意する必要があります。

## 次の手順

このドキュメントでは、TCF 2.0で概要を説明しているビジネス上の義務を果たすようにPlatformデータ操作を設定するプロセスについて説明しました。Platformのプライバシー関連機能について詳しくは、[ガバナンス、プライバシー、セキュリティ](../../overview.md)の概要を参照してください。
