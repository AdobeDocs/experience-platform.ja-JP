---
keywords: Experience Platform;home;IAB;IAB 2.0;consent;Consent
solution: Experience Platform
title: リアルタイム顧客データプラットフォームでのIAB TCF 2.0のサポート
topic: privacy events
translation-type: tm+mt
source-git-commit: 75a0b4ba1342ece3e34a7ef41782b9014516d4fe
workflow-type: tm+mt
source-wordcount: '2388'
ht-degree: 3%

---


# でのIAB TCF 2.0のサポート [!DNL Real-time Customer Data Platform]

(TCF) [!DNL Transparency & Consent Framework] は、(IAB)で概要を説明している [!DNL Interactive Advertising Bureau] 。TCFは、欧州和集合(GDPR)に準拠して、個人データの処理に関する消費者の同意を取得、記録、更新することを可能にする、オープン・スタンダードの技術枠組みである [!DNL General Data Protection Regulation] 。 フレームワークの2番目のイテレーションであるTCF 2.0では、ベンダーが正確な位置情報など、データ処理の特定の機能を使用するかどうか、使用方法など、消費者が同意を提供または保留する方法を柔軟に許可します。

>[!NOTE]
>
>TCF 2.0の詳細については、 [IAB EuropeのWebサイト](https://iabeurope.eu/tcf-2-0/)（サポート資料や技術仕様など）を参照してください。

[!DNL Real-time Customer Data Platform (Real-time CDP)] は、ID 565の下で、登録された [IAB TCF 2.0ベンダーリスト](https://iabeurope.eu/vendor-list-tcf-v2-0/)の一部で **す**。 TCF 2.0の要件に準拠して、顧客の同意データを収集し [!DNL Real-time CDP] 、保存されている顧客プロファイルに統合できます。 その後、この同意データは、使用事例に応じて、プロファイルが書き出されたオーディエンスセグメントに含まれるかどうかに組み込まれます。

>[!IMPORTANT]
>
>[!DNL Real-time CDP] は、TCFのバージョン2.0（またはそれ以降）にのみ準拠できます。 以前のバージョンのTCFはサポートされていません。

このドキュメントでは、データ操作とプロファイルスキーマがCMPで生成された顧客の同意データを受け入れるように設定する方法の概要、およびセグメントを書き出す際のユーザーの同意の選択肢を [!DNL Real-time CDP] 伝える方法について説明します。

## 前提条件

このガイドに従うには、IAB TCFと統合され、準拠している、商用または独自の同意管理プラットフォーム(CMP)を使用する必要があります。 詳しくは、準拠するCMPの [リストを参照してください](https://iabeurope.eu/cmp-list/) 。

>[!IMPORTANT]
>
>CMPのIDが無効な場合、はデータをそのまま処理 [!DNL Real-time CDP] し続けます。 TCF 2.0を強制するには、データを送信する前に、CMPにIAB TCF 2.0に登録された有効なIDがあることを確認する必要があり [!DNL Experience Platform]ます。

また、本ガイドでは、以下のAdobe Experience Platformサービスについて、実際に理解する必要があります。

* [エクスペリエンスデータモデル（XDM）](../../../xdm/home.md)[!DNL Experience Platform]： が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
* [Adobe Experience Platform・アイデンティティ・サービス](../../../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../profile/home.md):を活用 [!DNL Identity Service] して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 [!DNL Real-time Customer Profile] data Lakeからデータを取り込み、顧客プロファイルを個別のデータストアに維持します。
* [Adobe Experience PlatformウェブSDK](../../../edge/home.md):様々な [!DNL Platform] サービスを顧客と直接やり取りするWebサイトに統合できるクライアント側のJavaScriptライブラリ。
   * [SDKの同意コマンド](../../../edge/consent/supporting-consent.md):このガイドに示す同意関連のSDKコマンドの使用例の概要です。
* [Adobe Experience Platform分類サービス](../../../segmentation/home.md):同様の特性を共有し、マーケティング戦略と同様に対応する個人のグループに [!DNL Real-time Customer Profile] データを分割できます。

上記の [!DNL Platform] サービスに加えて、 [目的地](../../destinations/overview.md) 、およびその使用に関する知識も必要 [!DNL Real-time CDP]です。

## 顧客の同意フローの概要 {#summary}

次の節では、システムが正しく設定された後に同意データを収集および適用する方法について説明します。

### 同意データ収集

[!DNL Real-time CDP] 次のプロセスを通じて顧客の同意データを収集できます。

1. 顧客は、Webサイト上のダイアログを通じて、データ収集に関する同意の環境設定を行います。
1. CMPは同意の優先度の変更を検出し、それに応じてIAB同意データを生成します。
1. を使用し [!DNL Experience Platform Web SDK]て、生成された同意データ（CMPが返す）がAdobe Experience Platformに送信されます。
1. 収集された同意データは、スキーマにIAB同意フィールドが含まれる [!DNL Profile]有効なデータセットに取り込まれます。

CMPの同意変更フックによってトリガーされるSDKコマンドに加えて、同意データは、有効なデータセットに直接アップロードされる、お客様が生成したXDMデータ [!DNL Experience Platform] を通じてにも送信され [!DNL Profile]ます。

( [!DNL Platform] ソースコネクタを介して)Adobe Audience Managerと共有さ [!DNL Audience Manager] れるセグメントには、それらのセグメントに適切なフィールドが適用されている場合、同意データが含まれる可能性もあり [!DNL Experience Cloud Identity Service]ます。 での同意データの収集について詳し [!DNL Audience Manager]くは、IAB TCF用 [Adobe Audience Managerプラグインのドキュメントを参照してください](https://docs.adobe.com/help/ja-JP/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。

### 下流の同意の実施

IABの同意データが正常に取り込まれると、ダウンストリーム [!DNL Real-time CDP] サービスでは次の処理が行われます。

1. [!DNL Real-time Customer Profile] その顧客のプロファイルに対して保存されている同意データを更新します。
1. [!DNL Real-time CDP] は、(565)のベンダー権限がクラスター内のすべてのIDに対して指定され [!DNL Real-time CDP] ている場合にのみ、顧客IDを処理します。
1. TCF 2.0ベンダーリストのメンバーに属する宛先にセグメントをエクスポートする場合、 [!DNL Real-time CDP] プロファイルは、ベンダー権限が [!DNL Real-time CDP] (565)の両方に対して与えられ、宛先がクラスター内のIDごとに与えら ** れる場合にのみ含まれます。

このドキュメントの残りのセクションでは、上記の収集および実施要件を満たすためのデータ操作を構成 [!DNL Real-time CDP] および設定する方法に関するガイダンスを提供します。

## CMP内で顧客の同意データを生成する方法を決定する {#consent-data}

各CMPシステムは独自のものなので、顧客がサービスとやり取りする際に、顧客が同意を与える最善の方法を決定する必要があります。 これを行う一般的な方法は、次の例のように、cookieの同意ダイアログを使用することです。

![](../assets/iab/cmp-dialog.png)

このダイアログでは、顧客が次の操作を行えるオプトインか、または行えないかを指定する必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的は、ブランドが顧客のデータを使用できる広告技術の目的を定義します。 顧客IDを処理するには、次の目的を選択する必要 [!DNL Real-time CDP] があります。 <ul><li>**目的1**:デバイス上の情報の保存/アクセス</li><li>**目的10**:製品の開発と改善</li></ul> |
| **仕入先権限** | 広告技術の目的に加えて、このダイアログでは、 [!DNL  Real-time CDP] (565)を含む特定のベンダーが顧客のデータを使用するか、使用しないかを許可する必要もあります。 |

### 同意文字列 {#consent-strings}

データの収集方法に関係なく、目標は、顧客が選択した同意オプション（同意文字列）に基づいて文字列値を生成することです。

TCF仕様では、ポリシーおよびベンダーの定義に従って、顧客の同意設定に関する関連する詳細を特定のマーケティング目的の観点からエンコードする際に、同意文字列を使用します。 [!DNL Real-time CDP] これらの文字列を使用して各顧客の同意設定を保存します。したがって、これらの設定が変更されるたびに新しい同意文字列を作成する必要があります。

同意文字列は、IAB TCFに登録されたCMPによってのみ作成できます。 特定のCMPを使用して同意文字列を生成する方法の詳細については、IAB TCF GitHubリポジトリの [同意文字列書式化ガイド](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md) を参照してください。

## IABの同意フィールドを使用してデータセットを作成する {#datasets}

スキーマにIAB同意フィールドが含まれるデータセットに、顧客の同意データを送信する必要があります。 このガイドを続ける前に、必要な2つのデータセットを作成する方法については、TCF 2.0の同意を得るためのデータセットの [作成に関するチュートリアルを参照してください](./dataset-preparation.md) 。

## 同意データを含めるための [!DNL Profile] 結合ポリシーの更新 {#merge-policies}

同意データ収集用の [!DNL Profile]有効なデータセットを作成したら、マージポリシーが顧客プロファイルに常にIAB同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性のある他のデータセットよりも同意データセットの優先順位を優先するように、データセットの優先順位を設定する必要があります。

マージポリシーの使用方法の詳細については、『 [マージポリシーユーザーガイド](../../../profile/ui/merge-policies.md)』を参照してください。 マージポリシーを設定する場合は、データセットの準備に関するガイドで概要を説明しているように、 [](./dataset-preparation.md#privacy-mixin)XDMプライバシーミックスインが提供する必要な同意属性がすべてセグメントに含まれていることを確認する必要があります。

## Web SDKを統合して顧客の同意データを収集する [!DNL Experience Platform] {#sdk}

>[!NOTE]
>
>Adobe Experience Platformで同意データを直接処理するには、 [!DNL Experience Platform] Web SDKを使用する必要があります。 [!DNL Experience Cloud Identity Service] は現在サポートされていません。
>
>[!DNL Experience Cloud Identity Service] は、Adobe Audience Managerでの同意処理に対して引き続きサポートされますが、TCF 2.0への準拠には、ライブラリを [バージョン5.0に更新する必要があり](https://github.com/Adobe-Marketing-Cloud/id-service/releases)ます。

同意文字列を生成するようにCMPを設定したら、 [!DNL Experience Platform] Web SDKを統合してこれらの文字列を収集し、に送信する必要があり [!DNL Platform]ます。 SDKは、IABの同意データをプラットフォームに送信するために使用できる2つのコマンドを提供します（以下のサブセクションで説明）。顧客が初めて同意情報を提供した場合、その後同意が変更された場合に必ず使用します。 [!DNL Platform]

**SDKは、CMPとのインターフェイスを初期状態で行いません**。 SDKをWebサイトに統合する方法を決定し、CMPでの同意の変更をリッスンして、適切なコマンドを呼び出す必要があります。

### 新しいエッジ設定を作成する

SDKがデータを送信する先として、 [!DNL Experience Platform]最初にでの新しいエッジ設定を作成する必要 [!DNL Platform] があり [!DNL Adobe Experience Platform Launch]ます。 新しい設定を作成する方法に関する具体的な手順は、 [SDKドキュメントで説明し](../../../edge/fundamentals/edge-configuration.md)ます。

設定に一意の名前を指定した後、 **[!UICONTROL Adobe Experience Platformの横にある切り替えボタンを選択します]**。 次に、次の値を使用してフォームの残りの部分を完成させます。

| エッジ設定フィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | エッジ設定を設定するために必要なストリーミング接続とデータセットが含まれる [!DNL Platform] Sandboxの名前 [](../../../sandboxes/home.md) 。 |
| [!UICONTROL ストリーミングインレット] | の有効なストリーミング接続 [!DNL Experience Platform]。 既存のストリーミングインレットがない場合は、ストリーミング接続の [作成に関するチュートリアルを参照してください](../../../ingestion/tutorials/create-streaming-connection-ui.md) 。 |
| [!UICONTROL イベントデータセット] | [!DNL XDM ExperienceEvent] 前の手順で作成した [データセットを選択します](#datasets)。 |
| [!UICONTROL プロファイルデータセット] | [!DNL XDM Individual Profile] 前の手順で作成した [データセットを選択します](#datasets)。 |

![](../assets/iab/edge-config.png)

完了したら、画面の下部にある「 **[!UICONTROL 保存]** 」をクリックし、追加のプロンプトに従って設定を完了します。

### 同意変更コマンドの作成

前の節で説明したエッジ設定を作成したら、SDKコマンドを使用して開始し、に同意データを送信でき [!DNL Platform]ます。 以下の節では、各SDKコマンドが様々なシナリオでどのように使用できるかの例を示します。

>[!NOTE]
>
>すべての [!DNL Platform] SDKコマンドの共通の構文については、コマンドの [実行に関するドキュメントを参照してください](../../../edge/fundamentals/executing-commands.md)。

#### CMP同意変更フックの使用

多くのCMPは、同意変更イベントをリッスンする、すぐに使用できるフックを提供します。 これらのイベントが発生した場合は、 `setConsent` コマンドを使用して顧客の同意データを更新できます。

この `setConsent` コマンドには、次の2つの引数が必要です。(1)コマンドタイプ（この場合は「setConsent」）と、(2)配列を含むペイロードを示す文字列。このペイロードには、以下に示すように、必要な同意フィールドを提供する少なくとも1つのオブジェクトを含める必要があります。 `consent`

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
| `standard` | 使用される同意基準。 TCF 2.0の同意処理を行うには、この値を「IAB」に設定する必要があります。 |
| `version` | に示す同意基準のバージョン番号 `standard`。 TCF 2.0の同意処理を行う場合は、この値を「2.0」に設定する必要があります。 |
| `value` | CMPによって生成された、Base-64エンコードされた同意文字列。 |
| `gdprApplies` | GDPRが現在ログインしている顧客に適用されるかどうかを示すBoolean値。 TCF 2.0をこのお客様に適用するには、値を「true」に設定する必要があります。 |

この `setConsent` コマンドは、同意設定の変更を検出するCMPフックの一部として使用する必要があります。 次のJavaScriptは、OneTrustのフックに `setConsent``OnConsentChanged` コマンドを使用する方法の例を示しています。

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

#### イベントの使用

また、コマンドを使用して、でトリガーされるすべてのイベントのTCF 2.0同意データ [!DNL Platform] を収集することもで `sendEvent` きます。

>[!NOTE]
>
>この方法を使用するには、を [!DNL Experience Event Privacy mixin] 有効にした [!DNL Profile][!DNL XDM ExperienceEvent] スキーマに追加しておく必要があります。 設定方法の手順については、『データセット準備ガイド [』のExperienceEventスキーマの](./dataset-preparation.md#event-schema) 更新に関する節を参照してください。

この `sendEvent` コマンドは、Webサイトの適切なイベントリスナーのコールバックとして使用する必要があります。 コマンドには2つの引数が必要です。(1)コマンドタイプ（この場合は「sendEvent」）を示す文字列、および(2)必要な同意フィールドをJSONとして提供する `xdm` オブジェクトを含むペイロードを示します。

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
| `consentStandard` | 使用される同意基準。 TCF 2.0の同意処理を行うには、この値を「IAB」に設定する必要があります。 |
| `consentStandardVersion` | に示す同意基準のバージョン番号 `standard`。 TCF 2.0の同意処理を行う場合は、この値を「2.0」に設定する必要があります。 |
| `consentStringValue` | CMPによって生成された、Base-64エンコードされた同意文字列。 |
| `gdprApplies` | GDPRが現在ログインしている顧客に適用されるかどうかを示すBoolean値。 TCF 2.0をこのお客様に適用するには、値を「true」に設定する必要があります。 |

### SDKレスポンスの処理

すべての [!DNL Platform SDK] コマンドは、呼び出しが成功したか失敗したかを示すプロミスを返します。 その後、これらの応答を、確認メッセージを顧客に表示するなどの追加のロジックに使用できます。 特定の例については、SDKコマンドの実行に関するガイドの成功または失敗の [処理に関する節を参照してください](../../../edge/fundamentals/executing-commands.md#handling-success-or-failure) 。

## セグメントの書き出し {#export}

>[!NOTE]
>
>セグメントの書き出しを開始する前に、セグメントにすべての必須の同意フィールドが含まれていることを確認する必要があります。 詳細は、結合ポリシーの [設定に関する節を参照してください](#merge-policies) 。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリームの宛先にエクスポートする際に、TCF 2.0準拠を強制できます。

同意設定が顧客プロファイルのセット `gdprApplies``true` に対して設定されている場合、それらのプロファイルから下流の宛先にエクスポートされたデータは、各プロファイルの同意嗜好に基づいてフィルタリングされます。 必要な同意の環境設定を満たさないプロファイルは、エクスポート処理中にスキップされます。

プロファイルを宛先にエクスポートするセグメントに含めるには、 [TCF 2.0ポリシーで説明されているように、お客様は次の目的(TCF 2.0ポリシー](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions))に同意する必要があります。

* **目的1**:デバイス上の情報の保存/アクセス
* **目的10**:製品の開発と改善

TCF 2.0では、データの送信元は、送信先のベンダー権限を確認してから、送信先にデータを送信する必要もあります。 そのため、は、宛先にバインドされたデータを含める前に、宛先のベンダー権限がクラスター内のすべてのIDにオプトインされているかどうかを確認します。 [!DNL Real-time CDP]

>[!NOTE]
>
>Adobe Audience Managerと共有するセグメントには、そのセグメントと同じTCF 2.0同意値が含まれ [!DNL Platform] ます。 ベンダーID [!DNL Audience Manager] は [!DNL Real-time CDP] (565)と同じなので、同じ目的とベンダー権限が必要です。 詳しくは、 [Adobe Audience Managerプラグイン（IAB TCF用）のドキュメントを参照してください](https://docs.adobe.com/help/ja-JP/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html) 。

## Test your implementation {#test-implementation}

TCF 2.0実装を設定し、セグメントを宛先にエクスポートした後は、同意の要件を満たさないデータはエクスポートされません。 ただし、適切な顧客プロファイルがエクスポート中にフィルターされたかどうかを確認するには、宛先のデータストアを手動で確認し、同意が適切に適用されたかどうかを確認する必要があります。

複数のIDがクラスターを構成し、TCF 2.0が適用される場合、1つのIDに正しい目的とベンダー権限が含まれていなくても、クラスター全体が除外されることに注意してください。

## 次の手順

このドキュメントでは、でのデータ操作をTCF 2.0に準拠するように設定するプロセス [!DNL Real-time CDP] について説明しました。で提供される他のプライバシー機能の詳細については、次のドキュメント [!DNL Real-time CDP]を参照してください。

* [リアルタイム CDP のプライバシー](../privacy-overview.md)
* [リアルタイム CDP におけるデータガバナンス](../data-governance-overview.md)