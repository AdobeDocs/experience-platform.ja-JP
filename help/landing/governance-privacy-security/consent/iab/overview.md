---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: Experience Platformでの IAB TCF 2.0 のサポート
topic-legacy: privacy events
description: Adobe Experience Platformの宛先にセグメントをアクティブ化する際に、顧客の同意を伝えるデータ操作とスキーマを設定する方法について説明します。
exl-id: af787adf-b46e-43cf-84ac-dfb0bc274025
source-git-commit: 8ded2aed32dffa4f0923fedac7baf798e68a9ec9
workflow-type: tm+mt
source-wordcount: '2558'
ht-degree: 3%

---

# Experience Platformでの IAB TCF 2.0 のサポート

この [!DNL Transparency & Consent Framework] (TCF)。 [!DNL Interactive Advertising Bureau] (IAB) は、EU の [!DNL General Data Protection Regulation] (GDPR)。 フレームワークの 2 番目の反復である TCF 2.0 は、ベンダーが正確な位置情報などのデータ処理の特定の機能を使用するかどうかや方法など、消費者が同意を提供または拒否する方法を柔軟に提供します。

>[!NOTE]
>
>TCF 2.0 の詳細については、 [IAB Europe の Web サイト](https://iabeurope.eu/tcf-2-0/)（サポート資料および技術仕様を含む）

Adobe Experience Platformは登録済みの [IAB TCF 2.0 ベンダーリスト](https://iabeurope.eu/vendor-list-tcf-v2-0/)（ID の下） **565**. TCF 2.0 の要件に準拠して、Platform を使用すると、顧客の同意データを収集し、保存された顧客プロファイルに統合できます。 その後、この同意データは、使用例に応じて、プロファイルがエクスポートされたオーディエンスセグメントに含まれるかどうかに考慮できます。

>[!IMPORTANT]
>
>Platform は、TCF のバージョン 2.0（またはそれ以降）にのみ準拠できます。 以前のバージョンの TCF はサポートされていません。

このドキュメントでは、CMP で生成される顧客の同意データを受け入れるようにデータ操作とプロファイルスキーマを設定する方法の概要と、セグメントをエクスポートする際の Platform によるユーザーの同意選択の伝達方法を説明します。

## 前提条件

このガイドに従うには、IAB TCF に統合され、準拠している、商用または独自の同意管理プラットフォーム (CMP) を使用する必要があります。 詳しくは、 [準拠している CMP のリスト](https://iabeurope.eu/cmp-list/) を参照してください。

>[!IMPORTANT]
>
>CMP の ID が無効な場合、Platform はデータをそのまま処理し続けます。 TCF 2.0 を強制するには、データを Platform に送信する前に、IAB TCF 2.0 に登録されている有効な ID が CMP に含まれていることを確認する必要があります。

また、このガイドでは、次の Platform サービスに関する十分な知識が必要です。

* [Experience Data Model（XDM）](../../../../xdm/home.md)：Adobe Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステム間で ID を結び付けることで、顧客体験のフラグメント化によって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):活用 [!DNL Identity Service] を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。 [!DNL Real-time Customer Profile] はデータレイクからデータを取り込み、顧客プロファイルを独自の別々のデータストアに保持します。
* [Adobe Experience Platform Web SDK](../../../../edge/home.md):様々な Platform サービスを顧客向け Web サイトに統合できる、クライアント側の JavaScript ライブラリ。
   * [SDK の同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す同意関連の SDK コマンドの使用例の概要です。
* [Adobe Experience Platform Segmentation Service](../../../../segmentation/home.md):分割可能 [!DNL Real-time Customer Profile] 類似した特性を共有し、マーケティング戦略に同様の応答をする個人のグループにデータを送信します。

上記の Platform サービスに加えて、 [宛先](../../../../data-governance/home.md) Platform エコシステムでの役割も果たします。

## 顧客の同意フローの概要 {#summary}

次の節では、システムが適切に設定された後に同意データが収集され、適用される方法について説明します。

### 同意データ収集

Platform では、次のプロセスを通じて顧客の同意データを収集できます。

1. 顧客が Web サイト上のダイアログを通じて、データ収集に関する同意設定を提供します。
1. CMP が同意設定の変更を検出し、それに応じて TCF 同意データを生成します。
1. Platform Web SDK を使用して、生成された同意データ（CMP によって返される）がAdobe Experience Platformに送信されます。
1. 収集された同意データは、 [!DNL Profile]スキーマに TCF 同意フィールドが含まれる有効なデータセット。

CMP の同意変更フックによってトリガーされる SDK コマンドに加えて、同意データは、に直接アップロードされる、お客様が生成した XDM データを通じてExperience Platformに送ることもできます。 [!DNL Profile]-enabled データセット。

Adobe Audience Managerによって Platform と共有されたセグメント ( [!DNL Audience Manager] また、適切なフィールドが [!DNL Experience Cloud Identity Service]. での同意データの収集に関する詳細 [!DNL Audience Manager]を参照し、 [IAB TCF 用Adobe Audience Managerプラグイン](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=ja).

### ダウンストリームの同意の実施

TCF の同意データが正常に取り込まれると、ダウンストリーム Platform サービスで次のプロセスが実行されます。

1. [!DNL Real-time Customer Profile] 保存されている同意データを、その顧客のプロファイルに対して更新します。
1. Platform が顧客 ID を処理するのは、クラスター内の各 ID に対して Platform(565) のベンダー権限が指定されている場合のみです。
1. TCF 2.0 ベンダーリストのメンバーに属する宛先にセグメントを書き出す場合、Platform は、両方の Platform に対するベンダー権限 (565) がある場合にのみプロファイルを含みます *および* 個々の宛先は、クラスター内の各 ID に対して提供されます。

このドキュメントの残りの節では、前述の収集要件と実施要件を満たすように Platform とデータ操作を設定する方法に関するガイダンスを提供します。

## CMP 内で顧客の同意データを生成する方法を決定する {#consent-data}

各 CMP システムは固有なので、お客様がサービスとやり取りする際に同意を得る最適な方法を決定する必要があります。 これを実現する一般的な方法は、次の例のような Cookie の同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/iab/overview/cmp-dialog.png)

このダイアログでは、顧客が以下のオプトインまたはオプトアウトできるようにする必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的は、ブランドが顧客のデータを使用できる広告技術の目的を定義します。 Platform が顧客 ID を処理するには、次の目的をオプトインする必要があります。 <ul><li>**目的 1**:デバイス上での情報の保存/アクセス</li><li>**目的 10**:製品の開発と改善</li></ul> |
| **ベンダー権限** | 広告技術の目的に加えて、このダイアログでは、Adobe Experience Platform(565) を含む特定のベンダーがデータを使用することを顧客がオプトインまたはオプトアウトできるようにする必要があります。 |

### 同意文字列 {#consent-strings}

データの収集に使用する方法に関係なく、目標は、お客様が選択した同意オプション（同意文字列と呼ばれる）に基づいて文字列値を生成することです。

TCF 仕様では、同意文字列を使用して、ポリシーやベンダーが定義した特定のマーケティング目的に基づいて、顧客の同意設定に関する関連する詳細をエンコードします。 Platform では、これらの文字列を使用して各顧客の同意設定を保存します。したがって、これらの設定が変更されるたびに新しい同意文字列を生成する必要があります。

同意文字列は、IAB TCF に登録されている CMP によってのみ作成できます。 特定の CMP を使用して同意文字列を生成する方法について詳しくは、 [同意文字列書式設定ガイド](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md) （IAB TCF GitHub リポジトリ）を参照してください。

## TCF 同意フィールドを使用したデータセットの作成 {#datasets}

顧客の同意データは、TCF 同意フィールドを含むスキーマのデータセットに送信する必要があります。 次のチュートリアルを参照してください。 [TCF 2.0 同意を取り込むためのデータセットの作成](./dataset.md) 必要なプロファイルデータセット（およびオプションのエクスペリエンスイベントデータセット）を作成する方法については、このガイドを参照してください。

## 更新 [!DNL Profile] 同意データを含めるためのポリシーの結合 {#merge-policies}

以下を作成したら、 [!DNL Profile]：同意データを収集するための有効なデータセット。結合ポリシーが、顧客プロファイルに TCF 同意フィールドを常に含むように設定されていることを確認する必要があります。 これには、競合する可能性のある他のデータセットよりも同意データセットの方が優先されるように、データセットの優先順位を設定する必要があります。

結合ポリシーの操作方法について詳しくは、 [結合ポリシーの概要](../../../../profile/merge-policies/overview.md). 結合ポリシーを設定する場合、セグメントに、 [XDM プライバシースキーマフィールドグループ](./dataset.md#privacy-field-group)（データセットの準備に関するガイドで説明）

## Experience PlatformWeb SDK を統合して、顧客の同意データを収集する {#sdk}

>[!NOTE]
>
>Adobe Experience Platformで直接同意データを処理するには、Experience PlatformWeb SDK を使用する必要があります。 [!DNL Experience Cloud Identity Service] は現在サポートされていません。
>
>[!DNL Experience Cloud Identity Service] は引き続きAdobe Audience Managerでの同意処理に対してサポートされますが、TCF 2.0 への準拠には、ライブラリを [バージョン 5.0](https://github.com/Adobe-Marketing-Cloud/id-service/releases).

同意文字列を生成するように CMP を設定したら、Experience PlatformWeb SDK を統合してこれらの文字列を収集し、Platform に送信する必要があります。 Platform SDK は、TCF の同意データを Platform に送信するために使用できる 2 つのコマンドを提供します（以下のサブ節で説明）。顧客が初めて同意情報を提供する場合と、その後同意が変更される場合に使用します。

**SDK は、標準の CMP とのインターフェイスを提供しません**. SDK を Web サイトに統合する方法を決定し、CMP で同意の変更をリッスンして、適切なコマンドを呼び出すかどうかは、ユーザーが決定します。

### 新しいデータストリームの作成

SDK がExperience Platformにデータを送信するには、まず Platform 用の新しいデータストリームを作成する必要があります。 新しいデータストリームを作成する方法に関する具体的な手順は、 [SDK ドキュメント](../../../../edge/datastreams/overview.md).

データストリームに一意の名前を指定した後、の横にある切り替えボタンを選択します。 **[!UICONTROL Adobe Experience Platform]**. 次に、次の値を使用して、フォームの残りの部分を完了します。

| データストリームフィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | プラットフォームの名前 [サンドボックス](../../../../sandboxes/home.md) データストリームを設定するために必要なストリーミング接続とデータセットを含む |
| [!UICONTROL ストリーミングインレット] | 有効なストリーミングExperience Platform。 に関するチュートリアルを参照してください。 [ストリーミング接続の作成](../../../../ingestion/tutorials/create-streaming-connection-ui.md) 既存のストリーミングインレットがない場合。 |
| [!UICONTROL イベントデータセット] | を選択します。 [!DNL XDM ExperienceEvent] データセットを [前の手順](#datasets). 次を含む場合： [[!UICONTROL IAB TCF 2.0 同意] フィールドグループ](../../../../xdm/field-groups/event/iab.md) このデータセットのスキーマでは、 [`sendEvent`](#sendEvent) コマンドを使用して、そのデータをこのデータセットに保存します。 このデータセットに保存される同意の値は次のとおりです **not** 自動強制ワークフローで使用されます。 |
| [!UICONTROL プロファイルデータセット] | を選択します。 [!DNL XDM Individual Profile] データセットを [前の手順](#datasets). CMP の同意変更フックに応答する場合は、 [`setConsent`](#setConsent) コマンドを使用すると、収集したデータがこのデータセットに保存されます。 このデータセットはプロファイル対応なので、自動実施ワークフローの間、このデータセットに保存される同意の値は保持されます。 |

![](../../../images/governance-privacy-security/consent/iab/overview/edge-config.png)

終了したら、「 」を選択します。 **[!UICONTROL 保存]** 画面の下部で、追加のプロンプトに従って設定を完了します。

### 同意変更コマンドの実行

前の節で説明したデータストリームを作成したら、SDK コマンドを使用して、Platform に同意データを送信できます。 以下の節では、各 SDK コマンドが様々なシナリオでどのように使用できるかの例を示します。

>[!NOTE]
>
>すべての Platform SDK コマンドの一般的な構文の概要については、 [コマンドの実行](../../../../edge/fundamentals/executing-commands.md).

#### CMP 同意変更フックの使用 {#setConsent}

多くの CMP は、同意変更イベントをリッスンする標準のフックを提供します。 これらのイベントが発生した場合、 `setConsent` 」コマンドを使用して、顧客の同意データを更新する必要があります。

この `setConsent` コマンドには次の 2 つの引数が必要です。(1) コマンドの種類（この場合は「setConsent」）を示す文字列、および (2) `consent` 配列。次に示すように、必要な同意フィールドを提供する 1 つ以上のオブジェクトを含む必要があります。

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
| `standard` | 使用される同意の規格。 この値はに設定する必要があります。 `IAB` （TCF 2.0 同意処理用） |
| `version` | 以下に示す同意標準のバージョン番号 `standard`. この値はに設定する必要があります。 `2.0` （TCF 2.0 同意処理用） |
| `value` | CMP によって生成される、base-64 でエンコードされた同意文字列。 |
| `gdprApplies` | GDPR が現在ログインしている顧客に適用されるかどうかを示す Boolean 値。 この顧客に TCF 2.0 を適用するには、値をに設定する必要があります。 `true`. デフォルトはです。 `true` （定義されていない場合） |

この `setConsent` コマンドは、同意設定の変更を検出する CMP フックの一部として使用する必要があります。 次の JavaScript は、 `setConsent` コマンドを OneTrust の `OnConsentChanged` フック：

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

また、 `sendEvent` コマンドを使用します。

>[!NOTE]
>
>この方法を使用するには、「エクスペリエンスイベントプライバシー」フィールドグループを [!DNL Profile] — 有効 [!DNL XDM ExperienceEvent] スキーマ。 詳しくは、 [ExperienceEvent スキーマの更新](./dataset.md#event-schema) （データセット準備ガイド）を参照してください。

この `sendEvent` コマンドは、web サイト上の適切なイベントリスナーでコールバックとして使用する必要があります。 このコマンドは、次の 2 つの引数を受け取ります。(1) コマンドの種類を示す文字列 ( この場合は `sendEvent`) および (2) `xdm` 必要な同意フィールドを JSON として提供するオブジェクト。

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
| `xdm.consentStrings` | 必須の同意フィールドを提供する 1 つ以上のオブジェクトを含む必要がある配列。 |
| `consentStandard` | 使用される同意の規格。 この値はに設定する必要があります。 `IAB` （TCF 2.0 同意処理用） |
| `consentStandardVersion` | 以下に示す同意標準のバージョン番号 `standard`. この値はに設定する必要があります。 `2.0` （TCF 2.0 同意処理用） |
| `consentStringValue` | CMP によって生成される、base-64 でエンコードされた同意文字列。 |
| `gdprApplies` | GDPR が現在ログインしている顧客に適用されるかどうかを示す Boolean 値。 この顧客に TCF 2.0 を適用するには、値をに設定する必要があります。 `true`. デフォルトはです。 `true` （定義されていない場合） |

### SDK 応答の処理

すべて [!DNL Platform SDK] コマンドは、呼び出しが成功したか失敗したかを示す promise を返します。 その後、これらの応答を、顧客への確認メッセージの表示などの追加ロジックに使用できます。 詳しくは、 [成功または失敗の処理](../../../../edge/fundamentals/executing-commands.md#handling-success-or-failure) （特定の例に関する SDK コマンドの実行に関するガイド）。

## セグメントのエクスポート {#export}

>[!NOTE]
>
>セグメントのエクスポートを開始する前に、セグメントに必要な同意フィールドがすべて含まれていることを確認する必要があります。 詳しくは、 [結合ポリシーの設定](#merge-policies) を参照してください。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリームの宛先に書き出す際に、TCF 2.0 への準拠を実施できます。

同意設定が `gdprApplies` が `true` 一連の顧客プロファイルの場合、ダウンストリームの宛先に書き出されたプロファイルのデータは、各プロファイルの TCF 同意設定に基づいてフィルタリングされます。 必要な同意設定を満たさないプロファイルは、エクスポートプロセス中にスキップされます。

お客様は、次の目的 ( [TCF 2.0 ポリシー](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions)) を使用して、セグメントに含めることができます。

* **目的 1**:デバイス上での情報の保存/アクセス
* **目的 10**:製品の開発と改善

また、TCF 2.0 では、データのソースは、宛先にデータを送信する前に、宛先のベンダー権限を確認する必要があります。 したがって、Platform は、宛先にバインドされたデータを含める前に、宛先のベンダー権限が、クラスター内のすべての ID に対してオプトインされているかどうかを確認します。

>[!NOTE]
>
>Adobe Audience Managerと共有されるセグメントには、Platform と同じ TCF 2.0 同意値が含まれます。 次以降 [!DNL Audience Manager] は、Platform(565) と同じベンダー ID を共有し、同じ目的とベンダー権限が必要です。 ドキュメントを [IAB TCF 用Adobe Audience Managerプラグイン](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html) を参照してください。

## 実装をテストする {#test-implementation}

TCF 2.0 の実装を設定し、宛先にセグメントを書き出した後は、同意の要件を満たさないデータは書き出されません。 ただし、書き出し中に適切な顧客プロファイルがフィルタリングされたかどうかを確認するには、宛先のデータストアを手動で確認して、同意が適切に適用されたかどうかを確認する必要があります。

複数の ID がクラスターを構成し、TCF 2.0 が適用される場合、1 つの ID にも正しい目的とベンダー権限が含まれていないと、クラスター全体が除外されることに注意する必要があります。

## 次の手順

このドキュメントでは、TCF 2.0 で概要を説明しているビジネス上の義務を満たすように Platform データ操作を設定するプロセスについて説明しました。 [ガバナンス、プライバシー、セキュリティ](../../overview.md) を参照してください。
