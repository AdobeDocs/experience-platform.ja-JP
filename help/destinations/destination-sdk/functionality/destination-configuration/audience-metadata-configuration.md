---
description: Destination SDK で作成された宛先に対するオーディエンスメタデータ設定の設定方法を説明します。
title: オーディエンスメタデータ設定
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 74%

---


# オーディエンスメタデータ設定

Experience Platformから宛先にデータを書き出す場合、Experience Platformと宛先の間で共有するために、オーディエンス名やオーディエンス ID など、特定のオーディエンスメタデータが必要になる場合があります。

Destination SDK には、宛先プラットフォームのオーディエンスをプログラムで作成、更新または削除するのに使用できるツールが用意されています。

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、[Destination SDK を使用したストリーミング宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)方法に関するガイドを参照してください。

`/authoring/audience-templates` エンドポイントを介してオーディエンスメタデータテンプレートを設定できます。オーディエンスメタデータ設定を作成したら、`/authoring/destinations` エンドポイントを使用して、`audienceMetadataConfig` セクションを設定できます。このセクションは、オーディエンステンプレートにマッピングする必要があるオーディエンスメタデータを宛先に伝えます。

このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)
* [オーディエンステンプレートの作成](../../metadata-api/create-audience-template.md)
* [オーディエンステンプレートの更新](../../metadata-api/update-audience-template.md)

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○ |

## サポートされるパラメーター {#supported-parameters}

オーディエンスメタデータ設定を作成する際に、次の表で説明するパラメーターを使用して、オーディエンスマッピング設定を指定できます。

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
| `mapExperiencePlatformSegmentName` | ブール値 | を返します。 [[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 宛先のアクティベーションワークフローの値は、Experience Platformのオーディエンス名である必要があります。 |
| `mapExperiencePlatformSegmentId` | ブール値 | を返します。 [[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 宛先のアクティベーションワークフローの値は、Experience Platformオーディエンス ID である必要があります。 |
| `mapUserInput` | ブール値 | 宛先アクティベーションワークフローの[[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 値に対するユーザー入力を有効または無効にします。`true` に設定すると、`audienceTemplateId` は存在できません。 |
| `audienceTemplateId` | ブール値 | 宛先に使用される[オーディエンスメタデータテンプレート](../../metadata-api/create-audience-template.md)の `instanceId`。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むことで、宛先に対するオーディエンスメタデータの設定方法について、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証設定](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)