---
keywords: Experience Platform;人気のトピック;XDM;XDM システム;XDM 個人プロファイル;XDM ExperienceEvent;XDM エクスペリエンスイベント;ExperienceEvent;エクスペリエンスイベント;XDM エクスペリエンスイベント;XDM ExperienceEvent;エクスペリエンスデータモデル;データモデル;スキーマ;トラブルシューティング;FAQ;faq;結合スキーマ;結合プロファイル;http://ns.adobe.com/aep/errors/XDM-1010-404;http://ns.adobe.com/aep/errors/XDM-1011-404;http://ns.adobe.com/aep/errors/XDM-1012-404;http://ns.adobe.com/aep/errors/XDM-1013-404;http://ns.adobe.com/aep/errors/XDM-1014-404;http://ns.adobe.com/aep/errors/XDM-1015-404;http://ns.adobe.com/aep/errors/XDM-1016-404;http://ns.adobe.com/aep/errors/XDM-1017-404;http://ns.adobe.com/aep/errors/XDM-1521-400;http://ns.adobe.com/aep/errors/XDM-1020-400;http://ns.adobe.com/aep/errors/XDM-1021-400;http://ns.adobe.com/aep/errors/XDM-1022-400;http://ns.adobe.com/aep/errors/XDM-1023-400;http://ns.adobe.com/aep/errors/XDM-1024-400;http://ns.adobe.com/aep/errors/XDM-1006-400;http://ns.adobe.com/aep/errors/XDM-1007-400;http://ns.adobe.com/aep/errors/XDM-1008-400;http://ns.adobe.com/aep/errors/XDM-1009-400;http://ns.adobe.com/aep/errors/XDM-1526-400;http://ns.adobe.com/aep/errors/XDM-1527-400;http://ns.adobe.com/aep/errors/XDM-1528-400;XDM-1010-404;XDM-1011-404;XDM-1012-404;XDM-1013-404;XDM-1014-404;XDM-1015-404;XDM-1016-404;XDM-1017-404;XDM-1521-400;XDM-1020-400;XDM-1021-400;XDM-1022-400;XDM-1023-400;XDM-1024-400;XDM-1006-400;XDM-1007-400;XDM-1008-400;XDM-1009-400;XDM-1413-400;XDM-1526-400;XDM-1527-400;XDM-1528-400;
solution: Experience Platform
title: XDM システムトラブルシューティングガイド
description: 一般的な API エラーを解決する手順など、エクスペリエンスデータモデル（XDM）に関するよくある質問への回答を示します。
exl-id: a0c7c661-bee8-4f66-ad5c-f669c52c9de3
source-git-commit: 7021725e011a1e1d95195c6c7318ecb5afe05ac6
workflow-type: tm+mt
source-wordcount: '2074'
ht-degree: 100%

---

# XDM システムトラブルシューティングガイド

このドキュメントでは、一般的なエラーのトラブルシューティングガイドを含め、Adobe Experience Platform の [!DNL Experience Data Model]（XDM）および XDM システムに関するよくある質問への回答を示します。他の Platform サービスに関する質問やトラブルシューティングについては、[Experience Platform トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

**[!DNL Experience Data Model]（XDM）**&#x200B;は、カスタマーエクスペリエンス（顧客体験）管理のための標準化されたスキーマを定義するオープンソース仕様です。**XDM システム**&#x200B;は [!DNL Experience Platform] の基礎となる方法論で、[!DNL Experience Data Model] スキーマを [!DNL Platform] サービスで操作できるようにするものです。**[!DNL Schema Registry]** は、[!DNL Experience Platform] 内の **[!DNL Schema Library]** にアクセスするためのユーザーインターフェイスと RESTful API を提供します。詳しくは、[XDM のドキュメント](home.md)を参照してください。

## FAQ

XDM システムと [!DNL Schema Registry] API の使用に関するよくある質問への回答を以下に示します。

### スキーマにフィールドを追加するにはどうすればよいですか？

スキーマフィールドグループを使用することで、スキーマにフィールドを追加できます。各フィールドグループは 1 つ以上のクラスに適合し、それらの適合するクラスの 1 つを実装する任意のスキーマで、そのフィールドグループを使用できます。Adobe Experience Platform は、独自の定義済みフィールドを含む複数の業界用フィールドグループを提供しますが、API またはユーザーインターフェイスを使用してカスタムフィールドグループを作成することで、スキーマに独自のフィールドを追加できます。

[!DNL Schema Registry] API でのフィールドグループの作成について詳しくは、[フィールドグループエンドポイントガイド](api/field-groups.md#create)を参照してください。UI を使用する場合は、[スキーマエディターのチュートリアル](./tutorials/create-schema-ui.md)を参照してください。

### フィールドグループとデータタイプの最適な用途は何ですか？

[フィールドグループ](./schema/composition.md#field-group)は、スキーマ内の 1 つ以上のフィールドを定義するコンポーネントです。フィールドグループは、スキーマの階層にフィールドがどのように現れるかを強制するので、フィールドが含まれているすべてのスキーマで同じ構造を示します。フィールドグループは、`meta:intendedToExtend` 属性で識別される特定のクラスにのみ適合します。

[データタイプ](./schema/composition.md#data-type)の場合も、スキーマに 1 つ以上のフィールドを提供できます。ただし、フィールドグループとは異なり、データタイプは特定のクラスに限定されません。そのため、データ型は、潜在的に異なるクラスを持つ複数のスキーマで再利用可能な一般的なデータ構造を記述するためのより柔軟なオプションとなります。

### スキーマの一意の ID とは何ですか？

[!DNL Schema Registry] のすべてのリソース（スキーマ、フィールドグループ、データタイプ、クラス）には、参照およびルックアップ用の一意の ID として機能する URI があります。API でスキーマを表示すると、最上位レベルの `$id` および `meta:altId` 属性でスキーマが見つかります。

詳しくは、[!DNL Schema Registry] API ガイドの[リソースの識別](api/getting-started.md#resource-identification)の節を参照してください。

### スキーマでは重大な変更の防止をどのような場合に開始しますか？

データセットの作成に使用されていないか、[[!DNL Real-Time Customer Profile]](../profile/home.md) での使用が有効になっていない限り、スキーマに重大な変更を加えることができます。スキーマがデータセットの作成に使用されたり、[!DNL Real-Time Customer Profile] で使用できるようになったりすると、[スキーマ進化](schema/composition.md#evolution) のルールがシステムで厳密に適用されます。

### 長いフィールドタイプの最大サイズはどれくらいですか？

長いフィールドタイプは、最大サイズが 53（+1）ビットの整数で、可能な範囲は -9007199254740992 ～ 9007199254740992 です。これは、JSON の JavaScript 実装が長整数を表す方法に制限があるためです。

フィールドタイプについて詳しくは、[XDM フィールドタイプの制約](./schema/field-constraints.md)に関するドキュメントを参照してください。

### スキーマの ID を定義するにはどうすればよいですか？

[!DNL Experience Platform] では、ID は、解釈されるデータのソースに関係なく、サブジェクト（通常は個人）の識別に使用されます。キーフィールドを「ID」としてマークすることで、スキーマで ID が定義されます。ID によく使用されるフィールドには、メールアドレス、電話番号、[[!DNL Experience Cloud ID (ECID)]](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)、CRM ID、その他の一意の ID フィールドなどがあります。

フィールドは、API またはユーザーインターフェイスを使用して ID としてマークできます。

#### API で ID を定義

API では、ID は ID 記述子を作成することで確立されます。ID 記述子は、スキーマの特定のプロパティが一意の ID であることを示します。

ID 記述子は、/descriptors エンドポイントへの POST リクエストによって作成されます。作成に成功した場合は、HTTP ステータス 201（Created）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

API での ID 記述子の作成について詳しくは、[!DNL Schema Registry] 開発者ガイドの[記述子](api/descriptors.md)に関する節を参照してください。

#### UI での ID の定義

スキーマエディターにスキーマを開いた状態で、ID としてマークするフィールドをエディターの「**[!UICONTROL 構造]**」セクションで選択します。右側の「**[!UICONTROL フィールドプロパティ]**」で、「**[!UICONTROL ID]**」チェックボックスをオンにします。

UI で ID を管理する方法について詳しくは、スキーマエディターのチュートリアルの [ID フィールドの定義](./tutorials/create-schema-ui.md#identity-field)に関する節を参照してください。

### スキーマにプライマリ ID は必要ですか？

スキーマにはプライマリ ID がないか 1 つのみ存在できるので、プライマリ ID はオプションです。ただし、スキーマを [!DNL Real-Time Customer Profile] で使用できるようにするには、スキーマにプライマリ ID が必要です。詳しくは、スキーマエディターのチュートリアルの [ID](./tutorials/create-schema-ui.md#identity-field) に関する節を参照してください。

### [!DNL Real-Time Customer Profile] でスキーマを使用できるようにするにはどうすればよいですか？

スキーマの `meta:immutableTags` 属性内に「union」タグを追加すれば、スキーマを [[!DNL Real-Time Customer Profile]](../profile/home.md) で使用できるようになります。スキーマを [!DNL Profile] で使用できるようにするには、API またはユーザーインターフェイスを使用します。

#### API を使用して既存のスキーマを [!DNL Profile] に有効にする方法

PATCH リクエストを作成して、スキーマを更新し、値「union」を含む配列として `meta:immutableTags` 属性を追加します。更新が成功すると、応答に更新されたスキーマが表示され、スキーマに和集合タグが含まれるようになります。

API を使用して、スキーマを [!DNL Real-Time Customer Profile] で使用できるようにする方法について詳しくは、[!DNL Schema Registry] 開発者ガイドの[結合](./api/unions.md)に関するドキュメントを参照してください。

#### UI を使用して既存のスキーマを [!DNL Profile] に有効にする方法

[!DNL Experience Platform] で、左側のナビゲーションにある「**[!UICONTROL スキーマ]**」を選択し、有効にするスキーマの名前をスキーマのリストから選択します。次に、エディターの右側の「**[!UICONTROL スキーマプロパティ]**」で、「**[!UICONTROL プロファイル]**」を選択してオンに切り替えます。


詳しくは、[!UICONTROL スキーマエディター]のチュートリアルの[リアルタイム顧客プロファイルでの使用](./tutorials/create-schema-ui.md#profile)に関する節を参照してください。

### 和集合スキーマを直接編集できますか？

和集合スキーマは読み取り専用であり、システムによって自動的に生成されます。和集合スキーマを直接編集することはできません。和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されたときに、そのクラスに対して作成されます。

XDM での結合について詳しくは、[!DNL Schema Registry] API ガイドの[結合](./api/unions.md)の節を参照してください。

### スキーマにデータを取り込むには、データファイルをどのような形式にする必要がありますか？

[!DNL Experience Platform] では、[!DNL Parquet] または JSON 形式のデータファイルを受け入れます。これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。データファイル取得に関するベストプラクティスについて詳しくは、「[バッチ取得の概要](../ingestion/home.md)」を参照してください。

## エラーとトラブルシューティング

[!DNL Schema Registry] API を使用する際に発生する可能性のあるエラーメッセージのリストを以下に示します。

### リソースが見つかりません

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1010-404",
    "title": "Resource not found",
    "status": 404,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "The requested class resource https://ns.adobe.com/{TENANT_ID}/classes/11447bb484d4599d2cd9b0aseefff78b463cbbde1527f498 with version 1 is not found.",
        "sub-errors": []
    },
    "detail": "The requested class resource https://ns.adobe.com/{TENANT_ID}/classes/11447bb484d4599d2cd9b0aseefff78b463cbbde1527f498 with version 1 is not found."
}
```

このエラーは、システムが特定のリソースを見つけることができなかった場合に表示されます。リソースが削除されたか、API 呼び出しのパスが無効です。再試行する前に、API 呼び出しの有効なパスを入力したことを確認してください。リソースの正しい ID を入力したこと、およびパスが適切なコンテナ（グローバルまたはテナント）で適切に名前空間化されていることを確認する必要があります。。

>[!NOTE]
>
>取得するリソースタイプに応じて、このエラーは次の `type` URI のいずれかを使用できます。
>
>* `http://ns.adobe.com/aep/errors/XDM-1010-404`
>* `http://ns.adobe.com/aep/errors/XDM-1011-404`
>* `http://ns.adobe.com/aep/errors/XDM-1012-404`
>* `http://ns.adobe.com/aep/errors/XDM-1013-404`
>* `http://ns.adobe.com/aep/errors/XDM-1014-404`
>* `http://ns.adobe.com/aep/errors/XDM-1015-404`
>* `http://ns.adobe.com/aep/errors/XDM-1016-404`
>* `http://ns.adobe.com/aep/errors/XDM-1017-404`


API での参照パスの作成について詳しくは、[!DNL Schema Registry] 開発者ガイドの[コンテナ](./api/getting-started.md#container)および[リソースの識別](api/getting-started.md#resource-identification)の節を参照してください。

### タイトルが一意ではありません

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1521-400",
    "title": "Title not unique",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "Object titles must be unique. An object https://ns.adobe.com/{TENANT_ID}/classes/11447bb484d4599d2cd9b0aseefff78b463cbbde1527f498 already exists with the same title",
        "sub-errors": []
    },
    "detail": "Object titles must be unique. An object https://ns.adobe.com/{TENANT_ID}/classes/11447bb484d4599d2cd9b0aseefff78b463cbbde1527f498 already exists with the same title"
}
```

このエラーメッセージは、別のリソースで既に使用されているタイトルを持つリソースを作成しようとすると表示されます。タイトルは、すべてのリソースタイプで一意である必要があります。例えば、スキーマで既に使用されているタイトルのフィールドグループを作成しようとすると、このエラーが表示されます。

### 名前空間の検証エラー

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1021-400",
    "title": "Namespace validation error",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "A custom field is defined under an invalid namespace. All custom fields must be defined under a top-level field named {TENANT_ID}.",
        "sub-errors": []
    },
    "detail": "A custom field is defined under an invalid namespace. All custom fields must be defined under a top-level field named {TENANT_ID}."
}
```

このエラーメッセージが表示されるのは、名前空間の設定が適切でないフィールドを含んだリソースを作成しようとした場合や、名前空間の設定が適切でないフィールドを既存のリソースに追加しようとした場合です。

IMS 組織で定義されたリソースでは、他の業界やベンダーのリソースとの競合を避けるために、フィールドの名前空間をテナント ID の下に設定する必要があります。標準のフィールドグループを使用してスキーマを作成する場合は、それらのフィールドグループの構造内に追加するカスタムフィールドも、テナント ID の下に名前空間を設定する必要があります。

>[!NOTE]
>
>このエラーでは、名前空間エラーの固有の性質に応じて、次の `type` URI のいずれかを、異なるメッセージ詳細と共に使用できます。
>
>* `http://ns.adobe.com/aep/errors/XDM-1020-400`
>* `http://ns.adobe.com/aep/errors/XDM-1021-400`
>* `http://ns.adobe.com/aep/errors/XDM-1022-400`
>* `http://ns.adobe.com/aep/errors/XDM-1023-400`
>* `http://ns.adobe.com/aep/errors/XDM-1024-400`


XDM リソースの適切なデータ構造の詳細な例については、スキーマレジストリ API ガイドを参照してください。

* [カスタムクラスの作成](./api/classes.md#create)
* [カスタムフィールドグループの作成](./api/field-groups.md#create)
* [カスタムデータタイプの作成](./api/data-types.md#create)

### Accept ヘッダーが無効です

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1006-400",
    "title": "Accept header invalid",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "The supplied Accept header is not valid: application/vnd.adobe.xed+json;version=1 - A valid Accept value should look like application/vnd.adobe.{xed|xdm}+json",
        "sub-errors": []
    },
    "detail": "The supplied Accept header is not valid: application/vnd.adobe.xed+json;version=1 - A valid Accept value should look like application/vnd.adobe.{xed|xdm}+json"
}
```

[!DNL Schema Registry] API の GET リクエストには、システムで応答の形式を決定するために `Accept` ヘッダーが必要です。このエラーは、必須の `Accept` ヘッダーが無効か見つからない場合に発生します。

使用しているエンドポイントに応じて、正常な応答を得るための有効な `Accept` ヘッダーがどのようなものであるかを `detailed-message` プロパティが示します。実行しようとしている API リクエストに適合する `Accept` ヘッダーを正しく入力してあることを確認してから、もう一度やり直してください。

>[!NOTE]
>
>このエラーでは、使用しているエンドポイントに応じて、次の `type` URI のいずれかを使用できます。
>
>* `http://ns.adobe.com/aep/errors/XDM-1006-400`
>* `http://ns.adobe.com/aep/errors/XDM-1007-400`
>* `http://ns.adobe.com/aep/errors/XDM-1008-400`
>* `http://ns.adobe.com/aep/errors/XDM-1009-400`


様々な API リクエストに適合する Accept ヘッダーのリストについては、[スキーマレジストリ開発者ガイド](./api/overview.md)の対応する節を参照してください。

### [!DNL Real-Time Customer Profile] のエラー

次のエラーメッセージは、スキーマを [!DNL Real-Time Customer Profile] で使用できるようにする場合の操作に関連しています。詳しくは、[!DNL Schema Registry] API 開発者ガイドの[結合](./api/unions.md)の節を参照してください。

#### 参照 ID 記述子が必要です

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1526-400",
    "title": "Union descriptor validation error",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "If a schema contains properties that are associated with an xdm:descriptorOneToOne descriptor, those properties must also have a xdm:descriptorReferenceIdentity descriptor for that schema to participate in a union.",
        "sub-errors": []
    },
    "detail": "If a schema contains properties that are associated with an xdm:descriptorOneToOne descriptor, those properties must also have a xdm:descriptorReferenceIdentity descriptor for that schema to participate in a union."
}
```

このエラーメッセージが表示されるのは、スキーマを [!DNL Profile] で使用できるようにしようとしたときに、そのスキーマのプロパティのいずれかに参照 ID 記述子のない関係記述子が含まれている場合です。このエラーを解決するには、問題のスキーマフィールドに参照 ID 記述子を追加します。

#### 参照 ID 記述子フィールドの名前空間と宛先スキーマの名前空間が一致している必要があります

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1527-400",
    "title": "Union descriptor validation error",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "If both schemas from an existing xdm:descriptorOneToOne descriptor are promoted to union, and one of those schemas contains a primary identity, the xdm:identityNamespace of the source schema's descriptorReferenceIdentity field must match the xdm:namespace field of destination schema's xdm:descriptorIdentity field.",
        "sub-errors": []
    },
    "detail": "If both schemas from an existing xdm:descriptorOneToOne descriptor are promoted to union, and one of those schemas contains a primary identity, the xdm:identityNamespace of the source schema's descriptorReferenceIdentity field must match the xdm:namespace field of destination schema's xdm:descriptorIdentity field."
}
```

>[!NOTE]
>
>このエラーの場合、「宛先スキーマ」は関係の参照スキーマを指します。

関係記述子を含んだスキーマを [!DNL Profile] で使用できるようにするには、ソースフィールドの名前空間と参照フィールドのプライマリ名前空間を同じにする必要があります。このエラーメッセージが表示されるのは、参照 ID 記述子に一致しない名前空間を含んだスキーマを有効にしようとした場合です。

この問題を解決するには、参照スキーマの ID フィールドの `xdm:namespace` 値が、ソースフィールドの参照 ID 記述子における `xdm:identityNamespace` プロパティの値と一致することを確認してください。

標準の ID 名前空間コードのリストについては、「ID 名前空間の概要」の[標準の名前空間](../identity-service/namespaces.md)の節を参照してください。

#### スキーマには identityMap またはプライマリ ID が含まれている必要があります

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1528-400",
    "title": "Union descriptor validation error",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "To participate in a union, a schema must include an identityMap fieldgroup or a primary identity descriptor.",
        "sub-errors": []
    },
    "detail": "To participate in a union, a schema must include an identityMap fieldgroup or a primary identity descriptor."
}
```

プロファイルのスキーマを有効にする前に、まずスキーマの[プライマリ ID 記述子を作成](./api/descriptors.md#create)するか、代わりにプライマリ ID として機能する ID マップフィールドを含める必要があります。

#### 互換性のないデータタイプは結合できません

```json
{
    "type": "http://ns.adobe.com/aep/errors/XDM-1413-400",
    "title": "Merge Schema Error",
    "status": 400,
    "report": {
        "registryRequestId": "a15996b5-5133-4cec-9bf7-7d1207904ae3",
        "timestamp": "06-01-2021 04:11:06",
        "detailed-message": "Cannot merge incompatible data types. The path /person/name has already been defined in schema (id=0238be93d3e7a06aec5e0655955901ec) using a different data type. Types: string, object",
        "sub-errors": []
    },
    "detail": "Cannot merge incompatible data types. The path /person/name has already been defined in schema (id=0238be93d3e7a06aec5e0655955901ec) using a different data type. Types: string, object"
}
```

あるクラスの結合スキーマを構成するには、その同じクラスに属するすべてのプロファイル対応スキーマを結合できる必要があります。 このエラーが表示されるのは、パスが別のプロファイル対応スキーマと共有されているスキーマにフィールドを追加しようとして、データタイプが元と異なる場合です。 これらのスキーマはどちらもプロファイル対応で、同じフィールドパスを含んでいるので、プロファイルは結合スキーマを構成する際に、これら 2 つのフィールドを 1 つに結合しようとします。 しかし、異なるデータタイプを結合することはできないので、これは結合の競合と見なされ、許可されません。

この問題を解決するには、類似のフィールドを持つ同じクラスで他のプロファイル対応スキーマとの結合の競合が発生するのを避けるために、フィールドに別の名前を選択するか、一意の名前空間に属するオブジェクトの下にフィールドをネストします。
