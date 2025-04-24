---
title: リアルタイムでのエッジプロファイル属性の検索
description: カスタム Personalizationの宛先とEdge Network API を使用して、エッジプロファイル属性をリアルタイムで検索する方法を説明します
type: Tutorial
exl-id: e185d741-af30-4706-bc8f-d880204d9ec7
source-git-commit: 7f3459f678c74ead1d733304702309522dd0018b
workflow-type: tm+mt
source-wordcount: '1911'
ht-degree: 7%

---

# エッジ上でのプロファイル属性のリアルタイム検索

Adobe Experience Platformは、すべてのプロファイルデータの唯一の情報源として [ リアルタイム顧客プロファイル ](../../profile/home.md) を使用します。 リアルタイムのデータ取得を迅速に行うために、[ エッジプロファイル ](../../profile/edge-profiles.md) を使用します。これは、[Edge Network](../../collection/home.md#edge) 全体に配布される軽量のプロファイルです。 これにより、迅速でリアルタイムのパーソナライゼーションのユースケースが可能になります。

## ユースケース {#use-cases}

次に、エッジプロファイルのルックアップが役立つ 2 つのユースケースを示します。

* **リアルタイムPersonalization**：エッジプロファイルからプロファイル情報をすばやく取得して、web サイト上のユーザーエクスペリエンスをパーソナライズします。
* **カスタマーサポート**：お客様がサポートセンターエージェントに電話すると、プロファイル情報をリアルタイムで取得します。

このページでは、ダウンストリームアプリケーションを使用してパーソナライゼーションエクスペリエンスを配信したり、意思決定ルールを通知したりするために、エッジプロファイルデータをリアルタイムで検索するために従う必要がある手順について説明します。

## 用語と前提条件 {#prerequisites}

このページで説明するユースケースを設定する場合、次のExperience Platform コンポーネントを使用します。

* [ データストリーム ](../../datastreams/overview.md)：データストリームは、web SDKから受信したイベントデータを受け取り、エッジプロファイルデータで応答します。
* [ 結合ポリシー ](../../segmentation/ui/segment-builder.md#merge-policies): [!UICONTROL Edgeでアクティブ ] 結合ポリシーを作成して、エッジプロファイルが正しいプロファイルデータを使用していることを確認します。
* [ カスタム Personalization接続 ](../catalog/personalization/custom-personalization.md)：プロファイル属性をEdge Networkに送信する新しいカスタムパーソナライゼーション接続を設定します。
* [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/): Edge Network API [ インタラクティブデータ収集 ](https://developer.adobe.com/data-collection-apis/docs/endpoints/interact/) 機能を使用して、エッジプロファイルからプロファイル属性をすばやく取得します。

## パフォーマンスガードレール {#guardrails}

Edge プロファイルのルックアップのユースケースは、次の表に示す特定のパフォーマンスガードレールの影響を受けます。 Edge Network API ガードレールについて詳しくは、ガードレール [ ドキュメントページ ](https://developer.adobe.com/data-collection-apis/docs/getting-started/guardrails/) を参照してください。

| Edge Network サービス | Edgeセグメント化 | 1 秒あたりの要求数 |
|---------|----------|---------|
| [2}Edge Network API を介した ](../catalog/personalization/custom-personalization.md) カスタムパーソナライゼーションの宛先 ](https://developer.adobe.com/data-collection-apis/docs/api/)[ | ○ | 1500 |
| [2}Edge Network API を介した ](../catalog/personalization/custom-personalization.md) カスタムパーソナライゼーションの宛先 ](https://developer.adobe.com/data-collection-apis/docs/api/)[ | × | 1500 |

## 手順 1：データストリームの作成と設定 {#create-datastream}

[ データストリーム設定 ](../../datastreams/configure.md#create-a-datastream) ドキュメントの手順に従って、次の **[!UICONTROL サービス]** 設定で新しいデータストリームを作成します。

* **[!UICONTROL サービス]**: [!UICONTROL Adobe Experience Platform]
* **[!UICONTROL Personalizationの宛先]**：有効
* **[!UICONTROL Edgeのセグメント化]**: エッジのセグメント化が必要な場合は、このオプションを有効にします。 エッジ上でのプロファイル属性の検索のみが目的で、エッジプロファイルに基づいたセグメント化を実行しない場合は、このオプションを無効のままにします。


  <!-- >[!IMPORTANT]
    >
    >Enabling edge segmentation limits the maximum number of lookup requests to 1500 request per second. If you need a higher request throughput, disable edge segmentation for your datastream. See the [guardrails documentation](../guardrails.md#edge-destinations-activation) for detailed information. -->

  ![ データストリーム設定画面を示すExperience Platform UI 画像。](../assets/ui/activate-edge-profile-lookup/datastream-config.png)


## 手順 2:Edge 評価用のオーディエンスの設定 {#audience-edge-evaluation}

Edge でプロファイル属性を検索するには、オーディエンスを Edge 評価用に設定する必要があります。

アクティブ化するオーディエンスの [Edgeでアクティブ化結合ポリシー ](../../segmentation/ui/segment-builder.md#merge-policies) がデフォルトとして設定されていることを確認します。 [!DNL Active-On-Edge] 結合ポリシーを使用すると、オーディエンスが常に [ エッジ上で ](../../segmentation/methods/edge-segmentation.md) 評価され、リアルタイムパーソナライゼーションのユースケースで利用できるようになります。

[結合ポリシーの作成](../../profile/merge-policies/ui-guide.md#create-a-merge-policy)の手順に従い、「**[!UICONTROL エッジでアクティブ化結合ポリシー]**」切り替えスイッチを必ず有効にします。

>[!IMPORTANT]
>
>オーディエンスが異なる結合ポリシーを使用している場合、エッジからプロファイル属性を取得できず、エッジプロファイルの検索も実行できません。

## 手順 3:Edge Networkへのプロファイル属性データの送信{#configure-custom-personalization-connection}

属性やオーディエンスメンバーシップデータを含むエッジプロファイルをリアルタイムで検索するには、データをEdge Networkで使用できるようにする必要があります。 この目的のために、**[!UICONTROL 属性を含むカスタム Personalization]** 宛先への接続を作成し、エッジプロファイルで検索する属性を含むオーディエンスをアクティブ化する必要があります。

+++ カスタム Personalizationと属性の連携の設定

新しい宛先接続の作成方法に関する詳細な手順については、[宛先接続の作成チュートリアル](../ui/connect-destination.md)に従ってください。

新しい宛先を設定する際に、[ 手順 1](#create-datastream) で作成したデータストリームを「**[!UICONTROL データストリーム ID]**」フィールドで選択します。 **[!UICONTROL 統合エイリアス]** の場合は、今後この宛先接続を識別するのに役立つ任意の値（宛先名など）を使用できます。

![ 属性を含むカスタム Personalization設定画面を示すExperience Platform UI 画像。](../assets/ui/activate-edge-profile-lookup/destination-config.png)

+++

+++属性連携によるカスタム Personalizationに対するオーディエンスのアクティブ化

**[!UICONTROL 属性を含むカスタム Personalization]** 接続を作成したら、プロファイルデータをEdge Networkに送信する準備が整います。

>[!IMPORTANT]
> 
> * データをアクティブ化し、ワークフローの [ マッピングステップ ](#mapping) を有効にするには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。
> 
> [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![Experience Platform UI でハイライト表示された「宛先カタログ」タブ ](../assets/ui/activate-edge-personalization-destinations/catalog-tab.png)

1. **[!UICONTROL 属性を含むカスタム Personalization]** 宛先カードを見つけ、「**[!UICONTROL オーディエンスをアクティブ化]**」を選択します（下図を参照）。

   ![ カタログの宛先カードでハイライト表示されたオーディエンスコントロールをアクティブ化 ](../assets/ui/activate-edge-personalization-destinations/activate-audiences-button.png)

1. 以前に設定した宛先接続を選択し、「**[!UICONTROL 次へ]**」を選択します。

   ![ アクティベーションワークフローの宛先手順を選択します。](../assets/ui/activate-edge-personalization-destinations/select-destination.png)

1. オーディエンスを選択します。 オーディエンス名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするオーディエンスを選択し、「**[!UICONTROL 次へ]**」を選択します。

   接触チャネルに応じて、複数のタイプのオーディエンスから選択できます。

   * **[!UICONTROL セグメント化サービス]**：セグメント化サービスによってExperience Platform内で生成されたオーディエンス。 詳しくは、[ セグメント化ドキュメント ](../../segmentation/ui/overview.md) を参照してください。
   * **[!UICONTROL カスタムアップロード]**:Experience Platform以外で生成され、CSV ファイルとしてExperience Platformにアップロードされたオーディエンス。 外部オーディエンスについて詳しくは、[ オーディエンスの読み込み ](../../segmentation/ui/overview.md#import-audience) に関するドキュメントを参照してください。
   * その他のタイプのオーディエンス。他のAdobe ソリューション（[!DNL Audience Manager] など）から派生します。

     ![ 複数のオーディエンスがハイライト表示されたアクティベーションワークフローのオーディエンス選択手順。](../assets/ui/activate-edge-personalization-destinations/select-audiences.png)

1. エッジプロファイルで使用できるようにするプロファイル属性を選択します。

   * **ソース属性を選択** します。 ソース属性を追加するには、以下に示すように、**[!UICONTROL Source フィールド]** 列で「**[!UICONTROL 新しいフィールドを追加]**」コントロールを選択し、目的の XDM 属性フィールドを検索するか、移動します。

     ![ マッピングステップでターゲット属性を選択する方法を示す画面録画。](../assets/ui/activate-edge-personalization-destinations/mapping-step-select-attribute.gif)

   * **ターゲット属性を選択**。 ターゲット属性を追加するには、**[!UICONTROL ターゲットフィールド]** 列の **[!UICONTROL 新しいフィールドを追加]** コントロールを選択し、ソース属性をマッピングするカスタム属性名を入力します。

     ![ マッピングステップで XDM 属性を選択する方法を示す画面録画 ](../assets/ui/activate-edge-personalization-destinations/mapping-step-select-target-attribute.gif)



プロファイル属性のマッピングが完了したら、「**[!UICONTROL 次へ]**」を選択します。

「**[!UICONTROL レビュー]**」ページには、選択内容の概要が表示されます。**[!UICONTROL キャンセル]** を選択してフローを中断するか、**[!UICONTROL 戻る]** を選択して設定を変更する、または **[!UICONTROL 完了]** を選択して選択内容を確定し、Edge Networkへのプロファイルデータの送信を開始します。

![ レビュー手順の選択の概要。](../assets/ui/activate-edge-personalization-destinations/review.png)

+++

+++同意方針評価

組織で **Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した場合、**[!UICONTROL 適用可能な同意ポリシーを表示]**&#x200B;を選択すると、どの同意ポリシーが適用され、その結果、いくつのプロファイルがアクティベーションに含まれるかを確認することができます。詳しくは、[ 同意ポリシーの評価 ](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を参照してください。

**データ使用ポリシーのチェック**

**[!UICONTROL レビュー]** 手順では、Experience Platformはデータ使用ポリシーの違反もチェックします。 ポリシーに違反した場合の例を次に示します。違反を解決するまで、Audience Activation ワークフローを完了することはできません。 ポリシー違反の解決方法については、データガバナンスに関するドキュメントの [ データ使用ポリシー違反 ](/help/data-governance/enforcement/auto-enforcement.md#data-usage-violation) を参照してください。

![ データポリシー違反の例 ](../assets/common/data-policy-violation.png)

+++

+++オーディエンスのフィルタリング

**[!UICONTROL レビュー]** ステップでは、ページで使用可能なフィルターを使用して、このワークフローの一環としてスケジュールまたはマッピングが更新されたオーディエンスのみを表示できます。 また、表示するテーブル列を切り替えることもできます。

![ レビューステップで使用可能なオーディエンスフィルターを示す画面録画。](../assets/ui/activate-edge-personalization-destinations/filter-audiences-review-step.gif)


選択内容に満足し、ポリシー違反が検出されていない場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確定します。

+++

## 手順 4：エッジ上でのプロファイル属性の検索 {#configure-edge-profile-lookup}

これで [ データストリームの設定 ](#create-datastream) が完了し、[ 属性の宛先接続を使用した新しいカスタム Personalizationが作成されました ](#configure-destination) と、この接続を使用して [ プロファイル属性を送信 ](#activate-audiences) し、Edge Networkを検索できるようになりました。

次の手順では、エッジプロファイルからプロファイル属性を取得するようにパーソナライゼーションソリューションを設定します。

>[!IMPORTANT]
>
>プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するには、[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/getting-started/) を介してプロファイル属性を取得する必要があります。 さらに、API 呼び出しを認証するには、Edge Network API[ インタラクティブデータ収集エンドポイント ](https://developer.adobe.com/data-collection-apis/docs/endpoints/interact/) を介してプロファイル属性を取得する必要があります。
><br>上記の要件に従わない場合、パーソナライゼーションはオーディエンスメンバーシップのみに基づき、プロファイル属性は使用できません。

[ 手順 1](#create-datastream) で設定したデータストリームは、受信イベントデータを受け入れ、エッジプロファイル情報で応答する準備が整いました。

以下の例に示すように、エッジプロファイル情報を取得するように統合を設定します。

### リクエスト {#request}

エッジプロファイルデータを取得するには、空の `POST` 呼び出しを `/interact` エンドポイントに送信します。これには、以下に示すように、イベントに含まれるプロファイル属性を検索するプライマリ ID を含めます。

```shell
curl -X POST "https://server.adobedc.net/ee/v2/interact?dataStreamId={DATASTREAM_ID}" 
-H "Authorization: Bearer {TOKEN}" 
-H "x-gw-ims-org-id: {ORG_ID}" 
-H "x-api-key: {API_KEY}" 
-H "Content-Type: application/json" 
-d '{
    "event":
    {
        "xdm": {
            "identityMap": {
                "Email": [
                    {  
                        "id":"test123@adobetest.com",
                        "primary":true
                    }
                ]
            }
        }
    }
    
}'
```

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `dataStreamId` | `String` | はい。 | [ 手順 1](#create-datastream) で作成したデータストリームのデータストリーム ID。 |

### 応答 {#response}

応答が成功すると、HTTP ステータス `200 OK` が、プロファイルがエッジで見つかったかどうかに応じて、以下のタブの例に類似した情報を含む `Handle` オブジェクトと共に返されます。

>[!NOTE]
>
>API 応答はモジュール型で、`handle` オブジェクトには様々なタイプの複数の `payload` オブジェクトを含めることができます。 エッジプロファイル参照に関連する情報は、`"type": "activation:pull"` の `payload` オブジェクトの下にグループ化されます。

>[!BEGINTABS]

>[!TAB  プロファイルがエッジに存在する ]

プロファイルがエッジに存在する場合は、エッジに対してアクティブ化されたプロファイル属性とオーディエンスによっては、以下のような属性とオーディエンスメンバーシップを含む応答が期待できます。

```json
{
  "requestId": "3c600138-d785-42ca-a025-bb725f4b5da9",
  "handle": [
    {
      "payload": [
        {
          "type": "profileLookup",
          "destinationId": "9218b727-ec59-4a46-b8b9-05503f138c5d",
          "alias": "rk-demo-custom-personalization-XXXX",
          "attributes": {
            "zip": {
              "value": "19000"
            },
            "firstName": {
              "value": "Test"
            },
            "lastName": {
              "value": "User123"
            },
            "gender": {
              "value": "male"
            },
            "city": {
              "value": "Philadelphia"
            },
            "state": {
              "value": "PA"
            },
            "email": {
              "value": "test123@adobetest.com"
            }
          },
          "segments": [
            {
              "id": "85018bd8-7ad1-4e17-ae30-8389c04bd3c0",
              "namespace": "ups"
            },
            {
              "id": "d09a8159-8b30-4178-b2f2-7a8c5e3168d9",
              "namespace": "ups"
            }
          ]
        }
      ],
      "type": "activation:pull",
      "eventIndex": 0
    }
  ]
}
```

`handle` オブジェクトは、次の表で説明される情報を提供します。

| パラメーター | 説明 |
|---------|----------|
| `payload` | エッジ参照情報を含む `payload` オブジェクト。 応答には、エッジルックアップとは無関係に、複数の追加の `payload` オブジェクトが含まれる場合があります。 |
| `type` | ペイロードは、タイプ別に応答でグループ化されます。 エッジプロファイル参照のペイロードタイプは、常に `profileLookup` に設定されます。 |
| `destinationId` | [ 手順 3](#configure-custom-personalization-connection) で作成した **[!UICONTROL カスタム Personalization]** 接続インスタンスの ID。 |
| `alias` | [ カスタム Personalization](../catalog/personalization/custom-personalization.md) 宛先接続を作成する際にユーザーが設定する、宛先接続のエイリアス。 |
| `attributes` | この配列には、[ 手順 3](#configure-custom-personalization-connection) でアクティブ化したオーディエンスのエッジプロファイル属性が含まれます。 |
| `segments` | この配列には、[ 手順 3](#configure-custom-personalization-connection) でアクティブ化したオーディエンスが含まれます。 |
| `type` | オブジェクト `handle` タイプ別にグループ化されます。 エッジプロファイル参照のユースケースでは、`handle` オブジェクトのタイプは常に `activation:pull` です。 |
| `eventIndex` | Edge Networkは、クライアントから配列の形式でイベントを受け取ります。 配列内のイベントの順序は、処理中に保持され、このインデックスによって反映されます。 イベントのインデックス作成は `0` で始まります。 |

>[!TAB  プロファイルがエッジに存在しません ]

プロファイルがエッジに存在しない場合は、次のような応答が予想されます。

```json
{
  "requestId": "531b541a-4541-419e-ac99-fd7e452f0c0f",
  "handle": [
    {
      "payload": [],
      "type": "activation:pull",
      "eventIndex": 0
    }
  ]
}
```

`handle` オブジェクトは、次の表で説明される情報を提供します。

| パラメーター | 説明 |
|---------|----------|
| `payload` | プロファイルがエッジに存在しない場合、`payload` オブジェクトは空です。 |
| `type` | オブジェクト `payload` タイプ別にグループ化されます。 エッジプロファイル参照のユースケースでは、`payload` オブジェクトのタイプは常に `activation:pull` です。 |
| `eventIndex` | Edge Networkは、配列の形式でクライアントからイベントを受け取ります。 配列内のイベントの順序は、処理中に保持され、このインデックスによって反映されます。 イベントのインデックス作成は `0` で始まります。 |

>[!ENDTABS]

>[!SUCCESS]
>
>統合を正しく設定すると、エッジプロファイルデータにアクセスできるようになり、エッジプロファイルの属性とオーディエンスメンバーシップを使用して、ダウンストリームのパーソナライゼーションエンジンでリアルタイムのパーソナライゼーションをトリガー設定できます。

## まとめ {#conclusion}

上記の手順に従うと、エッジプロファイル属性をリアルタイムで効率的に検索でき、ダウンストリームアプリケーションを通じてパーソナライズされたエクスペリエンスや十分な情報に基づいた意思決定が可能になります。
