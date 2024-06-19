---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: Experience Platformでの IAB TCF 2.0 のサポート
description: Adobe Experience Platformで宛先に対してセグメントをアクティブ化する際に、顧客の同意の選択肢を伝えるためにデータ操作とスキーマを設定する方法について説明します。
exl-id: af787adf-b46e-43cf-84ac-dfb0bc274025
source-git-commit: b08c6cf12a38f79e019544dea91913a77bd6490a
workflow-type: tm+mt
source-wordcount: '2492'
ht-degree: 1%

---

# Experience Platformでの IAB TCF 2.0 のサポート

この [!DNL Transparency & Consent Framework] （TCF）の概要 [!DNL Interactive Advertising Bureau] （IAB）は、組織が EU に準拠して、個人データの処理に関する消費者の同意を取得、記録、更新できるようにすることを目的とした、オープン標準の技術フレームワークです [!DNL General Data Protection Regulation] （GDPR）。 このフレームワークの 2 つ目のイテレーションである TCF 2.0 では、ベンダーが正確な位置情報などのデータ処理の特定の機能を使用できるかどうかや、使用する方法など、消費者が同意を提供または留保する方法について、より柔軟に対応しています。

>[!NOTE]
>
>TCF 2.0 の詳細については、 [IAB Europe web サイト](https://iabeurope.eu/)（サポート資料および技術仕様を含む）

Adobe Experience Platformは、登録済みの [IAB TCF 2.0 ベンダーリスト](https://iabeurope.eu/vendor-list-tcf/)、ID の下 **565**. TCF 2.0 要件に準拠して、Platform を使用すると、顧客同意データを収集し、保存されている顧客プロファイルに統合できます。 この同意データは、ユースケースに応じて、書き出されたオーディエンスセグメントにプロファイルが含まれているかどうかの要因となります。

>[!IMPORTANT]
>
>Platform は、TCF バージョン 2.0 （以降）にのみ準拠できます。 以前のバージョンの TCF はサポートされていません。

このドキュメントでは、お客様の同意管理プラットフォーム（CMP）によって生成された顧客同意データを受け入れるようにデータ操作とプロファイルスキーマを設定する方法の概要について説明します。 また、セグメントを書き出す際のユーザーの同意の選択肢を Platform がどのように伝えるかについても説明します。

## 前提条件

このガイドに従うには、商用または独自の CMP を使用し、IAB TCF に統合および準拠する必要があります。 を参照してください。 [準拠している CMP のリスト](https://iabeurope.eu/cmp-list/) を参照してください。

>[!IMPORTANT]
>
>CMP の ID が無効な場合、Platform はデータを現状のまま処理し続けます。 TCF 2.0 を強制するには、データを Platform に送信する前に、CMP に有効な ID が IAB TCF 2.0 に登録されていることを確認する必要があります。

このガイドでは、次の Platform サービスに関する十分な知識も必要です。

* [Experience Data Model（XDM）](/help/xdm/home.md)：Adobe Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [Adobe Experience Platform ID サービス](/help/identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験のフラグメント化によって発生する基本的な課題を解決します。
* [リアルタイム顧客プロファイル](/help/profile/home.md)：使用 [!DNL Identity Service] を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。 [!DNL Real-Time Customer Profile] データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持する
* [Adobe Experience Platform Web SDK](/help/web-sdk/home.md)：様々な Platform サービスを顧客向け web サイトに統合できるクライアントサイド JavaScript ライブラリ。
   * [SDK 同意コマンド](../../../../web-sdk/commands/setconsent.md)：このガイドに示される同意関連の SDK コマンドのユースケースの概要。
* [Adobe Experience Platform セグメント化サービス](/help/segmentation/home.md)：除算の許可 [!DNL Real-Time Customer Profile] 類似の特性を共有し、マーケティング戦略と同様に応答する個人のグループへのデータ。

上記の Platform サービスに加えて、についても理解しておく必要があります [宛先](/help/data-governance/home.md) および Platform エコシステムでの役割。

## 顧客同意フローの概要 {#summary}

次の節では、システムが適切に設定された後に同意データを収集および適用する方法について説明します。

### 同意データの収集

Platform では、次のプロセスを通じて顧客同意データを収集できます。

1. 顧客は、web サイト上のダイアログを通じて、データ収集に対する同意環境設定を指定します。
1. CMP が同意設定の変更を検出し、それに応じて TCF 同意データを生成します。
1. Platform Web SDK を使用して、生成された同意データ（CMP によって返される）がAdobe Experience Platformに送信されます。
1. 収集された同意データはに取り込まれます。 [!DNL Profile]スキーマに TCF 同意フィールドが含まれている対応データセット。

CMP の同意変更フックによってトリガーされる SDK コマンドに加えて、同意データは、に直接アップロードされる、お客様が生成した XDM データを介してExperience Platformへと送ることもできます [!DNL Profile]が有効なデータセット。

Adobe Audience Managerが Platform と（を通じて）共有するセグメント [!DNL Audience Manager] ソースコネクタなど）には、を通じて適切なフィールドがそれらのセグメントに適用されている場合、同意データが含まれることもあります [!DNL Experience Cloud Identity Service]. での同意データの収集について詳しくは、 [!DNL Audience Manager]に関するドキュメントを参照してください。 [IAB TCF 用Adobe Audience Manager プラグイン](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja).

### ダウンストリームの同意の適用

TCF 同意データが正常に取り込まれると、ダウンストリーム Platform サービスで次のプロセスが実行されます。

1. [!DNL Real-Time Customer Profile] その顧客のプロファイルに保存された同意データを更新します。
1. Platform は、クラスター内のすべての ID に対して Platform （565）のベンダー権限が提供されている場合にのみ、顧客 ID を処理します。
1. TCF 2.0 ベンダーリストのメンバーに属する宛先にセグメントを書き出す場合、Platform には、両方の Platform に対するベンダー権限（565）がある場合にのみプロファイルが含まれます *および* 個々の宛先は、クラスター内のすべての ID に対して提供されます。

このドキュメントの残りの節では、前述の収集および実施要件を満たすために Platform とデータ操作を設定する方法について説明します。

## CMP 内で顧客同意データを生成する方法の決定 {#consent-data}

各 CMP システムは一意なので、顧客がサービスとやり取りする際に同意を提供できる最適な方法を決定する必要があります。 Cookie 同意ダイアログは、顧客の同意を得るための一般的な方法です。 CMP ダイアログの例を以下に示します。

![同意管理プラットフォームダイアログの例。](../../../images/governance-privacy-security/consent/iab/overview/cmp-dialog.png)

このダイアログで、顧客が次の項目をオプトイン/オプトアウトできるようにする必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的ブランドが顧客のデータを使用できる広告技術目的を定義します。 Platform で顧客 ID を処理する際には、次の目的を選択する必要があります。 <ul><li>**目的 1**：デバイス上での情報の保存やアクセス</li><li>**目的 10**：製品の開発と改善</li></ul> |
| **ベンダー権限** | このダイアログでは、広告技術の目的に加えて、お客様がAdobe Experience Platform（565）などの特定のベンダーでデータを使用することをオプトインまたはオプトアウトできるようにする必要があります。 |

### 同意文字列 {#consent-strings}

データの収集に使用する方法に関係なく、顧客が選択した同意オプションに基づいて文字列値を生成することが目標になります。これは、同意文字列と呼ばれます。

TCF の仕様では、ポリシーやベンダーで定義されている特定のマーケティング目的に照らして、顧客の同意設定に関する関連する詳細をエンコードするために同意文字列が使用されます。 Platform では、これらの文字列を使用して各顧客の同意設定を保存するので、設定が変更されるたびに新しい同意文字列を生成する必要があります。

同意文字列は、IAB TCF に登録されている CMP によってのみ作成できます。 特定の CMP を使用して同意文字列を生成する方法について詳しくは、 [同意文字列フォーマットガイド](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md) （IAB TCF GitHub リポジトリ内）。

## TCF 同意フィールドを使用したデータセットの作成 {#datasets}

顧客の同意データは、スキーマに TCF 同意フィールドが含まれるデータセットに送信する必要があります。 のチュートリアルを参照してください。 [tcf 2.0 同意を取得するためのデータセットの作成](./dataset.md) このガイドに進む前に必要なプロファイルデータセット（およびオプションのエクスペリエンスイベントデータセット）を作成する方法を説明します。

## 更新 [!DNL Profile] 同意データを含める結合ポリシー {#merge-policies}

を作成したら [!DNL Profile]で有効化されたデータセット同意データを収集する場合は、顧客プロファイルに常に TCF 同意フィールドを含めるように結合ポリシーが設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットが優先されるように、データセットの優先順位を設定することが含まれます。

結合ポリシーの操作方法について詳しくは、を参照してください。 [結合ポリシーの概要](/help/profile/merge-policies/overview.md). 結合ポリシーを設定する場合は、が提供する必要なすべての同意属性がセグメントに含まれていることを確認する必要があります [XDM プライバシースキーマフィールドグループ](./dataset.md#privacy-field-group)データセットの準備に関するガイドで説明しています。

## Experience PlatformWeb SDK を統合して、顧客同意データを収集します {#sdk}

>[!NOTE]
>
>Adobe Experience Platformで同意データを直接処理するには、Experience Platform Web SDK の使用が必要です。 [!DNL Experience Cloud Identity Service] はサポートされていません。
>
>[!DNL Experience Cloud Identity Service] ただし、Adobe Audience Managerでの同意処理では引き続きサポートされ、TCF 2.0 に準拠するには、ライブラリをに更新する必要があります [バージョン 5.0](https://github.com/Adobe-Marketing-Cloud/id-service/releases).

同意文字列を生成するように CMP を設定したら、Experience Platform Web SDK を統合してこれらの文字列を収集し、Platform に送信する必要があります。 Platform SDK には、TCF 同意データを Platform に送信するために使用できる 2 つのコマンドが用意されています（以下のサブセクションで説明します）。 これらのコマンドは、顧客が同意情報を初めて提供する際と、それ以降その同意が変更される場合に使用する必要があります。

**SDK は、初期設定では、どの CMP ともインターフェイスを行いません**. SDK を web サイトに統合する方法を決定し、CMP で同意の変更をリッスンして適切なコマンドを呼び出すかどうかは、ユーザー次第です。

### データストリームの作成

SDK がExperience Platformにデータを送信するには、まず Platform のデータストリームを作成する必要があります。 データストリームの作成方法に関する具体的な手順については、を参照してください [SDK ドキュメント](/help/datastreams/overview.md).

データストリームの一意の名前を指定したら、の横にある切り替えボタンを選択します **[!UICONTROL Adobe Experience Platform]**. 次に、以下の値を使用してフォームの残りの部分を完了します。

| データストリームフィールド | 値 |
| --- | --- |
| [!UICONTROL Sandbox] | プラットフォームの名前 [sandbox](/help/sandboxes/home.md) これには、データストリームを設定するために必要なストリーミング接続とデータセットが含まれています。 |
| [!UICONTROL ストリーミングインレット] | Experience Platformの有効なストリーミング接続。 チュートリアルを参照してください。 [ストリーミング接続の作成](/help/ingestion/tutorials/create-streaming-connection-ui.md) 既存のストリーミングインレットがない場合。 |
| [!UICONTROL イベントデータセット] | 「」を選択します [!DNL XDM ExperienceEvent] で作成されたデータセット [前の手順](#datasets). を含めた場合 [[!UICONTROL IAB TCF 2.0 同意] フィールドグループ](/help/xdm/field-groups/event/iab.md) このデータセットのスキーマでは、を使用して、同意変更イベントを経時的に追跡できます [`sendEvent`](#sendEvent) コマンドを実行し、そのデータをこのデータセットに保存します。 このデータセットに保存される同意値は次の点に注意してください **ではない** 自動適用ワークフローで使用されます。 |
| [!UICONTROL プロファイルデータセット] | 「」を選択します [!DNL XDM Individual Profile] で作成されたデータセット [前の手順](#datasets). を使用して CMP 同意変更フックに応答する場合 [`setConsent`](#setConsent) コマンドを実行すると、収集されたデータがこのデータセットに保存されます。 このデータセットはプロファイル対応なので、このデータセットに保存された同意値は、自動適用ワークフロー中に適用されます。 |

![](../../../images/governance-privacy-security/consent/iab/overview/edge-config.png)

終了したら、 **[!UICONTROL 保存]** 画面の下部で、追加のプロンプトに従って続行し、設定を完了します。

### 同意変更コマンドの実行

前の節で説明したデータストリームを作成したら、SDK コマンドを使用して、同意データを Platform に送信できます。 以下の節では、様々なシナリオで各 SDK コマンドを使用する方法の例を示します。

#### CMP 同意変更フックの使用 {#setConsent}

多くの CMP は、同意変更イベントをリッスンする、すぐに使用できるフックを提供しています。 これらのイベントが発生した場合は、 [`setConsent`](/help/web-sdk/commands/setconsent.md) その顧客の同意データを更新するコマンド。

この `setConsent` コマンドは、次の 2 つの引数を想定しています：

1. コマンドのタイプを示す文字列（この場合は「setConsent」）。
1. を含むペイロード `consent` 配列。 配列には、必要な同意フィールドを提供するオブジェクトが少なくとも 1 つ含まれている必要があります。

この `setConsent` コマンドは次のように表示されます。

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
| `standard` | 使用されている同意標準。 この値はに設定する必要があります。 `IAB` （TCF 2.0 同意処理の場合）。 |
| `version` | に示されている同意標準のバージョン番号 `standard`. この値はに設定する必要があります。 `2.0` （TCF 2.0 同意処理の場合）。 |
| `value` | CMP によって生成された base-64 でエンコードされた同意文字列。 |
| `gdprApplies` | 現在ログインしている顧客に GDPR を適用するかどうかを示すブール値。 TCF 2.0 をこの顧客に適用するには、値をに設定する必要があります `true`. デフォルトは `true` 定義されていない場合。 |

この `setConsent` 同意設定の変更を検出する CMP フックの一部としてコマンドを使用する必要があります。 次の JavaScript は、方法の例を示しています `setConsent` コマンドは、OneTrust のに使用できます `OnConsentChanged` フック：

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

また、を使用すると、Platform でトリガーされるすべてのイベントで TCF 2.0 同意データを収集できます `sendEvent` コマンド。

>[!NOTE]
>
>この方法を使用するには、エクスペリエンスイベントプライバシーフィールドグループをに追加する必要があります [!DNL Profile]-enabled [!DNL XDM ExperienceEvent] スキーマ。 の節を参照してください。 [experienceEvent スキーマの更新](./dataset.md#event-schema) 設定方法の手順については、データセット準備ガイドを参照してください。

この `sendEvent` コマンドは、web サイト上の適切なイベントリスナーでコールバックとして使用する必要があります。 コマンドは、次の 2 つの引数を想定します：（1） コマンドの種類を示す文字列（この場合は `sendEvent`）、および（2）を含むペイロード `xdm` 必須の同意フィールドを JSON として提供するオブジェクト：

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
| `consentStandard` | 使用されている同意標準。 この値はに設定する必要があります。 `IAB` （TCF 2.0 同意処理の場合）。 |
| `consentStandardVersion` | に示されている同意標準のバージョン番号 `standard`. この値はに設定する必要があります。 `2.0` （TCF 2.0 同意処理の場合）。 |
| `consentStringValue` | CMP によって生成された base-64 でエンコードされた同意文字列。 |
| `gdprApplies` | 現在ログインしている顧客に GDPR を適用するかどうかを示すブール値。 TCF 2.0 をこの顧客に適用するには、値をに設定する必要があります `true`. デフォルトは `true` 定義されていない場合。 |

### SDK 応答の処理

多くの Web SDK コマンドは、呼び出しが成功したか失敗したかを示すプロミスを返します。 その後、これらの応答を使用して、顧客に確認メッセージを表示するなどの追加のロジックを実行できます。 参照： [コマンドの応答](/help/web-sdk/commands/command-responses.md) を参照してください。

## セグメントの書き出し {#export}

>[!NOTE]
>
>セグメントの書き出しを開始する前に、セグメントにすべての必須同意フィールドが含まれていることを確認する必要があります。 の節を参照してください。 [結合ポリシーの設定](#merge-policies) を参照してください。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリーム宛先に書き出す際に、TCF 2.0 準拠を適用できます。

同意設定の場合 `gdprApplies` はに設定されています。 `true` 一連の顧客プロファイルについては、各プロファイルの TCF 同意環境設定に基づいて、ダウンストリーム宛先に書き出されるプロファイルからのデータがフィルタリングされます。 必要な同意環境設定を満たさないプロファイルは、書き出しプロセス中にスキップされます。

お客様は、以下の目的に同意する必要があります（以下で説明します） [TCF 2.0 ポリシー](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions)）に設定します。

* **目的 1**：デバイス上での情報の保存やアクセス
* **目的 10**：製品の開発と改善

また、TCF 2.0 では、データを宛先に送信する前に、データのソースが宛先のベンダー権限を確認する必要があります。 そのため、Platform は、その宛先にバインドされているデータを含める前に、クラスター内のすべての ID に対する宛先のベンダー権限がにオプトインされているかどうかを確認します。

>[!NOTE]
>
>Platform と共有されるセグメントには、Adobe Audience Managerの対応するセグメントと同じ TCF 2.0 同意値が含まれます。 以降 [!DNL Audience Manager] は Platform （565）と同じベンダー ID を共有するため、同じ目的とベンダーの権限が必要です。 に関するドキュメントを参照してください [IAB TCF 用Adobe Audience Manager プラグイン](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja) を参照してください。

## 実装のテスト {#test-implementation}

TCF 2.0 の実装を設定し、宛先にセグメントを書き出した後は、同意要件を満たさないデータは書き出されません。 書き出し中に正しい顧客プロファイルがフィルタリングされたかどうかを確認するには、宛先のデータストアを手動で確認して、同意が正しく適用されているかどうかを確認する必要があります。

>[!IMPORTANT]
>
>複数の ID がクラスターを構成し、TCF 2.0 が適用される場合、1 つの ID にも正しい目的とベンダー権限が含まれていなくても、クラスター全体が除外されます。

## 次の手順

このドキュメントでは、TCF 2.0 で説明されているビジネス義務を満たすように Platform データ操作を設定するプロセスについて説明しました。概要を参照してください [ガバナンス、プライバシー、セキュリティ](../../overview.md) 詳細情報：Platform のプライバシー関連機能。
