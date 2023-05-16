---
description: Destination SDKで作成した宛先に対して、オーディエンスのメタデータを設定する方法を説明します。
title: オーディエンスメタデータの設定
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 6%

---


# オーディエンスメタデータの設定

Experience Platformから宛先にデータを書き出す際、Experience Platformと宛先の間で共有するために、セグメント名やセグメント ID など、特定のオーディエンスメタデータが必要になる場合があります。

Destination SDKは、宛先プラットフォームでオーディエンスをプログラムによって作成、更新、削除するために使用できるツールを提供します。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したストリーミング先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration).

オーディエンスメタデータテンプレートは、 `/authoring/audience-templates` endpoint. オーディエンスのメタデータ設定を作成した後、 `/authoring/destinations` エンドポイント：設定 `audienceMetadataConfig` 」セクションに入力します。 このセクションは、オーディエンステンプレートにマッピングする必要があるセグメントメタデータを宛先に伝えます。

このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)
* [オーディエンステンプレートの作成](../../metadata-api/create-audience-template.md)
* [オーディエンステンプレートの更新](../../metadata-api/update-audience-template.md)

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | ○ |

## サポートされているパラメーター {#supported-parameters}

オーディエンスのメタデータ設定を作成する場合、次の表で説明するパラメーターを使用して、セグメントマッピング設定を指定できます。

```json
  "audienceMetadataConfig":{
   "mapExperiencePlatformSegmentName":false,
   "mapExperiencePlatformSegmentId":false,
   "mapUserInput":false,
   "audienceTemplateId":"YOUR_AUDIENCE_TEMPLATE_ID"
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `mapExperiencePlatformSegmentName` | ブール値 | を返します。 [[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 宛先のアクティベーションワークフローの値は、Experience Platformセグメント名である必要があります。 |
| `mapExperiencePlatformSegmentId` | ブール値 | を返します。 [[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 宛先のアクティベーションワークフローの値は、Experience Platformセグメント ID である必要があります。 |
| `mapUserInput` | ブール値 | のユーザー入力を有効または無効にします [[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) の値を指定します。 次に設定した場合： `true`, `audienceTemplateId` は存在しません。 |
| `audienceTemplateId` | ブール値 | この `instanceId` の [オーディエンスメタデータテンプレート](../../metadata-api/create-audience-template.md) を宛先に使用します。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むと、宛先に対するオーディエンスメタデータの設定方法をより深く理解できるようになります。

その他の宛先コンポーネントについて詳しくは、次の記事を参照してください。

* [顧客認証設定](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間の設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)