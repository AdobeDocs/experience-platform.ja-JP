---
keywords: Experience Platform；人気のあるトピック；XDM;XDM システム；XDM 個人プロファイル；XDM ExperienceEvent;XDM Experience Event;experienceEvent;experience event;XDM ExperienceEvent;XDM ExperienceEvent；エクスペリエンスデータモデル；データモデル；データモデル；スキーマ；トラブルシューティング；FAQ;FAQ；和集合スキーマ；和集合プロファイル；和集合プロファイル；http://ns.adobe.com/aep/errors/XDM-1010-404;http://ns.adobe.com/aep/errors/XDM-1011-404;http://ns.adobe.com/aep/errors/XDM-1012-404;http://ns.adobe.com/aep/errors/XDM-1013-404;http://ns.adobe.com/aep/errors/XDM-1014-404;http://ns.adobe.com/aep/errors/XDM-1015-404;http://ns.adobe.com/aep/errors/XDM-1016-404;http://ns.adobe.com/aep/errors/XDM-1017-404;http://ns.adobe.com/aep/errors/XDM-1521-400;http://ns.adobe.com/aep/errors/XDM-1020-400;http://ns.adobe.com/aep/errors/XDM-1021-400;http://ns.adobe.com/aep/errors/XDM-1022-400;http://ns.adobe.com/aep/errors/XDM-1023-400;http://ns.adobe.com/aep/errors/XDM-1024-400;http://ns.adobe.com/aep/errors/XDM-1006-400;http://ns.adobe.com/aep/errors/XDM-1007-400;http://ns.adobe.com/aep/errors/XDM-1008-400;http://ns.adobe.com/aep/errors/XDM-1009-400;http://ns.adobe.com/aep/errors/XDM-1526-400;http://ns.adobe.com/aep/errors/XDM-1527-400;http://ns.adobe.com/aep/errors/XDM-1528-400;XDM-1010-404;XDM-1011-404;XDM-1012-404;XDM-1013-404;XDM-1014-404;XDM-1015-404;XDM-1016-404;XDM-1017-404;XDM-1521-400;XDM-1020-400;XDM-1021-400;XDM-1022-400;XDM-1023-400;XDM-1024-400;XDM-1006-400;XDM-1007-400;XDM-1008-400;XDM-1009-400;XDM-1526-400;XDM-1527-400;XDM-1528-400;
solution: Experience Platform
title: XDM システムトラブルシューティングガイド
description: 一般的な API エラーを解決する手順など、エクスペリエンスデータモデル (XDM) に関するよくある質問への回答を見つけます。
topic-legacy: troubleshooting
exl-id: a0c7c661-bee8-4f66-ad5c-f669c52c9de3
source-git-commit: 4743d844b33ff391ace0d2693b66ca4298994025
workflow-type: tm+mt
source-wordcount: '1918'
ht-degree: 33%

---

# XDM システムトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformの [!DNL Experience Data Model](XDM) と XDM システムに関するよくある質問に対する回答を示します。これには、一般的なエラーのトラブルシューティングガイドが含まれます。 他の Platform サービスに関する質問とトラブルシューティングについては、[Experience Platform トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

**[!DNL Experience Data Model](XDM)** は、顧客体験管理のための標準スキーマを定義するオープンソース仕様です。[!DNL Experience Platform] を構築する方法論 **XDM システム** は、[!DNL Platform] サービスで使用する [!DNL Experience Data Model] スキーマを操作可能にします。 **[!DNL Schema Registry]** は、[!DNL Experience Platform] 内の **[!DNL Schema Library]** にアクセスするためのユーザーインターフェイスと RESTful API を提供します。 詳しくは、[XDM のドキュメント](home.md)を参照してください。

## FAQ

以下は、XDM システムと [!DNL Schema Registry] API の使用に関するよくある質問に対する回答のリストです。

### フィールドをスキーマに追加するには、どうすればよいですか？

スキーマフィールドグループを使用して、スキーマにフィールドを追加できます。 各フィールドグループは 1 つ以上のクラスと互換性があり、互換性のあるクラスの 1 つを実装する任意のスキーマでフィールドグループを使用できます。 Adobe Experience Platformは、独自の定義済みフィールドを持つ複数の業界フィールドグループを提供しますが、API またはユーザーインターフェイスを使用してカスタムフィールドグループを作成することで、独自のフィールドをスキーマに追加できます。

[!DNL Schema Registry] API でのフィールドグループの作成について詳しくは、[ フィールドグループエンドポイントのガイド ](api/field-groups.md#create) を参照してください。 UI を使用する場合は、[スキーマエディターのチュートリアル](./tutorials/create-schema-ui.md)を参照してください。

### フィールドグループとデータ型の最適な用途は何ですか。

[フィールド](./schema/composition.md#field-group) グループは、スキーマ内の 1 つ以上のフィールドを定義するコンポーネントです。フィールドグループは、スキーマの階層でフィールドが表示される方法を強制するので、フィールドグループは、スキーマに含まれるすべてのスキーマと同じ構造を持ちます。 フィールドグループは、 `meta:intendedToExtend` 属性で識別される特定のクラスとのみ互換性があります。

[データ型](./schema/composition.md#data-type)の場合も、スキーマに 1 つ以上のフィールドを提供できます。ただし、フィールドグループとは異なり、データ型は特定のクラスに制限されません。 そのため、データ型は、潜在的に異なるクラスを持つ複数のスキーマで再利用可能な一般的なデータ構造を記述するためのより柔軟なオプションとなります。

### スキーマの一意の ID とは何ですか？

すべての [!DNL Schema Registry] リソース（スキーマ、フィールドグループ、データタイプ、クラス）には、参照や検索のための一意の ID として機能する URI が含まれています。 API でスキーマを表示すると、最上位レベルの `$id` および `meta:altId` 属性でスキーマが見つかります。

詳しくは、[!DNL Schema Registry] API ガイドの [ リソースの識別 ](api/getting-started.md#resource-identification) の節を参照してください。

### スキーマでは重大な変更をいつ回避し始めますか？

データセットの作成時に重大な変更点が使用されていないか、[[!DNL Real-time Customer Profile]](../profile/home.md) で重大な変更点の使用が有効になっていない限り、スキーマに重大な変更点を加えることができます。 データセットの作成にスキーマを使用したり、[!DNL Real-time Customer Profile] での使用を有効にしたりすると、システムによって [ スキーマ進化 ](schema/composition.md#evolution) のルールが厳密に適用されます。

### 長いフィールドタイプの最大サイズはどれくらいですか？

長いフィールドタイプは、最大サイズが 53（+1）ビットの整数で、可能な範囲は -9007199254740992 ～ 9007199254740992 です。これは、JSON の JavaScript 実装が長整数を表す方法に制限があるためです。

フィールドタイプの詳細については、[XDM フィールドタイプ制約 ](./schema/field-constraints.md) のドキュメントを参照してください。

### スキーマの ID を定義するには、どうすればよいですか？

[!DNL Experience Platform] では、ID を使用して、解釈されるデータのソースに関係なく、対象（通常は個人）を識別します。 キーフィールドを「ID」としてマークすることで、スキーマで ID が定義されます。ID の一般的に使用されるフィールドには、電子メールアドレス、電話番号、[[!DNL Experience Cloud ID (ECID)]](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)、CRM ID、その他の一意の ID フィールドが含まれます。

フィールドは、API またはユーザーインターフェイスを使用して ID としてマークできます。

#### API で ID を定義

API では、ID は ID 記述子を作成することで確立されます。ID 記述子は、スキーマの特定のプロパティが一意の ID であることを示します。

ID 記述子は、/descriptors エンドポイントへの POST リクエストによって作成されます。作成に成功した場合は、HTTP ステータス 201（Created）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

API で ID 記述子を作成する方法について詳しくは、[!DNL Schema Registry] 開発者ガイドの [descriptors](api/descriptors.md) のドキュメントを参照してください。

#### UI で ID を定義

スキーマエディターでスキーマを開き、エディターの「**[!UICONTROL 構造]**」セクションにある、ID としてマークするフィールドを選択します。 右側の「**[!UICONTROL フィールドプロパティ]**」で、「**[!UICONTROL ID]**」チェックボックスを選択します。

UI で ID を管理する方法について詳しくは、スキーマエディターのチュートリアルの [ID フィールドの定義](./tutorials/create-schema-ui.md#identity-field)に関する節を参照してください。

### スキーマにプライマリ ID は必要ですか？

プライマリの ID はオプションです。スキーマは、0 または 1 を持つ場合があるからです。 ただし、[!DNL Real-time Customer Profile] でスキーマを使用するには、スキーマにプライマリ ID が必要です。 詳しくは、スキーマエディターのチュートリアルの [ID](./tutorials/create-schema-ui.md#identity-field) に関する節を参照してください。

### [!DNL Real-time Customer Profile] でスキーマの使用を有効にするにはどうすればよいですか？

スキーマの `meta:immutableTags` 属性内に「和集合」タグを追加することで、[[!DNL Real-time Customer Profile]](../profile/home.md) でスキーマの使用を有効にします。 [!DNL Profile] でのスキーマの使用を有効にするには、API またはユーザーインターフェイスを使用します。

#### API を使用して [!DNL Profile] の既存のスキーマを有効にする

PATCH リクエストを作成して、スキーマを更新し、値「union」を含む配列として `meta:immutableTags` 属性を追加します。更新が成功すると、応答に更新されたスキーマが表示され、スキーマに和集合タグが含まれるようになります。

API を使用して [!DNL Real-time Customer Profile] でのスキーマの使用を有効にする方法について詳しくは、[!DNL Schema Registry] 開発者ガイドの [ 和集合 ](./api/unions.md) のドキュメントを参照してください。

#### UI を使用した [!DNL Profile] の既存のスキーマの有効化

[!DNL Experience Platform] で、左側のナビゲーションで「**[!UICONTROL スキーマ]**」を選択し、スキーマのリストから有効にするスキーマの名前を選択します。 次に、エディターの右側の「**[!UICONTROL スキーマのプロパティ]**」で、「**[!UICONTROL プロファイル]**」を選択してオンに切り替えます。


詳しくは、スキーマエディターのチュートリアルの[リアルタイム顧客プロファイルでの使用](./tutorials/create-schema-ui.md#profile)に関する節を参照してください。

### 和集合スキーマを直接編集できますか？

和集合スキーマは読み取り専用であり、システムによって自動的に生成されます。和集合スキーマを直接編集することはできません。和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されたときに、そのクラスに対して作成されます。

XDM の和集合について詳しくは、[!DNL Schema Registry] API ガイドの [ 和集合 ](./api/unions.md) の節を参照してください。

### データをスキーマに取り込むには、どのようにデータファイルをフォーマットする必要がありますか？

[!DNL Experience Platform] は、または JSON 形式のデータ [!DNL Parquet] ファイルを受け入れます。これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。データファイル取得に関するベストプラクティスについて詳しくは、「[バッチ取得の概要](../ingestion/home.md)」を参照してください。

## エラーとトラブルシューティング

以下は、[!DNL Schema Registry] API を使用する際に発生する可能性のあるエラーメッセージのリストです。

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
>取得するリソースの種類に応じて、このエラーは次の `type` URI のいずれかを使用できます。
>
>* `http://ns.adobe.com/aep/errors/XDM-1010-404`
>* `http://ns.adobe.com/aep/errors/XDM-1011-404`
>* `http://ns.adobe.com/aep/errors/XDM-1012-404`
>* `http://ns.adobe.com/aep/errors/XDM-1013-404`
>* `http://ns.adobe.com/aep/errors/XDM-1014-404`
>* `http://ns.adobe.com/aep/errors/XDM-1015-404`
>* `http://ns.adobe.com/aep/errors/XDM-1016-404`
>* `http://ns.adobe.com/aep/errors/XDM-1017-404`


API で参照パスを作成する方法について詳しくは、[!DNL Schema Registry] 開発者ガイドの [ コンテナ ](./api/getting-started.md#container) と [ リソースの識別 ](api/getting-started.md#resource-identification) の節を参照してください。

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

### 名前空間検証エラー

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

IMS 組織で定義されたリソースは、他の業界やベンダーのリソースとの競合を避けるために、テナント ID の下のフィールドを名前空間化する必要があります。 標準のフィールドグループを使用してスキーマを作成する場合は、それらのフィールドグループの構造内に追加するカスタムフィールドも、テナント ID の下で名前空間化する必要があります。

>[!NOTE]
>
>名前空間エラーの特定の性質に応じて、このエラーは次の `type` URI のいずれかと、異なるメッセージの詳細を使用できます。
>
>* `http://ns.adobe.com/aep/errors/XDM-1020-400`
>* `http://ns.adobe.com/aep/errors/XDM-1021-400`
>* `http://ns.adobe.com/aep/errors/XDM-1022-400`
>* `http://ns.adobe.com/aep/errors/XDM-1023-400`
>* `http://ns.adobe.com/aep/errors/XDM-1024-400`


XDM リソースに適したデータ構造の詳細な例については、スキーマレジストリ API ガイドを参照してください。

* [カスタムクラスの作成](./api/classes.md#create)
* [カスタムフィールドグループの作成](./api/field-groups.md#create)
* [カスタムデータ型の作成](./api/data-types.md#create)

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

[!DNL Schema Registry] API のGETリクエストでは、応答の形式を決定するために `Accept` ヘッダーが必要です。 このエラーは、必要な `Accept` ヘッダーが無効か、見つからない場合に発生します。

使用しているエンドポイントに応じて、`detailed-message` プロパティは、有効な `Accept` ヘッダーが正常に応答した場合にどのように表示されるかを示します。 再試行する前に、作成しようとしている API リクエストと互換性のある `Accept` ヘッダーが正しく入力されていることを確認してください。

>[!NOTE]
>
>使用しているエンドポイントに応じて、このエラーは次の `type` URI のいずれかを使用できます。
>
>* `http://ns.adobe.com/aep/errors/XDM-1006-400`
>* `http://ns.adobe.com/aep/errors/XDM-1007-400`
>* `http://ns.adobe.com/aep/errors/XDM-1008-400`
>* `http://ns.adobe.com/aep/errors/XDM-1009-400`


様々な API リクエストの互換性のある Accept ヘッダーのリストについては、[スキーマレジストリ開発者ガイド](./api/overview.md)の対応する節を参照してください。

### [!DNL Real-time Customer Profile] errors

次のエラーメッセージは、[!DNL Real-time Customer Profile] のスキーマを有効にする際の操作に関連しています。 詳しくは、 [!DNL Schema Registry] API ガイドの [ 和集合 ](./api/unions.md) の節を参照してください。

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

このエラーメッセージは、[!DNL Profile] のスキーマを有効にしようとしたときに、そのプロパティの 1 つに参照 ID 記述子のない関係記述子が含まれている場合に表示されます。 このエラーを解決するには、問題のスキーマフィールドに参照 ID 記述子を追加します。

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

[!DNL Profile] で使用する関係記述子を含むスキーマを有効にするには、ソースフィールドの名前空間とターゲットフィールドのプライマリ名前空間が同じである必要があります。 このエラーメッセージは、参照 ID 記述子に対して一致しない名前空間が含まれるスキーマを有効にしようとすると表示されます。この問題を解決するには、宛先スキーマの ID フィールドの `xdm:namespace` 値が、ソースフィールドの参照 ID 記述子にある `xdm:identityNamespace` プロパティの値と一致することを確認してください。

標準 ID 名前空間コードのリストについては、「ID 名前空間の概要」の「[ 標準名前空間 ](../identity-service/namespaces.md)」の節を参照してください。

#### スキーマには、identityMap またはプライマリ ID が含まれている必要があります

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

プロファイルのスキーマを有効にする前に、まずスキーマのプライマリ ID 記述子 ](./api/descriptors.md#create) を作成するか、代わりにプライマリ ID で動作する ID マップフィールドを含める必要があります。[