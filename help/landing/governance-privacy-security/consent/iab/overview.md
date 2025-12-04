---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: Experience Platformでの IAB TCF 2.0 のサポート
description: Adobe Experience Platformで宛先に対してセグメントをアクティブ化する際に、顧客の同意の選択肢を伝えるためにデータ操作とスキーマを設定する方法について説明します。
role: Developer
feature: Consent
exl-id: af787adf-b46e-43cf-84ac-dfb0bc274025
source-git-commit: f988d7665a40b589ca281d439b6fca508f23cd03
workflow-type: tm+mt
source-wordcount: '2509'
ht-degree: 1%

---

# Experience Platformでの IAB TCF 2.0 のサポート

[!DNL Transparency & Consent Framework] （IAB）で概説されている [!DNL Interactive Advertising Bureau] （TCF）は、EU 一般 [!DNL General Data Protection Regulation] （GDPR）に準拠して、個人データの処理に関する消費者の同意を組織が取得、記録、更新できるようにすることを目的とした、オープン標準の技術フレームワークです。 このフレームワークの 2 つ目のイテレーションである TCF 2.0 では、ベンダーが正確な位置情報などのデータ処理の特定の機能を使用できるかどうかや、使用する方法など、消費者が同意を提供または留保する方法について、より柔軟に対応しています。

>[!NOTE]
>
>TCF 2.0 の詳細については、サポート資料や技術仕様など、[IAB Europe web サイト &#x200B;](https://iabeurope.eu/) をご覧ください。

Adobe Experience Platformは、ID [565](https://iabeurope.eu/vendor-list-tcf/) の登録済み **IAB TCF 2.0 ベンダーリスト** の一部です。 TCF 2.0 要件に準拠して、Experience Platformを使用すると、顧客同意データを収集し、保存されている顧客プロファイルに統合できます。 この同意データは、ユースケースに応じて、書き出されたオーディエンスセグメントにプロファイルが含まれているかどうかの要因となります。

>[!IMPORTANT]
>
>Experience Platformは、TCF バージョン 2.0 （以降）にのみ準拠できます。 以前のバージョンの TCF はサポートされていません。

このドキュメントでは、お客様の同意管理プラットフォーム（CMP）によって生成された顧客同意データを受け入れるようにデータ操作とプロファイルスキーマを設定する方法の概要について説明します。 また、セグメントを書き出す際のユーザーの同意の選択肢をExperience Platformがどのように伝えるかについても説明します。

## 前提条件

このガイドに従うには、商用または独自の CMP を使用し、IAB TCF に統合および準拠する必要があります。 詳しくは、[&#x200B; 準拠している CMP のリスト &#x200B;](https://iabeurope.eu/cmp-list/) を参照してください。

>[!IMPORTANT]
>
>CMP の ID が無効な場合、Experience Platformはデータをそのままに処理し続けます。 TCF 2.0 を強制するには、データをExperience Platformに送信する前に、CMP に有効な ID が IAB TCF 2.0 に登録されていることを確認する必要があります。

このガイドでは、次のExperience Platform サービスに関する十分な知識も必要です。

* [Experience Data Model（XDM）](/help/xdm/home.md)：Adobe Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [Adobe Experience Platform ID サービス &#x200B;](/help/identity-service/home.md): デバイスやシステムをまたいで ID を結び付けることで、カスタマーエクスペリエンスのフラグメント化によって発生する基本的な課題を解決します。
* [&#x200B; リアルタイム顧客プロファイル &#x200B;](/help/profile/home.md):[!DNL Identity Service] を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 [!DNL Real-Time Customer Profile] はデータレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform web SDK](/help/collection/js/js-overview.md)：様々なExperience Platform サービスをお客様に向けた web サイトに統合できるクライアントサイド JavaScript ライブラリです。
   * [SDK同意コマンド &#x200B;](/help/collection/js/commands/setconsent.md)：このガイドに記載されている同意関連のSDK コマンドのユースケースの概要です。
* [Adobe Experience Platform セグメント化サービス &#x200B;](/help/segmentation/home.md):[!DNL Real-Time Customer Profile] データを、類似の特性を共有し、マーケティング戦略に同様に応答する個人のグループに分割できます。

上記のExperience Platform サービスに加えて、[&#x200B; 宛先 &#x200B;](/help/data-governance/home.md) とExperience Platform エコシステムでの役割にも精通している必要があります。

## 顧客同意フローの概要 {#summary}

次の節では、システムが適切に設定された後に同意データを収集および適用する方法について説明します。

### 同意データの収集

Experience Platformでは、次のプロセスを通じて顧客同意データを収集できます。

1. 顧客は、web サイト上のダイアログを通じて、データ収集に対する同意環境設定を指定します。
1. CMP が同意設定の変更を検出し、それに応じて TCF 同意データを生成します。
1. Experience Platform Web SDKを使用して、生成された同意データ（CMP によって返された）がAdobe Experience Platformに送信されます。
1. 収集された同意データは、スキーマに TCF 同意フィールドが含まれている [!DNL Profile] 対応データセットに取り込まれます。

CMP の同意変更フックによってトリガーされるSDK コマンドに加えて、同意データは、[!DNL Profile] 対応データセットに直接アップロードされる顧客生成の XDM データを介してExperience Platformにも送ることができます。

[!DNL Audience Manager] を通じて適切なフィールドがExperience Platformに適用されている場合、Adobe Audience Managerによって（[!DNL Experience Cloud Identity Service] ソースコネクタなどを通じて）共有された任意のセグメントにも同意データが含まれている可能性があります。 [!DNL Audience Manager] での同意データの収集について詳しくは、[IAB TCF 用Adobe Audience Manager プラグイン &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja) のドキュメントを参照してください。

### ダウンストリームの同意の適用

TCF 同意データが正常に取り込まれると、ダウンストリーム Experience Platform サービスで次のプロセスが実行されます。

1. [!DNL Real-Time Customer Profile] は、その顧客プロファイルに対して保存された同意データを更新します。
1. Experience Platformは、クラスター内のすべての ID に対してExperience Platform（565）のベンダー権限が提供されている場合にのみ、顧客 ID を処理します。
1. TCF 2.0 ベンダーリストのメンバーに属する宛先にセグメントを書き出す場合、Experience Platformには、Experience Platform（565） *および* の両方に対するベンダー権限がクラスター内のすべての ID に対して提供されている場合にのみ、プロファイルが含まれます。

このドキュメントの残りの節では、前述の収集および実施要件を満たすためにExperience Platformとデータ操作を設定する方法について説明します。

## CMP 内で顧客同意データを生成する方法の決定 {#consent-data}

各 CMP システムは一意なので、顧客がサービスとやり取りする際に同意を提供できる最適な方法を決定する必要があります。 Cookie 同意ダイアログは、顧客の同意を得るための一般的な方法です。 CMP ダイアログの例を以下に示します。

![&#x200B; 同意管理プラットフォームダイアログの例 &#x200B;](../../../images/governance-privacy-security/consent/iab/overview/cmp-dialog.png)

このダイアログで、顧客が次の項目をオプトイン/オプトアウトできるようにする必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的ブランドが顧客のデータを使用できる広告技術目的を定義します。 Experience Platformで顧客 ID を処理する場合は、次の目的をにオプトインする必要があります。 <ul><li>**目的 1**：デバイスに関する情報の保存やアクセス</li><li>**目的 10**：製品の開発と改善</li></ul> |
| **ベンダー権限** | このダイアログでは、広告技術の目的に加えて、お客様がAdobe Experience Platform（565）などの特定のベンダーでデータを使用することをオプトインまたはオプトアウトできるようにする必要があります。 |

### 同意文字列 {#consent-strings}

データの収集に使用する方法に関係なく、顧客が選択した同意オプションに基づいて文字列値を生成することが目標になります。これは、同意文字列と呼ばれます。

TCF の仕様では、ポリシーやベンダーで定義されている特定のマーケティング目的に照らして、顧客の同意設定に関する関連する詳細をエンコードするために同意文字列が使用されます。 Experience Platformでは、これらの文字列を使用して各顧客の同意設定を保存するので、設定が変更されるたびに新しい同意文字列を生成する必要があります。

同意文字列は、IAB TCF に登録されている CMP によってのみ作成できます。 特定の CMP を使用して同意文字列を生成する方法について詳しくは、IAB TCF GitHub リポジトリの [&#x200B; 同意文字列形式ガイド &#x200B;](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md) を参照してください。

## TCF 同意フィールドを使用したデータセットの作成 {#datasets}

顧客の同意データは、スキーマに TCF 同意フィールドが含まれるデータセットに送信する必要があります。 このガイドに進む前に、[TCF 2.0 同意を取得するためのデータセットの作成 &#x200B;](./dataset.md) に関するチュートリアルを参照して、必要なプロファイルデータセット（およびオプションのエクスペリエンスイベントデータセット）を作成する方法を確認してください。

## 同意データ [!DNL Profile] 含めるように結合ポリシーを更新する {#merge-policies}

同意データを収集するための [!DNL Profile] 対応データセットを作成したら、顧客プロファイルに常に TCF 同意フィールドを含めるように結合ポリシーが設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットが優先されるように、データセットの優先順位を設定することが含まれます。

結合ポリシーの操作方法について詳しくは、[&#x200B; 結合ポリシーの概要 &#x200B;](/help/profile/merge-policies/overview.md) を参照してください。 結合ポリシーを設定する場合は、データセットの準備に関するガイドに記載されているように、セグメントに [XDM プライバシースキーマフィールドグループ &#x200B;](./dataset.md#privacy-field-group) で提供される必要なすべての同意属性が含まれていることを確認する必要があります。

## Experience Platform Web SDKを統合して、顧客の同意データを収集します {#sdk}

>[!NOTE]
>
>Adobe Experience Platformで同意データを直接処理するには、Experience Platform Web SDKを使用する必要があります。 [!DNL Experience Cloud Identity Service] はサポートされていません。
>
>ただし、Adobe Audience Managerでの同意処理では引き続き [!DNL Experience Cloud Identity Service] がサポートされ、TCF 2.0 に準拠するには、ライブラリを [&#x200B; バージョン 5.0](https://github.com/Adobe-Marketing-Cloud/id-service/releases) に更新する必要があります。

同意文字列を生成するように CMP を設定したら、Experience Platform Web SDKを統合して、これらの文字列を収集し、Experience Platformに送信する必要があります。 Experience Platform SDKには、TCF 同意データをExperience Platformに送信するために使用できる 2 つのコマンドが用意されています（以下のサブセクションで説明します）。 これらのコマンドは、顧客が同意情報を初めて提供する際と、それ以降その同意が変更される場合に使用する必要があります。

**SDKは、初期設定では、どの CMP ともインターフェイスを行いません**。 SDKを web サイトに統合する方法を決め、CMP で同意の変更をリッスンして、適切なコマンドを呼び出すかどうかは、ユーザー次第です。

### データストリームの作成

SDKがExperience Platformにデータを送信するには、まずExperience Platformのデータストリームを作成する必要があります。 データストリームの作成方法に関する具体的な手順については、[SDKのドキュメント &#x200B;](/help/datastreams/overview.md) を参照してください。

データストリームの一意の名前を指定したら、**[!UICONTROL Adobe Experience Platform]** の横にある切り替えボタンを選択します。 次に、以下の値を使用してフォームの残りの部分を完了します。

| データストリームフィールド | 値 |
| --- | --- |
| [!UICONTROL Sandbox] | データストリームを設定するために必要なストリーミング接続とデータセットを含む、Experience Platform[&#x200B; サンドボックス &#x200B;](/help/sandboxes/home.md) の名前。 |
| [!UICONTROL Streaming Inlet] | Experience Platformの有効なストリーミング接続。 既存のストリーミングインレットがない場合は、[&#x200B; ストリーミング接続の作成 &#x200B;](/help/ingestion/tutorials/create-streaming-connection-ui.md) に関するチュートリアルを参照してください。 |
| [!UICONTROL Event Dataset] | [!DNL XDM ExperienceEvent] 前の手順 [&#x200B; で作成した &#x200B;](#datasets) データセットを選択します。 [[!UICONTROL IAB TCF 2.0 Consent] フィールドグループを含めた場合 &#x200B;](/help/xdm/field-groups/event/iab.md) このデータセットのスキーマでは、[`sendEvent`](#sendEvent) コマンドを使用して同意変更イベントを経時的に追跡し、そのデータをこのデータセットに保存できます。 このデータセットに保存される同意値は、自動適用ワークフローでは使用 **されません**。 |
| [!UICONTROL Profile Dataset] | [!DNL XDM Individual Profile] 前の手順 [&#x200B; で作成した &#x200B;](#datasets) データセットを選択します。 [`setConsent`](#setConsent) コマンドを使用して CMP 同意変更フックに応答する場合、収集されたデータはこのデータセットに保存されます。 このデータセットはプロファイル対応なので、このデータセットに保存された同意値は、自動適用ワークフロー中に適用されます。 |

![](../../../images/governance-privacy-security/consent/iab/overview/edge-config.png)

終了したら、画面の下部にある「**[!UICONTROL Save]**」を選択し、追加のプロンプトに従って続行して設定を完了します。

### 同意変更コマンドの実行

前の節で説明したデータストリームを作成したら、SDK コマンドを使用して、同意データをExperience Platformに送信することができます。 以下の節では、各SDK コマンドを様々なシナリオでどのように使用できるかについて例を示します。

#### CMP 同意変更フックの使用 {#setConsent}

多くの CMP は、同意変更イベントをリッスンする、すぐに使用できるフックを提供しています。 これらのイベントが発生した場合は、[`setConsent`](/help/collection/js/commands/setconsent.md) コマンドを使用して、その顧客の同意データを更新できます。

`setConsent` コマンドには、次の 2 つの引数を指定できます。

1. コマンドのタイプを示す文字列（この場合は「setConsent」）。
1. `consent` 配列を含むペイロード。 配列には、必要な同意フィールドを提供するオブジェクトが少なくとも 1 つ含まれている必要があります。

`setConsent` のコマンドを以下に示します。

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
| `standard` | 使用されている同意標準。 TCF 2.0 同意処理では、この値を `IAB` に設定する必要があります。 |
| `version` | `standard` に示されている同意標準のバージョン番号。 TCF 2.0 同意処理では、この値を `2.0` に設定する必要があります。 |
| `value` | CMP によって生成された base-64 でエンコードされた同意文字列。 |
| `gdprApplies` | 現在ログインしている顧客に GDPR を適用するかどうかを示すブール値。 この顧客に TCF 2.0 を適用するには、値を `true` に設定する必要があります。 定義されていない場合、デフォルトは `true` です。 |

同意設定の変更を検出する CMP フックの一部として、`setConsent` コマンドを使用してください。 次のJavaScriptは、OneTrust の `setConsent` フックに `OnConsentChanged` コマンドを使用する方法の例を示しています。

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

また、`sendEvent` コマンドを使用して、Experience Platformでトリガーされるすべてのイベントに関する TCF 2.0 同意データを収集することもできます。

>[!NOTE]
>
>この方法を使用するには、[!DNL Profile] 対応の [!DNL XDM ExperienceEvent] スキーマにエクスペリエンスイベントプライバシーフィールドグループを追加する必要があります。 これを設定する手順については、データセット準備ガイドの [ExperienceEvent スキーマの更新 &#x200B;](./dataset.md#event-schema) に関する節を参照してください。

`sendEvent` コマンドは、Web サイト上の適切なイベントリスナーでコールバックとして使用する必要があります。 コマンドは、次の 2 つの引数を想定します。（1）コマンドタイプを示す文字列（この場合は `sendEvent`）、（2）必須の同意フィールドを JSON として提供する `xdm` オブジェクトを含むペイロード。

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
| `xdm.consentStrings` | 必須の同意フィールドを提供するオブジェクトを少なくとも 1 つ含む必要がある配列。 |
| `consentStandard` | 使用されている同意標準。 TCF 2.0 同意処理では、この値を `IAB` に設定する必要があります。 |
| `consentStandardVersion` | `standard` に示されている同意標準のバージョン番号。 TCF 2.0 同意処理では、この値を `2.0` に設定する必要があります。 |
| `consentStringValue` | CMP によって生成された base-64 でエンコードされた同意文字列。 |
| `gdprApplies` | 現在ログインしている顧客に GDPR を適用するかどうかを示すブール値。 この顧客に TCF 2.0 を適用するには、値を `true` に設定する必要があります。 定義されていない場合、デフォルトは `true` です。 |

### SDKの応答の処理

多くの Web SDK コマンドは、呼び出しが成功したか失敗したかを示すプロミスを返します。 その後、これらの応答を使用して、顧客に確認メッセージを表示するなどの追加のロジックを実行できます。 詳細については、「[&#x200B; コマンドの応答 &#x200B;](/help/collection/js/commands/command-responses.md)」を参照してください。

## セグメントの書き出し {#export}

>[!NOTE]
>
>セグメントの書き出しを開始する前に、セグメントにすべての必須同意フィールドが含まれていることを確認する必要があります。 詳しくは、[&#x200B; 結合ポリシーの設定 &#x200B;](#merge-policies) の節を参照してください。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリーム宛先に書き出す際に、TCF 2.0 準拠を適用できます。

一連の顧客プロファイルに対して同意設定 `gdprApplies` が `true` に設定されている場合、ダウンストリーム宛先に書き出されるこれらのプロファイルからのデータは、各プロファイルの TCF 同意環境設定に基づいてフィルタリングされます。 必要な同意環境設定を満たさないプロファイルは、書き出しプロセス中にスキップされます。

宛先に書き出されるセグメントにプロファイルを含めるには、次の目的（[TCF 2.0 ポリシー &#x200B;](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions) で説明されている通り）に同意する必要があります。

* **目的 1**：デバイスに関する情報の保存やアクセス
* **目的 10**：製品の開発と改善

また、TCF 2.0 では、データを宛先に送信する前に、データのソースが宛先のベンダー権限を確認する必要があります。 そのため、Experience Platformは、その宛先にバインドされているデータを含める前に、クラスター内のすべての ID に対して宛先のベンダー権限がにオプトインされているかどうかを確認します。

>[!NOTE]
>
>Experience Platformと共有されるセグメントには、Adobe Audience Managerの対応するセグメントと同じ TCF 2.0 同意値が含まれます。 [!DNL Audience Manager] はExperience Platform（565）と同じベンダー ID を共有するので、同じ目的とベンダーの許可が必要です。 詳しくは、[IAB TCF 用Adobe Audience Manager プラグイン &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja) のドキュメントを参照してください。

## 実装のテスト {#test-implementation}

TCF 2.0 の実装を設定し、宛先にセグメントを書き出した後は、同意要件を満たさないデータは書き出されません。 書き出し中に正しい顧客プロファイルがフィルタリングされたかどうかを確認するには、宛先のデータストアを手動で確認して、同意が正しく適用されているかどうかを確認する必要があります。

>[!IMPORTANT]
>
>複数の ID がクラスターを構成し、TCF 2.0 が適用される場合、1 つの ID にも正しい目的とベンダー権限が含まれていなくても、クラスター全体が除外されます。

## 次の手順

このドキュメントでは、TCF 2.0 で説明されているように、ビジネス義務を満たすようにExperience Platform データの運用を設定するプロセスについて説明しました。Experience Platformのプライバシー関連機能について詳しくは、[&#x200B; ガバナンス、プライバシー、セキュリティ &#x200B;](../../overview.md) に関する概要を参照してください。
