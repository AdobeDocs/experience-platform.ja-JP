---
description: Destination SDK で作成された宛先に対するパートナースキーマの設定方法を説明します。
title: パートナースキーマ設定
source-git-commit: 20dc7b31f75e88badac17faa542e046598632690
workflow-type: tm+mt
source-wordcount: '1892'
ht-degree: 90%

---


# パートナースキーマ設定

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。データが Platform に取り込まれると、XDM スキーマに応じて構造化されます。デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](../../../../xdm/schema/composition.md)を参照してください。

Destination SDK で宛先を作成する場合、宛先プラットフォームによって使用される独自のパートナースキーマを定義できます。これにより、ユーザーに、Platform から宛先プラットフォームが認識する特定のフィールドにプロファイル属性をマッピングする機能を提供します。これらは、すべて Platform UI 内で実行できます。

宛先用にパートナースキーマを設定する場合、宛先プラットフォームでサポートされているフィールドマッピングを微調整できます。以下に例を示します。

* ユーザーは、`phoneNumber` XDM 属性を宛先プラットフォームでサポートされている `phone` 属性にマッピングできます。
* 宛先内のサポートされるすべての属性のリストを取得するために Experience Platform が動的に呼び出す、動的パートナースキーマを作成します。
* 宛先プラットフォームが必要とする必須のフィールドマッピングを定義します。

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、[Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration)方法に関するガイドを参照してください。

`/authoring/destinations` エンドポイントを介してスキーマ設定を設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべてのスキーマ設定オプションを説明し、Platform UI で顧客に何が表示されるかを示します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○ |

## サポートされるスキーマ設定 {#supported-schema-types}

Destination SDK は、以下の複数のスキーマ設定をサポートします。

* 静的スキーマは、`schemaConfig` セクションの `profileFields` 配列を通じて定義されます。静的スキーマでは、`profileFields` 配列で Experience Platform UI に表示される必要がある各ターゲット属性を定義します。スキーマを更新する必要がある場合、[宛先設定を更新](../../authoring-api/destination-configuration/update-destination-configuration.md)する必要があります。
* 動的スキーマは、[動的スキーマサーバー](../../authoring-api/destination-server/create-destination-server.md)と呼ばれる、追加の宛先サーバータイプを使用して、独自の API に基づいて動的にスキーマを生成します。動的スキーマは、`profileFields` 配列を使用しません。スキーマを更新する必要がある場合、[宛先設定を更新](../../authoring-api/destination-configuration/update-destination-configuration.md)する必要はありません。代わりに、動的スキーマサーバーは、更新されたスキーマを API から取得します。
* スキーマ設定内では、必須の（または事前定義済みの）マッピングを追加するオプションがあります。これらは、ユーザーが Platform UI で表示できるマッピングですが、宛先への接続を設定する際には変更できません。例えば、常に宛先に送信されるように、メールアドレスフィールドを強制できます。

`schemaConfig` セクションは、以下の節で示すように、必要とするスキーマのタイプに応じて、複数の設定パラメーターを使用できます。

## 静的スキーマの作成 {#attributes-schema}

プロファイル属性を使用して静的スキーマを作成するには、以下に示すように、`profileFields` 配列でターゲット属性を定義します。

```json
"schemaConfig":{
      "profileFields":[
           {
              "name":"phoneNo",
              "title":"phoneNo",
              "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the mobilePhone.number value in Experience Platform could be phoneNo on your side.",
              "type":"string",
              "isRequired":false,
              "readOnly":false,
              "hidden":false
           },
                      {
              "name":"firstName",
              "title":"firstName",
              "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the person.name.firstName value in Experience Platform could be firstName on your side.",
              "type":"string",
              "isRequired":false,
              "readOnly":false,
              "hidden":false
           },
                      {
              "name":"lastName",
              "title":"lastName",
              "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the person.name.lastName value in Experience Platform could be phoneNo on your side.",
              "type":"string",
              "isRequired":false,
              "readOnly":false,
              "hidden":false
           }
        ],
      "useCustomerSchemaForAttributeMapping":false,
      "profileRequired":true,
      "segmentRequired":true,
      "identityRequired":true,
      "segmentNamespaceAllowList": ["someNamespace"],
      "segmentNamespaceDenyList": ["someOtherNamespace"]

}
```

| パラメーター | タイプ | 必須／オプション | 説明 |
|---------|----------|------|---|
| `profileFields` | 配列 | オプション | 顧客がプロファイル属性をマッピングする宛先プラットフォームによって受け入れられるターゲット属性の配列を定義します。`profileFields` 配列を使用する場合、`useCustomerSchemaForAttributeMapping` パラメーター全体を省略できます。 |
| `useCustomerSchemaForAttributeMapping` | ブール値 | オプション | 顧客スキーマから `profileFields` 配列で定義する属性への属性のマッピングを有効または無効にします。 <ul><li>`true` に設定すると、ユーザーには、マッピングフィールドのソース列のみが表示されます。この場合、`profileFields` は適用されません。</li><li>`false` に設定すると、ユーザーは、ユーザーのスキーマから `profileFields` 配列で定義した属性にソース属性をマッピングできます。</li></ul> デフォルト値は `false` です。 |
| `profileRequired` | ブール値 | オプション | ユーザーが Experience Platform から宛先プラットフォームのカスタム属性にプロファイル属性をマッピングできる必要がある場合は、`true` を使用します。 |
| `segmentRequired` | ブール値 | 必須 | このパラメーターは、Destination SDK に必須で、常に `true` に設定される必要があります。 |
| `identityRequired` | ブール値 | 必須 | ユーザーが Experience Platform から `profileFields` 配列で定義した属性に [ID タイプ](identity-namespace-configuration.md)をマッピングできる必要がある場合は、`true` に設定します。 |
| `segmentNamespaceAllowList` | 配列 | オプション | ユーザーがオーディエンスを宛先にマッピングできる特定のオーディエンス名前空間を定義します。 このパラメーターを使用して、配列で定義したオーディエンス名前空間からのみオーディエンスをエクスポートするよう Platform ユーザーを制限します。 このパラメーターは、と一緒に使用することはできません `segmentNamespaceDenyList`.<br> <br> 例： `"segmentNamespaceAllowList": ["AudienceManager"]` は、ユーザーが `AudienceManager` 名前空間をこの宛先に追加します。 <br> <br> ユーザーが任意のオーディエンスを宛先に書き出すことを許可するには、このパラメーターを無視します。 <br> <br> 両方の `segmentNamespaceAllowList` および `segmentNamespaceDenyList` が設定にない場合、ユーザーは、 [セグメント化サービス](../../../../segmentation/home.md). |
| `segmentNamespaceDenyList` | 配列 | オプション | 配列で定義されたオーディエンス名前空間からユーザーがオーディエンスを宛先にマッピングできないように制限します。 と一緒に使用することはできません `segmentNamespaceAllowed`. <br> <br> 例： `"segmentNamespaceDenyList": ["AudienceManager"]` は、ユーザーが `AudienceManager` 名前空間をこの宛先に追加します。 <br> <br> ユーザーが任意のオーディエンスを宛先に書き出すことを許可するには、このパラメーターを無視します。 <br> <br> 両方の `segmentNamespaceAllowed` および `segmentNamespaceDenyList` が設定にない場合、ユーザーは、 [セグメント化サービス](../../../../segmentation/home.md). <br> <br> 接触チャネルに関係なく、すべてのオーディエンスのエクスポートを許可するには、 `"segmentNamespaceDenyList":[]`. |

{style="table-layout:auto"}

以下の画像に、結果の UI エクスペリエンスを示します。

ユーザーがターゲットマッピングを選択する場合、`profileFields` 配列で定義されたフィールドを確認できます。

![ターゲット属性画面を示す UI 画像。](../../assets/functionality/destination-configuration/select-attributes.png)

属性を選択すると、ターゲットフィールド列で確認できます。

![属性を含む静的ターゲットスキーマを示す UI 画像](../../assets/functionality/destination-configuration/static-schema-attributes.png)

## 動的スキーマの作成 {#dynamic-schema-configuration}

Destination SDK は、動的パートナースキーマの作成をサポートしています。静的スキーマとは対照的に、動的スキーマは、`profileFields` 配列を使用しません。代わりに、動的スキーマは、スキーマ設定を取得する独自の API に接続する、動的スキーマサーバーを使用します。

>[!IMPORTANT]
>
>動的スキーマを作成する前に、[動的スキーマサーバーを作成](../../authoring-api/destination-server/create-destination-server.md)する必要があります。

動的スキーマ設定では、以下に示すように、`profileFields` 配列が `dynamicSchemaConfig` セクションによって置き換えられます。

```json
"schemaConfig":{
   "dynamicSchemaConfig":{
      "dynamicEnum": {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"DYNAMIC_SCHEMA_SERVER_ID",
         "value": "Schema Name",
         "responseFormat": "SCHEMA"
      }
   },
   "profileRequired":true,
   "segmentRequired":true,
   "identityRequired":true
}
```

| パラメーター | タイプ | 必須／オプション | 説明 |
|---------|----------|------|---|
| `dynamicEnum.authenticationRule` | 文字列 | 必須 | [!DNL Platform] の顧客が宛先に接続する方法を示します。使用できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`、<br> です。 <ul><li>Platform の顧客が[こちら](customer-authentication.md)で説明しているいずれかの認証方法でお使いのシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。 </li><li> アドビと宛先との間にグローバル認証システムがあり、[!DNL Platform] の顧客が宛先への接続に認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION` を使用します。この場合、資格情報 API を使用して、[資格情報オブジェクトを作成](../../credentials-api/create-credential-configuration.md)する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `dynamicEnum.destinationServerId` | 文字列 | 必須 | 動的スキーマサーバーの `instanceId`。この宛先サーバーには、動的スキーマを取得するために Experience Platform が呼び出す API エンドポイントが含まれます。 |
| `dynamicEnum.value` | 文字列 | 必須 | 動的スキーマサーバー設定で定義された、動的スキーマの名前。 |
| `dynamicEnum.responseFormat` | 文字列 | 必須 | 動的スキーマを定義する際は、常に `SCHEMA` に設定します。 |
| `profileRequired` | ブール値 | オプション | ユーザーが Experience Platform から宛先プラットフォームのカスタム属性にプロファイル属性をマッピングできる必要がある場合は、`true` を使用します。 |
| `segmentRequired` | ブール値 | 必須 | このパラメーターは、Destination SDK に必須で、常に `true` に設定される必要があります。 |
| `identityRequired` | ブール値 | 必須 | ユーザーが Experience Platform から `profileFields` 配列で定義した属性に [ID タイプ](identity-namespace-configuration.md)をマッピングできる必要がある場合は、`true` に設定します。 |

{style="table-layout:auto"}

## 必須のマッピング {#required-mappings}

スキーマ設定内では、静的または動的スキーマに加えて、必須の（または事前定義済みの）マッピングを追加するオプションがあります。これらは、ユーザーが Platform UI で表示できるマッピングですが、宛先への接続を設定する際には変更できません。

例えば、常に宛先に送信されるように、メールアドレスフィールドを強制できます。

>[!NOTE]
>
>現在、必須のマッピングの以下の組み合わせがサポートされています。
>* 必須のソースフィールドと必須の宛先フィールドを設定できます。この場合、ユーザーは、2 つのうちどちらかのフィールドを編集または選択できず、選択した方のみを表示できます。
>* 必須の宛先フィールドのみを設定できます。この場合、ユーザーは、宛先にマッピングするためのソースフィールドを選択できます。
>
> 必須のソースフィールドのみの設定は、現在、サポート&#x200B;*されていません*。

必須のマッピングを含むスキーマ設定と、これらが[バッチ宛先に対するデータの有効化ワークフロー](../../../ui/activate-batch-profile-destinations.md)のマッピング手順でどのように見えるかについて、以下の 2 つ例を参照してください。


>[!BEGINTABS]

>[!TAB 必須のソースおよび宛先マッピング]

以下に、必須のソースと宛先の両方のマッピングの例を示します。ソースと宛先の両方のフィールドが必須のマッピングとして指定されている場合、ユーザーは、2 つのうちどちらかのフィールドを選択または編集できず、事前定義済みの選択のみを表示できます。

```json
"schemaConfig": {
    "requiredMappingsOnly": true,
    "requiredMappings": [
      {
        "sourceType": "text/x.schema-path",
        "source": "personalEmail.address",
        "destination": "personalEmail.address"
      }
    ] 
}
```

| パラメーター | タイプ | 必須／オプション | 説明 |
|---|---|---|---|
| `requiredMappingsOnly` | ブール値 | オプション | これが true に設定されている場合、ユーザーは、`requiredMappings` 配列で定義する必須のマッピング以外に、アクティベーションフローで他の属性および ID をマッピングできません。 |
| `requiredMappings.sourceType` | 文字列 | 必須 | `source` フィールドのタイプを示します。サポートされている値： <ul><li>`text/x.schema-path`：`source` フィールドが XDM スキーマからのプロファイル属性の場合に、この値を使用します。</li><li>`text/x.aep-xl`：`source` フィールドが正規表現で定義されている場合に、この値を使用します。例：`iif(segmentMembership.ups.aep_seg_id.status==\"exited\", \"1\", \"0\")`</li><li>`text/plain`：`source` フィールドがマクロテンプレートで定義されている場合に、この値を使用します。現在、サポートされている唯一のマクロテンプレートは、`metadata.segment.alias` です。</li></ul> |
| `requiredMappings.source` | 文字列 | 必須 | ソースフィールドの値を示します。サポートされる値タイプを以下に示します。 <ul><li>XDM プロファイル属性。例：`personalEmail.address`。ソース属性が XDM プロファイル属性の場合は、`sourceType` パラメーターを `text/x.schema-path` に設定します。</li><li>正規表現。例：`iif(segmentMembership.ups.aep_seg_id.status==\"exited\", \"1\", \"0\")`。ソース属性が正規表現の場合は、`sourceType` パラメーターを `text/x.aep-xl` に設定します。</li><li>マクロテンプレート。例：`metadata.segment.alias`。ソース属性がマクロテンプレートの場合は、`sourceType` パラメーターを `text/plain` に設定します。現在、サポートされている唯一のマクロテンプレートは、`metadata.segment.alias` です。</li></ul> |
| `requiredMappings.destination` | 文字列 | 必須 | ターゲットフィールドの値を示します。ソースと宛先の両方のフィールドが必須のマッピングとして指定されている場合、ユーザーは、2 つのうちどちらかのフィールドを選択または編集できず、選択した方のみを表示できます。 |

{style="table-layout:auto"}

その結果、Platform UI の&#x200B;**[!UICONTROL ソースフィールド]**&#x200B;と&#x200B;**[!UICONTROL ターゲットフィールド]**&#x200B;の両方のセクションが灰色表示になります。

![UI アクティベーションフローでの必須のマッピングの画像。](../../assets/functionality/destination-configuration/required-mappings-2.png)

>[!TAB 必須の宛先マッピング]

以下に、必須の宛先マッピングの例を示します。宛先フィールドのみが必須として指定されている場合、ユーザーは、マッピングするソースフィールドを選択できます。

```json
"schemaConfig": {
    "requiredMappingsOnly": true,
    "requiredMappings": [
      {
        "destination": "identityMap.ExamplePartner_ID",
        "mandatoryRequired": true,
        "primaryKeyRequired": true
      }
    ] 
}
```

| パラメーター | タイプ | 必須／オプション | 説明 |
|---|---|---|---|
| `requiredMappingsOnly` | ブール値 | オプション | これが true に設定されている場合、ユーザーは、`requiredMappings` 配列で定義する必須のマッピング以外に、アクティベーションフローで他の属性および ID をマッピングできません。 |
| `requiredMappings.destination` | 文字列 | 必須 | ターゲットフィールドの値を示します。宛先フィールドのみが指定されている場合、ユーザーは、宛先にマッピングするソースフィールドを選択できます。 |
| `mandatoryRequired` | ブール値 | オプション | マッピングが[必須の属性](../../../ui/activate-batch-profile-destinations.md#mandatory-attributes)としてマークされる必要があるかどうかを示します。 |
| `primaryKeyRequired` | ブール値 | オプション | マッピングが[重複排除キー](../../../ui/activate-batch-profile-destinations.md#deduplication-keys)としてマークされる必要があるかどうかを示します 。 |

{style="table-layout:auto"}

その結果、Platform UI の&#x200B;**[!UICONTROL ターゲットフィールド]**&#x200B;セクションが灰色表示になるのに対して、**[!UICONTROL ソースフィールド]**&#x200B;セクションはアクティブになり、ユーザーが操作できます。**[!UICONTROL 必須キー]**&#x200B;および&#x200B;**[!UICONTROL 重複排除キー]**&#x200B;オプションがアクティブになり、ユーザーは変更できません。

![UI アクティベーションフローでの必須のマッピングの画像。](../../assets/functionality/destination-configuration/required-mappings-1.png)

>[!ENDTABS]

## 次の手順 {#next-steps}

この記事を読むことで、Destination SDK でサポートされるスキーマタイプと、どのようにスキーマを設定できるかについて、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [UI 属性](ui-attributes.md)
* [顧客データフィールド](customer-data-fields.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)