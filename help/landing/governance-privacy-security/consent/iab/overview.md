---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: Experience Platformでの IAB TCF 2.0 のサポート
topic-legacy: privacy events
description: Adobe Experience Platformの宛先にセグメントをアクティブ化する際に、顧客の同意に基づく選択肢を伝えるためのデータ操作とスキーマの設定方法について説明します。
exl-id: af787adf-b46e-43cf-84ac-dfb0bc274025
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '2563'
ht-degree: 2%

---

# Experience Platformでの IAB TCF 2.0 のサポート

[!DNL Transparency & Consent Framework](TCF) は、[!DNL Interactive Advertising Bureau](IAB) で概説される、オープンスタンダードな技術フレームワークで、EU の [!DNL General Data Protection Regulation](GDPR) に従って、企業が個人データの処理に関する消費者の同意を取得、記録、更新できるようにします。 フレームワークの 2 番目の反復である TCF 2.0 では、ベンダーが正確な位置情報などの特定の機能を使用するかどうかや方法など、消費者が同意を提供または拒否する方法に関して、より柔軟性を付与します。

>[!NOTE]
>
>TCF 2.0 の詳細は、[IAB Europe の Web サイト ](https://iabeurope.eu/tcf-2-0/) に記載されています。サポート資料や技術仕様も含まれます。

Adobe Experience Platformは、ID **565** の下の登録済み [IAB TCF 2.0 ベンダーリスト ](https://iabeurope.eu/vendor-list-tcf-v2-0/) に含まれています。 TCF 2.0 の要件に準拠した Platform を使用すると、顧客の同意データを収集し、保存された顧客プロファイルに統合できます。 その後、この同意データは、使用例に応じて、プロファイルが書き出されたオーディエンスセグメントに含まれるかどうかに考慮できます。

>[!IMPORTANT]
>
>Platform は、TCF のバージョン 2.0 以降にのみ準拠できます。 以前のバージョンの TCF はサポートされていません。

このドキュメントでは、CMP で生成される顧客の同意データを受け入れるようにデータ操作とプロファイルスキーマを設定する方法、およびセグメントをエクスポートする際に Platform がユーザーの同意選択を伝達する方法の概要を説明します。

## 前提条件

このガイドに従うには、IAB TCF に統合され、準拠している、商用または独自の同意管理プラットフォーム (CMP) を使用する必要があります。 詳しくは、[ 準拠する CMP](https://iabeurope.eu/cmp-list/) のリストを参照してください。

>[!IMPORTANT]
>
>CMP の ID が無効な場合、Platform はデータをそのまま処理し続けます。 TCF 2.0 を強制するには、データを Platform に送信する前に、IAB TCF 2.0 に登録されている有効な ID が CMP に含まれていることを確認する必要があります。

また、このガイドでは、次の Platform サービスに関する十分な知識が必要です。

* [エクスペリエンスデータモデル (XDM)](../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステム間で ID を結び付けることで、顧客体験データの断片化によって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):を活用し [!DNL Identity Service] て、データセットから詳細な顧客プロファイルをリアルタイムで作成します。[!DNL Real-time Customer Profile] は、データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform Web SDK](../../../../edge/home.md):様々な Platform サービスを顧客向け Web サイトに統合できる、クライアントサイド JavaScript ライブラリ。
   * [SDK の同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す、同意関連の SDK コマンドの使用例の概要です。
* [Adobe Experience Platform Segmentation Service](../../../../segmentation/home.md):データを、類似した特性を [!DNL Real-time Customer Profile] 共有し、マーケティング戦略と同様に対応する個人のグループに分割できます。

上記の Platform サービスに加えて、[ 宛先 ](../../../../data-governance/home.md) と、Platform エコシステムでのその役割についても理解しておく必要があります。

## 顧客の同意フローの概要 {#summary}

次の節では、システムが適切に設定された後に同意データを収集して適用する方法について説明します。

### 同意データ収集

Platform では、次のプロセスを通じて顧客の同意データを収集できます。

1. 顧客が Web サイト上のダイアログを通じて、データ収集に関する同意設定を提供します。
1. CMP が同意設定の変更を検出し、それに応じて TCF 同意データを生成します。
1. Platform Web SDK を使用して、生成された同意データ（CMP によって返される）がAdobe Experience Platformに送信されます。
1. 収集された同意データは、スキーマに TCF の同意フィールドが含まれている [!DNL Profile] 対応のデータセットに取り込まれます。

CMP の同意変更フックによってトリガーされる SDK コマンドに加えて、同意データは、[!DNL Profile] 対応のデータセットに直接アップロードされた、顧客が生成した XDM データを通じてExperience Platformに送ることもできます。

適切なフィールドが [!DNL Experience Cloud Identity Service] を通じてこれらのセグメントに適用されている場合、Adobe Audience Managerによって（[!DNL Audience Manager] ソースコネクタを介して）Platform と共有されるセグメントには、同意データも含まれる場合があります。 [!DNL Audience Manager] での同意データの収集について詳しくは、[IAB TCF 用Adobe Audience Managerプラグイン ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja) のドキュメントを参照してください。

### 下流の同意の実施

TCF の同意データが正常に取り込まれると、ダウンストリーム Platform サービスで次のプロセスが実行されます。

1. [!DNL Real-time Customer Profile] 保存されている同意データを、その顧客のプロファイルに対して更新します。
1. Platform は、クラスター内の各 ID に対して Platform(565) のベンダー権限が指定されている場合にのみ、顧客 ID を処理します。
1. TCF 2.0 ベンダーリストのメンバーに属する宛先にセグメントを書き出す場合、Platform には、Platform(565) *と* の両方の個々の宛先に対するベンダー権限が、クラスター内の各 ID に対して提供されている場合にのみ、プロファイルが含まれます。

このドキュメントの残りの節では、前述の収集要件と実施要件を満たすように Platform とデータ操作を設定する方法に関するガイダンスを提供します。

## CMP 内で顧客の同意データを生成する方法の決定 {#consent-data}

各 CMP システムは固有なので、お客様がサービスとやり取りする際に同意を提供できる最適な方法を決定する必要があります。 これを実現する一般的な方法は、次の例のような Cookie の同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/iab/overview/cmp-dialog.png)

このダイアログでは、顧客が次の項目をオプトインまたはオプトアウトできる必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的は、ブランドが顧客のデータを使用できる広告技術の目的を定義します。 Platform で顧客 ID を処理するには、次の目的をオプトインする必要があります。 <ul><li>**目的 1**:デバイス上の情報の保存/アクセス</li><li>**目的 10**:製品の開発と改善</li></ul> |
| **ベンダー権限** | 広告技術の目的に加えて、このダイアログでは、Adobe Experience Platform(565) を含む特定のベンダーがデータを使用することを顧客がオプトインまたはオプトアウトできるようにする必要があります。 |

### 同意文字列 {#consent-strings}

データの収集方法に関係なく、目標は、お客様が選択した同意オプション（同意文字列）に基づいて文字列値を生成することです。

TCF 仕様では、同意文字列を使用して、ポリシーやベンダーが定義した特定のマーケティング目的の観点から、顧客の同意設定に関する関連する詳細をエンコードします。 Platform では、これらの文字列を使用して各顧客の同意設定を保存するので、これらの設定が変更されるたびに新しい同意文字列を生成する必要があります。

同意文字列は、IAB TCF に登録されている CMP によってのみ作成できます。 特定の CMP を使用して同意文字列を生成する方法について詳しくは、IAB TCF GitHub リポジトリの [ 同意文字列のフォーマットガイド ](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md) を参照してください。

## TCF 同意フィールドを使用したデータセットの作成 {#datasets}

顧客の同意データは、スキーマに TCF 同意フィールドが含まれるデータセットに送信する必要があります。 このガイドに進む前に、必要なプロファイルデータセット（およびオプションの Experience Event データセット）の作成方法について、TCF 2.0 同意 ](./dataset.md) を取得するための [ データセットの作成に関するチュートリアルを参照してください。

## [!DNL Profile] 結合ポリシーを更新して、同意データを含めます {#merge-policies}

同意データを収集するための [!DNL Profile] 対応のデータセットを作成したら、結合ポリシーが顧客プロファイルに常に TCF 同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットの方が優先されるように、データセットの優先順位を設定する必要があります。

結合ポリシーの操作方法の詳細については、「[ 結合ポリシーの概要 ](../../../../profile/merge-policies/overview.md)」を参照してください。 結合ポリシーを設定する場合、データセットの準備に関するガイドで説明されているように、[XDM プライバシースキーマフィールドグループ ](./dataset.md#privacy-field-group) が提供する必要な同意属性をすべてセグメントに含める必要があります。

## Experience PlatformWeb SDK を統合して顧客の同意データを収集する {#sdk}

>[!NOTE]
>
>Adobe Experience Platformで直接同意データを処理するには、Experience PlatformWeb SDK を使用する必要があります。 [!DNL Experience Cloud Identity Service] は現在サポートされていません。
>
>[!DNL Experience Cloud Identity Service] は、引き続きAdobe Audience Managerでの同意処理をサポートします。また、TCF 2.0 への準拠には、ライブラリがバージョン 5.0 に更新され [る必要があります](https://github.com/Adobe-Marketing-Cloud/id-service/releases)。

同意文字列を生成するように CMP を設定したら、Experience PlatformWeb SDK を統合してこれらの文字列を収集し、Platform に送信する必要があります。 Platform SDK は、TCF の同意データを Platform に送信するために使用できる 2 つのコマンドを提供します（以下のサブ節で説明）。顧客が初めて同意情報を提供する場合と、その後同意が変更された場合に使用します。

**SDK は、標準の** CMP とのインターフェイスを取りません。SDK を Web サイトに統合する方法を決定し、CMP で同意の変更をリッスンして、適切なコマンドを呼び出す必要があります。

### 新しいデータストリームの作成

SDK がデータをExperience Platformに送信するには、まずデータ収集 UI で Platform 用の新しいデータストリームを作成する必要があります。 新しいデータストリームの作成方法に関する具体的な手順は、[SDK ドキュメント ](../../../../edge/fundamentals/datastreams.md) に記載されています。

データストリームに一意の名前を指定した後、**[!UICONTROL Adobe Experience Platform]** の横にある切り替えボタンを選択します。 次に、次の値を使用して、残りのフォームに入力します。

| Datastream フィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | 必要なストリーミング接続と、データストリームを設定するためのデータセットを含む、プラットフォーム [ サンドボックス ](../../../../sandboxes/home.md) の名前。 |
| [!UICONTROL ストリーミングインレット] | 有効なストリーミングExperience Platform。 既存のストリーミングインレットがない場合は、[ ストリーミング接続の作成 ](../../../../ingestion/tutorials/create-streaming-connection-ui.md) に関するチュートリアルを参照してください。 |
| [!UICONTROL イベントデータセット] | [ 前の手順 ](#datasets) で作成した [!DNL XDM ExperienceEvent] データセットを選択します。 このデータセットのスキーマに [[!UICONTROL IAB TCF 2.0 Consent] フィールドグループ ](../../../../xdm/field-groups/event/iab.md) を含めた場合は、 [`sendEvent`](#sendEvent) コマンドを使用して、経時的に同意変更イベントを追跡し、そのデータをこのデータセットに保存できます。 このデータセットに保存される同意値は、自動強制ワークフローでは **使用** されないことに注意してください。 |
| [!UICONTROL プロファイルデータセット] | [ 前の手順 ](#datasets) で作成した [!DNL XDM Individual Profile] データセットを選択します。 [`setConsent`](#setConsent) コマンドを使用して CMP 同意変更フックに応答する場合、収集されたデータはこのデータセットに保存されます。 このデータセットはプロファイル対応なので、自動実施ワークフローの間、このデータセットに保存される同意値は保持されます。 |

![](../../../images/governance-privacy-security/consent/iab/overview/edge-config.png)

終了したら、画面の下部で「**[!UICONTROL 保存]**」を選択し、追加のプロンプトに従って設定を完了します。

### 同意変更コマンドの実行

前の節で説明したデータストリームを作成したら、SDK コマンドを使用して、Platform に同意データを送信できます。 以下の節では、様々なシナリオで各 SDK コマンドを使用する方法の例を示します。

>[!NOTE]
>
>すべての Platform SDK コマンドに共通する構文の概要については、[ コマンドの実行 ](../../../../edge/fundamentals/executing-commands.md) に関するドキュメントを参照してください。

#### CMP の同意変更フックの使用 {#setConsent}

多くの CMP には、同意変更イベントをリッスンする標準のフックが用意されています。 これらのイベントが発生した場合は、`setConsent` コマンドを使用して、その顧客の同意データを更新できます。

`setConsent` コマンドは、次の 2 つの引数を受け取ります。(1) コマンドの種類（この場合は「setConsent」）と (2)`consent` 配列を含むペイロードを示す文字列。この配列には、次に示すように、必要な同意フィールドを提供するオブジェクトが少なくとも 1 つ含まれている必要があります。

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
| `standard` | 使用される同意基準。 TCF 2.0 の同意処理を行う場合は、この値を `IAB` に設定する必要があります。 |
| `version` | `standard` で示された同意基準のバージョン番号。 TCF 2.0 の同意処理を行う場合は、この値を `2.0` に設定する必要があります。 |
| `value` | CMP によって生成された、base-64 でエンコードされたコンセントストリング。 |
| `gdprApplies` | GDPR が現在ログインしている顧客に適用されるかどうかを示す Boolean 値。 この顧客に TCF 2.0 を適用するには、値を `true` に設定する必要があります。 定義されていない場合は、デフォルトで `true` が使用されます。 |

`setConsent` コマンドは、同意設定の変更を検出する CMP フックの一部として使用する必要があります。 次の JavaScript は、OneTrust の `OnConsentChanged` フックに `setConsent` コマンドを使用する例を示しています。

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

また、`sendEvent` コマンドを使用して、Platform でトリガーされるすべてのイベントで TCF 2.0 の同意データを収集できます。

>[!NOTE]
>
>この方法を使用するには、[!DNL Profile] 対応の [!DNL XDM ExperienceEvent] スキーマに「エクスペリエンスイベントプライバシー」フィールドグループを追加しておく必要があります。 この設定方法の手順については、データセット準備ガイドの [ExperienceEvent スキーマの更新 ](./dataset.md#event-schema) に関する節を参照してください。

`sendEvent` コマンドは、Web サイト上の適切なイベントリスナーでコールバックとして使用する必要があります。 このコマンドは、次の 2 つの引数を受け取ります。(1) コマンドの種類（この場合は `sendEvent`）と (2) 必要な同意フィールドを JSON として提供する `xdm` オブジェクトを含むペイロードを示す文字列。

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
| `xdm.consentStrings` | 必須の同意フィールドを提供するオブジェクトを少なくとも 1 つ含む配列。 |
| `consentStandard` | 使用される同意基準。 TCF 2.0 の同意処理を行う場合は、この値を `IAB` に設定する必要があります。 |
| `consentStandardVersion` | `standard` で示された同意基準のバージョン番号。 TCF 2.0 の同意処理を行う場合は、この値を `2.0` に設定する必要があります。 |
| `consentStringValue` | CMP によって生成された、base-64 でエンコードされたコンセントストリング。 |
| `gdprApplies` | GDPR が現在ログインしている顧客に適用されるかどうかを示す Boolean 値。 この顧客に TCF 2.0 を適用するには、値を `true` に設定する必要があります。 定義されていない場合は、デフォルトで `true` が使用されます。 |

### SDK 応答の処理

すべての [!DNL Platform SDK] コマンドは、呼び出しが成功したか失敗したかを示す promise を返します。 その後、これらの応答を、顧客への確認メッセージの表示などの追加ロジックに使用できます。 具体的な例については、SDK コマンドの実行に関するガイドの [ 成功または失敗 ](../../../../edge/fundamentals/executing-commands.md#handling-success-or-failure) の処理に関する節を参照してください。

## セグメントのエクスポート {#export}

>[!NOTE]
>
>セグメントのエクスポートを開始する前に、必要な同意フィールドがすべてセグメントに含まれていることを確認する必要があります。 詳しくは、[ 結合ポリシーの設定 ](#merge-policies) の節を参照してください。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリームの宛先に書き出す際に、TCF 2.0 への準拠を強制できます。

一連の顧客プロファイルに対して同意設定 `gdprApplies` を `true` に設定した場合、ダウンストリームの宛先に書き出されるこれらのプロファイルのデータは、各プロファイルの TCF 同意設定に基づいてフィルタリングされます。 必要な同意設定を満たさないプロファイルは、エクスポートプロセス中にスキップされます。

プロファイルを宛先に書き出されるセグメントに含めるには、次の目的（[TCF 2.0 ポリシー ](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions) で説明）に同意する必要があります。

* **目的 1**:デバイス上の情報の保存/アクセス
* **目的 10**:製品の開発と改善

また、TCF 2.0 では、データのソースが宛先にデータを送信する前に、宛先のベンダー権限を確認する必要があります。 そのため、Platform は、宛先にバインドされたデータを含める前に、宛先のベンダー権限が、クラスター内のすべての ID に対してオプトインされているかどうかを確認します。

>[!NOTE]
>
>Adobe Audience Managerと共有されるセグメントには、対応する Platform と同じ TCF 2.0 同意値が含まれます。 [!DNL Audience Manager] はプラットフォーム (565) と同じベンダー ID を共有するので、同じ目的とベンダー権限が必要です。 詳しくは、IAB TCF](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html) 用Adobe Audience Managerプラグインのドキュメントを参照してください。[

## 実装のテスト {#test-implementation}

TCF 2.0 実装を設定し、宛先にセグメントを書き出した後は、同意要件を満たさないデータは書き出されません。 ただし、書き出し中に適切な顧客プロファイルがフィルターされたかどうかを確認するには、宛先のデータストアを手動で確認して、同意が適切に適用されたかどうかを確認する必要があります。

複数の ID がクラスターを構成し、TCF 2.0 が適用される場合、1 つの ID に正しい目的とベンダー権限が含まれていない場合は、クラスター全体が除外されることに注意してください。

## 次の手順

このドキュメントでは、TCF 2.0 で概要を説明しているビジネス上の義務を満たすように Platform データ操作を設定するプロセスを説明しました。Platform のプライバシー関連機能の詳細については、[ ガバナンス、プライバシー、セキュリティ ](../../overview.md) の概要を参照してください。
