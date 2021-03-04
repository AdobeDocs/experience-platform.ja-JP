---
keywords: Experience Platform；ホーム；IAB;IAB 2.0；同意；同意
solution: Experience Platform
title: Experience PlatformでのIAB TCF 2.0のサポート
topic: プライバシーイベント
description: Adobe Experience Platformの宛先にセグメントをアクティブ化する際に、顧客の同意を伝えるためのデータ操作とスキーマの設定方法を説明します。
translation-type: tm+mt
source-git-commit: a845ade0fc1e6e18c36b5f837fe7673a976f01c7
workflow-type: tm+mt
source-wordcount: '2472'
ht-degree: 1%

---


# Experience PlatformでのIAB TCF 2.0のサポート

[!DNL Transparency & Consent Framework] (TCF)は、[!DNL Interactive Advertising Bureau] (IAB)で概要を説明している通り、オープンスタンダードの技術的枠組みで、GDPR(欧州和集合のa2/>)に従って、個人データの処理に対する消費者の同意を得、記録、更新することを目的としている。 [!DNL General Data Protection Regulation]フレームワークの2番目のイテレーションであるTCF 2.0では、ベンダーが正確な位置情報など、データ処理の特定の機能を使用するかどうか、使用方法など、消費者が同意を提供または保留する方法を柔軟に許可します。

>[!NOTE]
>
>TCF 2.0に関する詳細は、[IAB EuropeのWebサイト](https://iabeurope.eu/tcf-2-0/)に掲載されています。サポート資料や技術仕様も含まれています。

Adobe Experience Platformは、ID **565**&#x200B;の下で、登録された[IAB TCF 2.0ベンダーリスト](https://iabeurope.eu/vendor-list-tcf-v2-0/)の一部です。 TCF 2.0の要件に準拠したプラットフォームを使用すると、顧客の同意データを収集し、保存されている顧客プロファイルに統合できます。 その後、この同意データは、使用事例に応じて、プロファイルが書き出されたオーディエンスセグメントに含まれるかどうかに組み込まれます。

>[!IMPORTANT]
>
>プラットフォームは、TCFのバージョン2.0（またはそれ以降）に対してのみ準拠できます。 以前のバージョンのTCFはサポートされていません。

このドキュメントでは、データ操作とプロファイルスキーマがCMPで生成された顧客の同意データを受け入れるように設定する方法の概要、およびPlatformがセグメントを書き出す際にユーザーの同意を得る方法について説明します。

## 前提条件

このガイドに従うには、IAB TCFと統合され、準拠している、商用または独自の同意管理プラットフォーム(CMP)を使用する必要があります。 詳しくは、[準拠したCMPのリスト](https://iabeurope.eu/cmp-list/)を参照してください。

>[!IMPORTANT]
>
>CMPのIDが無効な場合、プラットフォームはデータをそのまま処理し続けます。 TCF 2.0を強制するには、データをプラットフォームに送信する前に、IAB TCF 2.0に登録された有効なIDがCMPに含まれていることを確認する必要があります。

また、このガイドでは、次のプラットフォームサービスに関する十分な理解が必要です。

* [Experience Data Model(XDM)](../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [Adobe Experience Platform・アイデンティティ・サービス](../../../../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):を活用 [!DNL Identity Service] して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。[!DNL Real-time Customer Profile] data Lakeからデータを取り込み、顧客プロファイルを個別のデータストアに維持します。
* [Adobe Experience PlatformウェブSDK](../../../../edge/home.md):様々なプラットフォームサービスを顧客と直接やり取りするWebサイトに統合できる、クライアント側のJavaScriptライブラリ。
   * [SDKの同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す同意関連のSDKコマンドの使用例の概要です。
* [Adobe Experience Platform分類サービス](../../../../segmentation/home.md):同様の特性を共有し、マーケティング戦略と同様に対応する個人のグループに [!DNL Real-time Customer Profile] データを分割できます。

上記のプラットフォームサービスに加えて、[宛先](../../../../data-governance/home.md)や、プラットフォームエコシステムでのそれらの役割にも精通している必要があります。

## 顧客の同意フローの概要{#summary}

次の節では、システムが正しく設定された後に同意データを収集および適用する方法について説明します。

### 同意データ収集

プラットフォームでは、次のプロセスを通じて顧客の同意データを収集できます。

1. 顧客は、Webサイト上のダイアログを通じて、データ収集に関する同意の環境設定を行います。
1. CMPは、同意嗜好の変更を検出し、それに応じてTCF同意データを生成します。
1. プラットフォームWeb SDKを使用して、生成された同意データ（CMPによって返される）がAdobe Experience Platformに送信されます。
1. 収集した同意データは、[!DNL Profile]対応データセットに取り込まれ、そのスキーマにTCF同意フィールドが含まれています。

CMPの同意変更フックによってトリガーされるSDKコマンドに加えて、[!DNL Profile]対応のデータセットに直接アップロードされる、お客様が生成するXDMデータを介してExperience Platformに同意データを送ることもできます。

（[!DNL Audience Manager]ソースコネクタを介して）Adobe Audience Managerがプラットフォームと共有するセグメントには、[!DNL Experience Cloud Identity Service]を介して適切なフィールドが適用されていれば、同意データが含まれる場合もあります。 [!DNL Audience Manager]での同意データの収集について詳しくは、[IAB TCF](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)のAdobe Audience Managerプラグインのドキュメントを参照してください。

### 下流の同意の実施

TCFの同意データが正常に取り込まれると、ダウンストリームプラットフォームサービスでは以下の処理が行われます。

1. [!DNL Real-time Customer Profile] その顧客のプロファイルに対して保存されている同意データを更新します。
1. プラットフォームは、プラットフォーム(565)のベンダー権限がクラスター内のすべてのIDに対して指定されている場合にのみ、顧客IDを処理します。
1. TCF 2.0ベンダーリストのメンバーに属する宛先にセグメントをエクスポートする場合、プラットフォームには、個々の宛先のベンダー権限がクラスター内の各IDごとに提供される場合にのみ、プロファイルが含まれます。**

このドキュメントの残りのセクションでは、上記の収集および実施の要件を満たすためのプラットフォームおよびデータ操作の設定方法に関するガイダンスを提供します。

## CMP {#consent-data}内で顧客の同意データを生成する方法を決定する

各CMPシステムは独自のものなので、顧客がサービスとやり取りする際に、顧客が同意を与える最善の方法を決定する必要があります。 これを行う一般的な方法は、次の例のように、cookieの同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/iab/overview/cmp-dialog.png)

このダイアログでは、顧客が次の操作を行えるオプトインか、または行えないかを指定する必要があります。

| 同意オプション | 説明 |
| --- | --- |
| **目的** | 目的は、ブランドが顧客のデータを使用できる広告技術の目的を定義します。 プラットフォームで顧客IDを処理するには、次の目的を選択する必要があります。 <ul><li>**目的1**:デバイス上の情報の保存/アクセス</li><li>**目的10**:製品の開発と改善</li></ul> |
| **仕入先権限** | 広告技術の目的に加えて、このダイアログでは、Adobe Experience Platform(565)を含む特定のベンダーが顧客のデータを使用するか、使用しないかを許可する必要もあります。 |

### 同意文字列{#consent-strings}

データの収集方法に関係なく、目標は、顧客が選択した同意オプション（同意文字列）に基づいて文字列値を生成することです。

TCF仕様では、ポリシーおよびベンダーの定義に従って、顧客の同意設定に関する関連する詳細を特定のマーケティング目的の観点からエンコードする際に、同意文字列を使用します。 プラットフォームは、これらの文字列を使用して各顧客の同意設定を保存します。したがって、これらの設定が変更されるたびに新しい同意文字列を生成する必要があります。

同意文字列は、IAB TCFに登録されたCMPによってのみ作成できます。 特定のCMPを使用して同意文字列を生成する方法の詳細については、IAB TCF GitHubリポジトリの[同意文字列書式化ガイド](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20Consent%20string%20and%20vendor%20list%20formats%20v2.md)を参照してください。

## TCF同意フィールド{#datasets}を使用してデータセットを作成する

スキーマにTCFの同意フィールドが含まれるデータセットには、顧客の同意データを送信する必要があります。 このガイドを続ける前に、必要な2つのデータセットを作成する方法については、[TCF 2.0同意](./dataset.md)を取り込むためのデータセットの作成に関するチュートリアルを参照してください。

## [!DNL Profile]結合ポリシーを更新して、同意データを含める{#merge-policies}

同意データ収集用の[!DNL Profile]対応のデータセットを作成したら、結合ポリシーが顧客プロファイルに常にTCF同意フィールドを含むように設定されていることを確認する必要があります。 これには、競合する可能性のある他のデータセットよりも同意データセットの優先順位を優先するように、データセットの優先順位を設定する必要があります。

結合ポリシーの使用方法の詳細については、[merge policies user guide](../../../../profile/ui/merge-policies.md)を参照してください。 マージポリシーを設定する場合は、データセットの準備に関するガイドで概要を説明しているように、[XDMプライバシーミックスイン](./dataset.md#privacy-mixin)が提供する必要な同意属性がすべてセグメントに含まれていることを確認する必要があります。

## Experience PlatformWeb SDKを統合して、顧客の同意データを収集{#sdk}

>[!NOTE]
>
>Adobe Experience Platformで同意データを直接処理するには、Experience PlatformWeb SDKを使用する必要があります。 [!DNL Experience Cloud Identity Service] は現在サポートされていません。
>
>[!DNL Experience Cloud Identity Service] は、Adobe Audience Managerでの同意処理に対して引き続きサポートされますが、TCF 2.0への準拠には、ライブラリを [バージョン5.0に更新する必要があり](https://github.com/Adobe-Marketing-Cloud/id-service/releases)ます。

同意文字列を生成するようにCMPを設定したら、Experience PlatformWeb SDKを統合してこれらの文字列を収集し、Platformに送信する必要があります。 プラットフォームSDKは、TCFの同意データをプラットフォームに送信するために使用できる2つのコマンドを提供します（以下のサブセクションで説明）。顧客が初めて同意情報を提供した場合、その後同意が変更された場合に使用します。

**SDKは、CMPとのインターフェイスを初期状態で行いません**。SDKをWebサイトに統合する方法を決定し、CMPでの同意の変更をリッスンして、適切なコマンドを呼び出す必要があります。

### 新しいエッジ設定を作成する

SDKがExperience Platformにデータを送信するには、まず[!DNL Adobe Experience Platform Launch]にPlatform用の新しいエッジ設定を作成する必要があります。 新しい設定を作成する方法に関する具体的な手順は、[SDKドキュメント](../../../../edge/fundamentals/edge-configuration.md)に記載されています。

設定に一意の名前を指定した後、**[!UICONTROL Adobe Experience Platform]**&#x200B;の横のトグルボタンを選択します。 次に、次の値を使用してフォームの残りの部分を完成させます。

| エッジ設定フィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | エッジ設定を設定するために必要なストリーミング接続とデータセットが含まれている、プラットフォーム[サンドボックス](../../../../sandboxes/home.md)の名前です。 |
| [!UICONTROL ストリーミングインレット] | Experience Platform用の有効なストリーミング接続。 既存のストリーミングインレットがない場合は、[ストリーミング接続](../../../../ingestion/tutorials/create-streaming-connection-ui.md)の作成のチュートリアルを参照してください。 |
| [!UICONTROL イベントデータセット] | [前の手順](#datasets)で作成した[!DNL XDM ExperienceEvent]データセットを選択します。 |
| [!UICONTROL プロファイルデータセット] | [前の手順](#datasets)で作成した[!DNL XDM Individual Profile]データセットを選択します。 |

![](../../../images/governance-privacy-security/consent/iab/overview/edge-config.png)

完了したら、画面の下部にある「**[!UICONTROL 保存]**」を選択し、追加のプロンプトに従って設定を完了します。

### 同意変更コマンドの作成

前の節で説明したエッジ設定を作成したら、SDKコマンドを使用して開始し、プラットフォームに同意データを送信できます。 以下の節では、各SDKコマンドが様々なシナリオでどのように使用できるかの例を示します。

>[!NOTE]
>
>すべてのプラットフォームSDKコマンドの共通の構文については、[executing commands](../../../../edge/fundamentals/executing-commands.md)のドキュメントを参照してください。

#### CMP同意変更フックの使用

多くのCMPは、同意変更イベントをリッスンする、すぐに使用できるフックを提供します。 これらのイベントが発生した場合は、`setConsent`コマンドを使用して、その顧客の同意データを更新できます。

`setConsent`コマンドには2つの引数が必要です。(1)コマンドタイプ（この場合は「setConsent」）と(2)`consent`配列を含むペイロードを示す文字列。以下に示すように、必須の同意フィールドを提供するオブジェクトを少なくとも1つ含める必要があります。

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
| `standard` | 使用される同意基準。 TCF 2.0の同意処理を行うには、この値を`IAB`に設定する必要があります。 |
| `version` | `standard`で示された同意基準のバージョン番号。 TCF 2.0の同意処理を行うには、この値を`2.0`に設定する必要があります。 |
| `value` | CMPによって生成された、Base-64エンコードされた同意文字列。 |
| `gdprApplies` | GDPRが現在ログインしている顧客に適用されるかどうかを示すBoolean値。 TCF 2.0をこのお客様に適用するには、`true`に設定する必要があります。 定義されていない場合、デフォルトは`true`です。 |

`setConsent`コマンドは、同意設定の変更を検出するCMPフックの一部として使用する必要があります。 次のJavaScriptは、OneTrustの`OnConsentChanged`フックに`setConsent`コマンドを使用する方法の例を示しています。

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

また、`sendEvent`コマンドを使用して、プラットフォームでトリガーされるすべてのイベントでTCF 2.0の同意データを収集することもできます。

>[!NOTE]
>
>このメソッドを使用するには、[!DNL Experience Event Privacy mixin]を[!DNL Profile]-enabled [!DNL XDM ExperienceEvent]スキーマに追加しておく必要があります。 この設定方法の手順については、データセット準備ガイドの[ExperienceEventスキーマの更新](./dataset.md#event-schema)に関する節を参照してください。

`sendEvent`コマンドは、Webサイト上の適切なイベントリスナーのコールバックとして使用する必要があります。 コマンドには2つの引数が必要です。(1)コマンドタイプ（この場合は`sendEvent`）、(2)必須の同意フィールドをJSONとして提供する`xdm`オブジェクトを含むペイロードを示す文字列。

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
| `consentStandard` | 使用される同意基準。 TCF 2.0の同意処理を行うには、この値を`IAB`に設定する必要があります。 |
| `consentStandardVersion` | `standard`で示された同意基準のバージョン番号。 TCF 2.0の同意処理を行うには、この値を`2.0`に設定する必要があります。 |
| `consentStringValue` | CMPによって生成された、Base-64エンコードされた同意文字列。 |
| `gdprApplies` | GDPRが現在ログインしている顧客に適用されるかどうかを示すBoolean値。 TCF 2.0をこのお客様に適用するには、`true`に設定する必要があります。 定義されていない場合、デフォルトは`true`です。 |

### SDKレスポンスの処理

すべての[!DNL Platform SDK]コマンドは、呼び出しが成功したか失敗したかを示す約束を返します。 その後、これらの応答を、確認メッセージを顧客に表示するなどの追加のロジックに使用できます。 特定の例については、SDKコマンドの実行に関するガイドの[成功または失敗の処理](../../../../edge/fundamentals/executing-commands.md#handling-success-or-failure)に関する節を参照してください。

## セグメントのエクスポート{#export}

>[!NOTE]
>
>セグメントの書き出しを開始する前に、セグメントにすべての必須の同意フィールドが含まれていることを確認する必要があります。 詳しくは、[マージポリシーの設定](#merge-policies)の節を参照してください。

顧客の同意データを収集し、必要な同意属性を含むオーディエンスセグメントを作成したら、それらのセグメントをダウンストリームの宛先にエクスポートする際に、TCF 2.0準拠を強制できます。

一連の顧客プロファイルに対する同意設定`gdprApplies`を`true`に設定した場合、各プロファイルのTCF同意嗜好に基づいて、それらのプロファイルから下流の宛先にエクスポートされたデータがフィルタリングされる。 必要な同意の環境設定を満たさないプロファイルは、エクスポート処理中にスキップされます。

プロファイルを宛先にエクスポートするセグメントに含めるには、次の目的（[TCF 2.0ポリシー](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#Appendix_A_Purposes_and_Features_Definitions)で説明されているとおり）に同意する必要があります。

* **目的1**:デバイス上の情報の保存/アクセス
* **目的10**:製品の開発と改善

TCF 2.0では、データの送信元は、送信先のベンダー権限を確認してから、送信先にデータを送信する必要もあります。 そのため、プラットフォームは、宛先に連結されたデータを含める前に、宛先のベンダー権限がクラスター内のすべてのIDにオプトインされているかどうかを確認します。

>[!NOTE]
>
>Adobe Audience Managerと共有するセグメントには、対応するプラットフォームと同じTCF 2.0同意値が含まれます。 [!DNL Audience Manager]はプラットフォーム(565)と同じベンダーIDを共有するので、同じ目的とベンダーの権限が必要です。 詳しくは、[IAB TCF](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)のAdobe Audience Managerプラグインのドキュメントを参照してください。

## 導入をテスト{#test-implementation}

TCF 2.0実装を設定し、セグメントを宛先にエクスポートした後は、同意の要件を満たさないデータはエクスポートされません。 ただし、適切な顧客プロファイルがエクスポート中にフィルターされたかどうかを確認するには、宛先のデータストアを手動で確認し、同意が適切に適用されたかどうかを確認する必要があります。

複数のIDがクラスターを構成し、TCF 2.0が適用される場合、1つのIDに正しい目的とベンダー権限が含まれていなくても、クラスター全体が除外されることに注意してください。

## 次の手順

このドキュメントでは、TCF 2.0で説明されているビジネス上の義務を満たすようにプラットフォームデータ操作を設定するプロセスについて説明します。プラットフォームのプライバシー関連機能の詳細については、[ガバナンス、プライバシー、セキュリティ](../../overview.md)の概要を参照してください。