---
keywords: Experience Platform;home;popular topics;;XDM;XDM system;XDM individual profile;XDM ExperienceEvent;XDM Experience Event;experienceEvent;experience eventExperience event;XDM Experience Event;XDM ExperienceEvent;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema;troubleshooting;FAQ;faq;Union schema;UNION PROFILE;union profile
solution: Experience Platform
title: エクスペリエンスデータモデル（XDM）システムのトラブルシューティングガイド
description: このドキュメントでは、エクスペリエンスデータモデル（XDM）システムに関するよくある質問の回答に加えて、一般的なエラーのトラブルシューティングガイドを提供します。
topic: troubleshooting
translation-type: tm+mt
source-git-commit: e87fcd9f028bc6dedaec0435c4eef54e6aecae2d
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 67%

---


# [!DNL Experience Data Model] (XDM)システムトラブルシューティングガイド

This document provides answers to frequently asked questions about [!DNL Experience Data Model] (XDM) System, as well as a troubleshooting guide for common errors. Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

**[!DNL Experience Data Model](XDM)** は、顧客体験管理のための標準化されたスキーマを定義するオープンソース仕様です。 The methodology on which [!DNL Experience Platform] is built, **XDM System**, operationalizes [!DNL Experience Data Model] schemas for use by [!DNL Platform] services. に **[!DNL Schema Registry]** は、ユーザーインターフェイスと、内部にアクセスするRESTful APIが用意さ **[!DNL Schema Library]** れてい [!DNL Experience Platform]ます。 詳しくは、[XDM のドキュメント](home.md)を参照してください。

## FAQ

The following is a list of answers to frequently asked questions about XDM System and use of the [!DNL Schema Registry] API.

### フィールドをスキーマに追加するには、どうすればよいですか？

Mixin を使用して、スキーマにフィールドを追加できます。各 Mixin は 1 つ以上のクラスと互換性があり、それらの互換性のあるクラスの 1 つを実装する任意のスキーマで Mixin を使用できます。Adobe Experience Platform は、独自の定義済みフィールドを含む複数の業界用 Mixin を提供しますが、API またはユーザーインターフェイスを使用して新しい Mixin を作成することで、独自のフィールドをスキーマに追加できます。

APIでの新しいミックスインの作成について詳しくは、 [!DNL Schema Registry] mixinエンドポイントガイドを参照してください [](api/mixins.md#create)。 UI を使用する場合は、[スキーマエディターのチュートリアル](./tutorials/create-schema-ui.md)を参照してください。

### Mixin とデータ型の最適な用途は何ですか？

[Mixin](./schema/composition.md#mixin) は、スキーマ内の 1 つ以上のフィールドを定義するコンポーネントです。Mixin は、スキーマの階層でフィールドが表示される方法を強制するため、フィールドが含まれるすべてのスキーマが同じ構造を持ちます。Mixin は、`meta:intendedToExtend` 属性で識別される特定のクラスのみと互換性があります。

[データ型](./schema/composition.md#data-type)の場合も、スキーマに 1 つ以上のフィールドを提供できます。ただし、Mixin とは異なり、データ型は特定のクラスに制限されません。そのため、データ型は、潜在的に異なるクラスを持つ複数のスキーマで再利用可能な一般的なデータ構造を記述するためのより柔軟なオプションとなります。

### スキーマの一意の ID とは何ですか？

All [!DNL Schema Registry] resources (schemas, mixins, data types, classes) have a URI that acts as an unique ID for reference and lookup purposes. API でスキーマを表示すると、最上位レベルの `$id` および `meta:altId` 属性でスキーマが見つかります。

For more information, see the [resource identification](api/getting-started.md#resource-identification) section in the [!DNL Schema Registry] API developer guide.

### スキーマでは重大な変更をいつ回避し始めますか？

Breaking changes can be made to a schema as long as it has never been used in the creation of a dataset or enabled for use in [[!DNL Real-time Customer Profile]](../profile/home.md). Once a schema has been used in dataset creation or enabled for use with [!DNL Real-time Customer Profile], the rules of [Schema Evolution](schema/composition.md#evolution) become strictly enforced by the system.

### 長いフィールドタイプの最大サイズはどれくらいですか？

長いフィールドタイプは、最大サイズが 53（+1）ビットの整数で、可能な範囲は -9007199254740992 ～ 9007199254740992 です。これは、JSON の JavaScript 実装が長整数を表す方法に制限があるためです。

フィールドの種類について詳しくは、「 [XDMフィールドの種類の制約に関するドキュメント](./schema/field-constraints.md)」を参照してください。

### スキーマの ID を定義するには、どうすればよいですか？

In [!DNL Experience Platform], identities are used to identify a subject (typically an individual person) regardless of the sources of data being interpreted. キーフィールドを「ID」としてマークすることで、スキーマで ID が定義されます。Commonly used fields for identity include email address, phone number, [[!DNL Experience Cloud ID (ECID)]](https://docs.adobe.com/content/help/ja-JP/id-service/using/home.html), CRM ID, and other unique ID fields.

フィールドは、API またはユーザーインターフェイスを使用して ID としてマークできます。

#### API で ID を定義

API では、ID は ID 記述子を作成することで確立されます。ID 記述子は、スキーマの特定のプロパティが一意の識別子であることを示します。

ID 記述子は、/descriptors エンドポイントへの POST リクエストによって作成されます。作成に成功した場合は、HTTP ステータス 201（Created）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

For more details on creating identity descriptors in the API, see the document on [descriptors](api/descriptors.md) section in the [!DNL Schema Registry] developer guide.

#### UI で ID を定義

スキーマエディターでスキーマを開き、エディターの「**[!UICONTROL 構造]**」セクションのフィールド（ID としてマークする）をクリックします。右側の「**[!UICONTROL フィールドプロパティ]**」で、「**[!UICONTROL ID]**」チェックボックスをクリックします。

UI で ID を管理する方法について詳しくは、スキーマエディターのチュートリアルの [ID フィールドの定義](./tutorials/create-schema-ui.md#identity-field)に関する節を参照してください。

### スキーマにプライマリ ID は必要ですか？

スキーマには 0 または 1 の ID が含まれる場合があるため、プライマリ ID はオプションです。However, a schema must have a primary identity in order for the schema to be enabled for use in [!DNL Real-time Customer Profile]. 詳しくは、スキーマエディターのチュートリアルの [ID](./tutorials/create-schema-ui.md#identity-field) に関する節を参照してください。

### で使用するスキーマを有効にする方法を教えてく [!DNL Real-time Customer Profile]ださい。

Schemas are enabled for use in [[!DNL Real-time Customer Profile]](../profile/home.md) through the addition of a &quot;union&quot; tag, located in the `meta:immutableTags` attribute of the schema. Enabling a schema for use with [!DNL Profile] can be done using the API or the user interface.

#### Enabling an existing schema for [!DNL Profile] using the API

PATCH リクエストを作成して、スキーマを更新し、値「union」を含む配列として `meta:immutableTags` 属性を追加します。更新が成功すると、応答に更新されたスキーマが表示され、スキーマに和集合タグが含まれるようになります。

For more information on using the API to enable a schema for use in [!DNL Real-time Customer Profile], see the [unions](./api/unions.md) document of the [!DNL Schema Registry] developer guide.

#### Enabling an existing schema for [!DNL Profile] using the UI

In [!DNL Experience Platform], click on **[!UICONTROL Schemas]** in the left-navigation, and select the name of the schema you wish to enable from the list of schemas. 次に、エディターの右側の「**[!UICONTROL スキーマプロパティ]**」で、「**[!UICONTROL プロファイル]**」をクリックしてオンに切り替えます。


詳しくは、スキーマエディターのチュートリアルの[リアルタイム顧客プロファイルでの使用](./tutorials/create-schema-ui.md#profile)に関する節を参照してください。

### 和集合スキーマを直接編集できますか？

和集合スキーマは読み取り専用であり、システムによって自動的に生成されます。和集合スキーマを直接編集することはできません。和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されたときに、そのクラスに対して作成されます。

For more information on unions in XDM, see the [unions](./api/unions.md) section in the [!DNL Schema Registry] API developer guide.

### データをスキーマに取り込むには、どのようにデータファイルをフォーマットする必要がありますか？

[!DNL Experience Platform] は、 [!DNL Parquet] またはJSON形式のデータファイルを受け入れます。 これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。データファイル取得に関するベストプラクティスについて詳しくは、「[バッチ取得の概要](../ingestion/home.md)」を参照してください。

## エラーとトラブルシューティング

The following is a list of error messages that you may encounter when working with the [!DNL Schema Registry] API.

### オブジェクトが見つかりません

```json
{
    "type": "/placeholder/type/uri",
    "status": 404,
    "title": "NotFoundError",
    "detail": "Object https://ns.adobe.com/incorrectTenantId/schemas/ee067e31b08514d21e2b82577813409d 
      with version 1 not found"
}
```

このエラーは、システムが特定のリソースを見つけることができなかった場合に表示されます。リソースが削除されたか、API 呼び出しのパスが無効です。再試行する前に、API 呼び出しの有効なパスを入力したことを確認してください。リソースの正しい ID を入力したこと、およびパスが適切なコンテナ（グローバルまたはテナント）で適切に名前空間化されていることを確認する必要があります。。

For more information on constructing lookup paths in the API, see the [container](./api/getting-started.md#container) and [resource identification](api/getting-started.md#resource-identification) sections in the [!DNL Schema Registry] developer guide.

### タイトルは一意である必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "Title must be unique. An object 
      https://ns.adobe.com/{TENANT_ID}/schemas/26f6833e55db1dd8308aa07a64f2042d 
      already exists with the same title."
}
```

このエラーメッセージは、別のリソースで既に使用されているタイトルを持つリソースを作成しようとすると表示されます。タイトルは、すべてのリソースタイプで一意である必要があります。例えば、スキーマで既に使用されているタイトルを持つ Mixin を作成しようとすると、このエラーが表示されます。

### カスタムフィールドではトップレベルのフィールドを使用する必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "For custom fields, you must use a top level field named _{TENANT_ID}
       and all the other fields must be defined under it"
}
```

このエラーメッセージは、不適切に名前空間化されたフィールドを含む新しい Mixin を作成しようとすると表示されます。IMS 組織で定義された Mixin では、他の業界やベンダーのリソースとの競合を避けるために、`TENANT_ID` でフィールドを名前空間化する必要があります。ミックスインに適したデータ構造の詳細な例は、ミックスインエンドポイントガイドを参照して [ください](./api/mixins.md#create)。


### [!DNL Real-time Customer Profile] エラー

The following error messages are associated with operations involved in enabling schemas for [!DNL Real-time Customer Profile]. See the [unions](./api/unions.md) section in the [!DNL Schema Registry] API developer guide for more information.

#### プロファイルデータセットを有効にするには、スキーマが有効である必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "To enable profile datasets the schema should be valid"
}
```

This error message displays when you attempt to enable a profile dataset for a schema that has not been enabled for [!DNL Real-time Customer Profile]. データセットを有効にする前に、スキーマに和集合タグが含まれていることを確認してください。

#### 参照 ID 記述子が必要です

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "For a schema to be able to participate in union, if any of its 
      property is associated with a xdm:descriptorOneToOne descriptor, there must 
      be a xdm:descriptorReferenceIdentity descriptor for that property"
}
```

This error message displays when you attempt to enable a schema for [!DNL Profile] and one of its properties contains a relationship descriptor without a reference identity descriptor. このエラーを解決するには、問題のスキーマフィールドに参照 ID 記述子を追加します。

#### 参照 ID 記述子フィールドの名前空間と宛先スキーマの名前空間が一致している必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "If both schemas from an already defined xdm:descriptorOneToOne 
      descriptor are promoted to union, and if there is a primary identity on one of 
      the schemas from the xdm:descriptorOneToOne descriptor, the 
      xdm:identityNamespace of the sourceSchema's descriptorReferenceIdentity and the 
      xdm:namespace field of the xdm:descriptorIdentity for the destinationSchema must 
      match"
}
```

In order to enable schemas that contain relationship descriptors for use in [!DNL Profile], the namespace of the source field and the primary namespace of the target field must be the same. このエラーメッセージは、参照 ID 記述子に対して一致しない名前空間が含まれるスキーマを有効にしようとすると表示されます。この問題を解決するには、宛先スキーマの ID フィールドの `xdm:namespace` 値が、ソースフィールドの参照 ID 記述子にある `xdm:identityNamespace` プロパティの値と一致することを確認してください。

サポートされる ID 名前空間コードのリストについては、「ID 名前空間の概要」の「[標準の名前空間](../identity-service/namespaces.md)」の節を参照してください。

### Accept ヘッダーのエラー

Most GET requests in the [!DNL Schema Registry] API require an Accept header in order for the system to determine how to format the response. Accept ヘッダーに関連する一般的なエラーのリストを以下に示します。様々な API リクエストの互換性のある Accept ヘッダーのリストについては、[スキーマレジストリ開発者ガイド](api/getting-started.md)の対応する節を参照してください。

#### Accept ヘッダーのパラメーターが必要です

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Accept header parameter is required"
}
```

このエラーメッセージは、API リクエストに Accept ヘッダーがない場合に表示されます。再試行する前に、Accept ヘッダーが含まれていることを確認してください。

#### 不明な Accept メディアが指定されました

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Unknown Accept media supplied: xed+json"
}
```

このエラーメッセージは、Accept ヘッダーが無効な場合に表示されます。再試行する前に、作成しようとしている API リクエストと互換性のある Accept ヘッダーが正しく入力されていることを確認してください。

#### 不明な Accept 形式が使用可能です

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Unknown Accept format available "
}
```

このエラーメッセージは、記述子を検索する際に、Accept ヘッダーが正しく指定されていない場合に表示されます。再試行する前に、[記述子でサポートされている Accept ヘッダー](./api/descriptors.md)の 1 つが正しく入力されていることを確認してください。

#### Accept ヘッダーでバージョンを指定する必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "version must be supplied in the accept header. Example: 
      application/vnd.adobe.xed-full-notext+json; version=1"
}
```

このエラーメッセージは、バージョン番号が Accept ヘッダーに含まれていない場合に表示されます。スキーマなどの特定の要素については、個々のインスタンスを検索する際にバージョンを指定する必要があります。バージョン番号を含む Accept ヘッダーは次のようになります。

```plaintext
application/vnd.adobe.xed+json; version=1
```

For a list of supported Accept headers, see the [Accept header](api/getting-started.md#accept) section in the [!DNL Schema Registry] developer guide.

#### Accept ヘッダーにバージョンを指定しないでください

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "version must not be supplied in the accept header. Example: 
      application/vnd.adobe.xed-full+json"
}
```

リソースのリスト（GET）時に Accept ヘッダーにバージョンを含めようとすると、このエラーが表示されます。バージョンは、単一のリソースに対してルックアップリクエストを試行する場合にのみ必要です。エラーを解決するには、Accept ヘッダーからバージョンを削除します。