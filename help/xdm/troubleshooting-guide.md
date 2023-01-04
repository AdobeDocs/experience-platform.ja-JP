---
keywords: Experience Platform；人気のトピック；XDM;XDM システム；XDM 個人プロファイル；XDM ExperienceEvent;XDM ExperienceEvent;experienceEvent;XDM ExperienceEvent;XDM ExperienceEvent；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマ；トラブルシューティング；FAQ;FAQ；和集合スキーマ；UNION PROFILE；和集合プロファイル；http://ns.adobe.com/aep/errors/XDM-1010-404;http://ns.adobe.com/aep/errors/XDM-1011-404;http://ns.adobe.com/aep/errors/XDM-1012-404;http://ns.adobe.com/aep/errors/XDM-1013-404;http://ns.adobe.com/aep/errors/XDM-1014-404;http://ns.adobe.com/aep/errors/XDM-1015-404;http://ns.adobe.com/aep/errors/XDM-1016-404;http://ns.adobe.com/aep/errors/XDM-1017-404;http://ns.adobe.com/aep/errors/XDM-1521-400;http://ns.adobe.com/aep/errors/XDM-1020-400;http://ns.adobe.com/aep/errors/XDM-1021-400;http://ns.adobe.com/aep/errors/XDM-1022-400;http://ns.adobe.com/aep/errors/XDM-1023-400;http://ns.adobe.com/aep/errors/XDM-1024-400;http://ns.adobe.com/aep/errors/XDM-1006-400;http://ns.adobe.com/aep/errors/XDM-1007-400;http://ns.adobe.com/aep/errors/XDM-1008-400;http://ns.adobe.com/aep/errors/XDM-1009-400;http://ns.adobe.com/aep/errors/XDM-1526-400;http://ns.adobe.com/aep/errors/XDM-1527-400;http://ns.adobe.com/aep/errors/XDM-1528-400;XDM-1010-404;XDM-1011-404;XDM-1012-404;XDM-1013-404;XDM-1014-404;XDM-1015-404;XDM-1016-404;XDM-1017-404;XDM-1521-400;XDM-1020-400;XDM-1021-400;XDM-1022-400;XDM-1023-400;XDM-1024-400;XDM-1006-400;XDM-1007-400;XDM-1008-400;XDM-1009-400;XDM-1413-400;XDM-1526-400;XDM-1527-400;XDM-1528-400;
solution: Experience Platform
title: XDM システムトラブルシューティングガイド
description: 一般的な API エラーを解決する手順など、エクスペリエンスデータモデル (XDM) に関するよくある質問への回答を見つけます。
topic-legacy: troubleshooting
exl-id: a0c7c661-bee8-4f66-ad5c-f669c52c9de3
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '2060'
ht-degree: 30%

---

# XDM システムトラブルシューティングガイド

このドキュメントでは、 [!DNL Experience Data Model] (XDM) およびAdobe Experience Platformの XDM システム。一般的なエラーのトラブルシューティングガイドを含みます。 他の Platform サービスに関する質問とトラブルシューティングについては、[Experience Platform トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

**[!DNL Experience Data Model](XDM)** は、顧客体験管理のための標準化されたスキーマを定義するオープンソース仕様です。 方法 [!DNL Experience Platform] が構築されている **XDM システム**、操作可能 [!DNL Experience Data Model] スキーマ [!DNL Platform] サービス。 この **[!DNL Schema Registry]** は、 **[!DNL Schema Library]** 範囲 [!DNL Experience Platform]. 詳しくは、[XDM のドキュメント](home.md)を参照してください。

## FAQ

次に、XDM システムとの使用に関するよくある質問に対する回答のリストを示します [!DNL Schema Registry] API

### フィールドをスキーマに追加するには、どうすればよいですか？

スキーマフィールドグループを使用して、スキーマにフィールドを追加できます。 各フィールドグループは 1 つ以上のクラスと互換性があり、互換性のあるクラスの 1 つを実装する任意のスキーマでフィールドグループを使用できます。 Adobe Experience Platformは、独自の事前定義済みフィールドを持つ複数の業界フィールドグループを提供しますが、API またはユーザーインターフェイスを使用してカスタムフィールドグループを作成することで、独自のフィールドをスキーマに追加できます。

詳しくは、 [!DNL Schema Registry] API( [フィールドグループエンドポイントガイド](api/field-groups.md#create). UI を使用する場合は、[スキーマエディターのチュートリアル](./tutorials/create-schema-ui.md)を参照してください。

### フィールドグループとデータタイプの最適な用途は何ですか？

[フィールドグループ](./schema/composition.md#field-group) は、スキーマ内の 1 つ以上のフィールドを定義するコンポーネントです。 フィールドグループは、スキーマの階層でフィールドが表示される方法を強制するので、フィールドグループは、スキーマが含まれるすべてのスキーマで同じ構造を示します。 フィールドグループは、特定のクラス ( `meta:intendedToExtend` 属性。

[データ型](./schema/composition.md#data-type)の場合も、スキーマに 1 つ以上のフィールドを提供できます。ただし、フィールドグループとは異なり、データ型は特定のクラスに制限されません。 そのため、データ型は、潜在的に異なるクラスを持つ複数のスキーマで再利用可能な一般的なデータ構造を記述するためのより柔軟なオプションとなります。

### スキーマの一意の ID とは何ですか？

すべて [!DNL Schema Registry] リソース（スキーマ、フィールドグループ、データタイプ、クラス）には、参照および検索のために一意の ID として機能する URI が含まれます。 API でスキーマを表示すると、最上位レベルの `$id` および `meta:altId` 属性でスキーマが見つかります。

詳しくは、 [リソース識別](api/getting-started.md#resource-identification) セクション [!DNL Schema Registry] API ガイド。

### スキーマでは重大な変更をいつ回避し始めますか？

データセットの作成時に重大な変更点が使用されていないか、で重大な変更点の使用が有効になっていない限り、スキーマに重大な変更を加えることができます [[!DNL Real-Time Customer Profile]](../profile/home.md). スキーマがデータセットの作成に使用されたり、での使用が有効になったりしたら、 [!DNL Real-Time Customer Profile]、 [スキーマの変化](schema/composition.md#evolution) システムによって厳密に強制されます。

### 長いフィールドタイプの最大サイズはどれくらいですか？

長いフィールドタイプは、最大サイズが 53（+1）ビットの整数で、可能な範囲は -9007199254740992 ～ 9007199254740992 です。これは、JSON の JavaScript 実装が長整数を表す方法に制限があるためです。

フィールドタイプの詳細については、 [XDM フィールドタイプ制約](./schema/field-constraints.md).

### スキーマの ID を定義するには、どうすればよいですか？

In [!DNL Experience Platform]の場合、id は、解釈されるデータのソースに関係なく、主体（通常は個人）を識別するために使用されます。 キーフィールドを「ID」としてマークすることで、スキーマで ID が定義されます。ID の一般的に使用されるフィールドには、電子メールアドレス、電話番号、 [[!DNL Experience Cloud ID (ECID)]](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)、CRM ID およびその他の一意の ID フィールド。

フィールドは、API またはユーザーインターフェイスを使用して ID としてマークできます。

#### API で ID を定義

API では、ID は ID 記述子を作成することで確立されます。ID 記述子は、スキーマの特定のプロパティが一意の ID であることを示します。

ID 記述子は、/descriptors エンドポイントへの POST リクエストによって作成されます。作成に成功した場合は、HTTP ステータス 201（Created）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

API で ID 記述子を作成する方法について詳しくは、 [記述子](api/descriptors.md) セクション [!DNL Schema Registry] 開発者ガイド。

#### UI で ID を定義

スキーマエディターでスキーマを開き、 **[!UICONTROL 構造]** ID としてマークするエディターのセクション。 の下 **[!UICONTROL フィールドプロパティ]** 右側で、 **[!UICONTROL ID]** チェックボックス。

UI で ID を管理する方法について詳しくは、スキーマエディターのチュートリアルの [ID フィールドの定義](./tutorials/create-schema-ui.md#identity-field)に関する節を参照してください。

### スキーマにプライマリ ID は必要ですか？

プライマリID はオプションです。スキーマには 0 または 1 の ID が含まれる場合があるからです。 ただし、でスキーマを使用するには、スキーマにプライマリ ID が必要です [!DNL Real-Time Customer Profile]. 詳しくは、スキーマエディターのチュートリアルの [ID](./tutorials/create-schema-ui.md#identity-field) に関する節を参照してください。

### でスキーマの使用を有効にする方法を教えてください。 [!DNL Real-Time Customer Profile]?

スキーマはでの使用に対して有効になっています [[!DNL Real-Time Customer Profile]](../profile/home.md) ( `meta:immutableTags` スキーマの属性。 でのスキーマの使用の有効化 [!DNL Profile] は、API またはユーザーインターフェイスを使用して実行できます。

#### の既存のスキーマの有効化 [!DNL Profile] API の使用

PATCH リクエストを作成して、スキーマを更新し、値「union」を含む配列として `meta:immutableTags` 属性を追加します。更新が成功すると、応答に更新されたスキーマが表示され、スキーマに和集合タグが含まれるようになります。

API を使用してでのスキーマの使用を有効にする方法について詳しくは、 [!DNL Real-Time Customer Profile]を参照し、 [和集合](./api/unions.md) ドキュメント [!DNL Schema Registry] 開発者ガイド。

#### の既存のスキーマの有効化 [!DNL Profile] UI の使用

In [!DNL Experience Platform]を選択します。 **[!UICONTROL スキーマ]** 左側のナビゲーションで、有効にするスキーマの名前をスキーマのリストから選択します。 次に、エディターの右側の、下の **[!UICONTROL スキーマのプロパティ]**&#x200B;を選択します。 **[!UICONTROL プロファイル]** オンに切り替えるには、をクリックします。


詳しくは、 [リアルタイム顧客プロファイルでの使用](./tutorials/create-schema-ui.md#profile) 内 [!UICONTROL スキーマエディター] チュートリアル

### 和集合スキーマを直接編集できますか？

和集合スキーマは読み取り専用であり、システムによって自動的に生成されます。和集合スキーマを直接編集することはできません。和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されたときに、そのクラスに対して作成されます。

XDM の和集合について詳しくは、 [和集合](./api/unions.md) セクション [!DNL Schema Registry] API ガイド。

### データをスキーマに取り込むには、どのようにデータファイルをフォーマットする必要がありますか？

[!DNL Experience Platform] 次のいずれかの方法でデータファイルを受け入れる [!DNL Parquet] または JSON 形式を使用します。 これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。データファイル取得に関するベストプラクティスについて詳しくは、「[バッチ取得の概要](../ingestion/home.md)」を参照してください。

## エラーとトラブルシューティング

以下は、 [!DNL Schema Registry] API

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
>取得するリソースタイプに応じて、このエラーは次のいずれかを使用できます `type` URI:
>
>* `http://ns.adobe.com/aep/errors/XDM-1010-404`
>* `http://ns.adobe.com/aep/errors/XDM-1011-404`
>* `http://ns.adobe.com/aep/errors/XDM-1012-404`
>* `http://ns.adobe.com/aep/errors/XDM-1013-404`
>* `http://ns.adobe.com/aep/errors/XDM-1014-404`
>* `http://ns.adobe.com/aep/errors/XDM-1015-404`
>* `http://ns.adobe.com/aep/errors/XDM-1016-404`
>* `http://ns.adobe.com/aep/errors/XDM-1017-404`


API で参照パスを作成する方法について詳しくは、 [コンテナ](./api/getting-started.md#container) および [リソース識別](api/getting-started.md#resource-identification) セクション [!DNL Schema Registry] 開発者ガイド。

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

このエラーメッセージは、別のリソースで既に使用されているタイトルを持つリソースを作成しようとすると表示されます。タイトルは、すべてのリソースタイプで一意である必要があります。例えば、スキーマで既に使用されているタイトルを持つフィールドグループを作成しようとすると、このエラーが表示されます。

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

このエラーメッセージは、不適切に名前空間化されたフィールドを含むリソースを作成しようとした場合や、不適切に名前空間化されたフィールドを既存のリソースに追加しようとした場合に表示されます。

IMS 組織で定義されたリソースは、他の業界やベンダーのリソースとの競合を避けるために、テナント ID の下でフィールドを名前空間化する必要があります。 標準のフィールドグループを使用してスキーマを作成する場合は、それらのフィールドグループの構造内に追加するカスタムフィールドも、テナント ID の下で名前空間化する必要があります。

>[!NOTE]
>
>名前空間エラーの特定の特性に応じて、このエラーは次のいずれかを使用できます `type` URI と異なるメッセージの詳細：
>
>* `http://ns.adobe.com/aep/errors/XDM-1020-400`
>* `http://ns.adobe.com/aep/errors/XDM-1021-400`
>* `http://ns.adobe.com/aep/errors/XDM-1022-400`
>* `http://ns.adobe.com/aep/errors/XDM-1023-400`
>* `http://ns.adobe.com/aep/errors/XDM-1024-400`


XDM リソースに適したデータ構造の詳細な例については、『スキーマレジストリ API ガイド』を参照してください。

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

GETリクエスト [!DNL Schema Registry] API には `Accept` ヘッダーを使用して、応答の形式を決定します。 このエラーは、必須の `Accept` ヘッダーが無効か、見つかりません。

使用しているエンドポイントに応じて、 `detailed-message` プロパティは、有効な `Accept` ヘッダーは、成功応答の場合はのようになります。 正しく入力されていることを確認し、 `Accept` 再試行する前に作成しようとしている API リクエストと互換性のあるヘッダー。

>[!NOTE]
>
>使用しているエンドポイントに応じて、このエラーは次のいずれかを使用できます `type` URI:
>
>* `http://ns.adobe.com/aep/errors/XDM-1006-400`
>* `http://ns.adobe.com/aep/errors/XDM-1007-400`
>* `http://ns.adobe.com/aep/errors/XDM-1008-400`
>* `http://ns.adobe.com/aep/errors/XDM-1009-400`


様々な API リクエストの互換性のある Accept ヘッダーのリストについては、[スキーマレジストリ開発者ガイド](./api/overview.md)の対応する節を参照してください。

### [!DNL Real-Time Customer Profile] エラー

次のエラーメッセージは、 [!DNL Real-Time Customer Profile]. 詳しくは、 [和集合](./api/unions.md) セクション [!DNL Schema Registry] API ガイドを参照してください。

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

このエラーメッセージは、 [!DNL Profile] およびそのプロパティの 1 つには、参照 id 記述子のない関係記述子が含まれています。 このエラーを解決するには、問題のスキーマフィールドに参照 ID 記述子を追加します。

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

で使用する関係記述子を含むスキーマを有効にするには、次の手順を実行します。 [!DNL Profile]の場合、ソースフィールドの名前空間とターゲットフィールドのプライマリ名前空間は同じである必要があります。 このエラーメッセージは、参照 ID 記述子に対して一致しない名前空間が含まれるスキーマを有効にしようとすると表示されます。この問題を解決するには、宛先スキーマの ID フィールドの `xdm:namespace` 値が、ソースフィールドの参照 ID 記述子にある `xdm:identityNamespace` プロパティの値と一致することを確認してください。

標準の ID 名前空間コードのリストについては、 [標準名前空間](../identity-service/namespaces.md) （「id 名前空間の概要」）。

#### スキーマには、identityMap またはプライマリ ID を含める必要があります

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

プロファイルに対してスキーマを有効にする前に、まず [プライマリ id 記述子の作成](./api/descriptors.md#create) スキーマの場合は、代わりにプライマリ id で動作する id マップフィールドを含めます。

#### 互換性のないデータタイプを結合できません

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

同じクラスに属するすべてのプロファイル対応スキーマは、そのクラスの和集合スキーマを構築するために、結合できる必要があります。 このエラーは、パスが別のプロファイルが有効なスキーマと共有され、データタイプが元のスキーマと異なるフィールドをスキーマに追加しようとすると表示されます。 スキーマは両方ともプロファイル対応で、同じフィールドパスを含むので、プロファイルは和集合スキーマを構築する際に、これら 2 つのフィールドの結合を試みます。 異なるデータ型を結合することはできないので、結合の競合と見なされ、使用できません。

この問題を解決するには、同じクラスで類似のフィールドを持つ他のプロファイル対応スキーマとの結合の競合を避けるために、フィールドに別の名前を選択するか、一意の名前空間オブジェクトの下にネストします。
