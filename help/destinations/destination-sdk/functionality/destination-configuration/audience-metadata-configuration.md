---
description: Destination SDK で作成された宛先に対するオーディエンスメタデータ設定の設定方法を説明します。
title: オーディエンスメタデータ設定
exl-id: ae71df4f-b753-4084-835f-03559b4986cb
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 91%

---

# オーディエンスメタデータ設定

Experience Platform から宛先にデータを書き出す場合、特定のオーディエンスメタデータ（オーディエンス名やオーディエンス ID など）を Experience Platform と宛先の間で共有する必要がある可能性があります。

Destination SDK には、宛先プラットフォームのオーディエンスをプログラムで作成、更新または削除するのに使用できるツールが用意されています。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したストリーミング先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration).

`/authoring/audience-templates` エンドポイントを介してオーディエンスメタデータテンプレートを設定できます。オーディエンスメタデータ設定を作成したら、`/authoring/destinations` エンドポイントを使用して、`audienceMetadataConfig` セクションを設定できます。このセクションは、オーディエンステンプレートにマッピングする必要があるオーディエンスメタデータを宛先に指示します。

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

オーディエンスメタデータ設定を作成する際に、 以下の表で説明されているパラメーターを使用して、オーディエンスマッピング設定を設定できます。

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
| `mapExperiencePlatformSegmentName` | ブール値 | 宛先アクティベーションワークフローの[[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 値が Experience Platform オーディエンス名である必要があるかどうかを示します。 |
| `mapExperiencePlatformSegmentId` | ブール値 | 宛先アクティベーションワークフローの[[!UICONTROL マッピング ID]](../../../ui/activate-segment-streaming-destinations.md#scheduling) 値が Experience Platform オーディエンス ID である必要があるかどうかを示します。 |
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
