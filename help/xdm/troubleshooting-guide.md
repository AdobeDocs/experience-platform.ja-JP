---
keywords: Experience Platform；ホーム；人気の高いトピック；XDM;XDMプロファイル;XDM個人システム；XDM個人イベント;XDM ExperienceEvent;XDM ExperienceEventイベント;XDM ExperienceEvent；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデルモデル；スキーマ；トラブルシューティング；FAQ;和集合スキーマ;イベント和集合;プロファイル和集合
solution: Experience Platform
title: XDMシステムトラブルシューティングガイド
description: このドキュメントでは、Adobe Experience PlatformのExperience Data Model(XDM)とXDM Systemに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。
topic-legacy: troubleshooting
exl-id: a0c7c661-bee8-4f66-ad5c-f669c52c9de3
translation-type: tm+mt
source-git-commit: 3985ba8f46a62e8d9ea8b1f084198b245318a24f
workflow-type: tm+mt
source-wordcount: '1898'
ht-degree: 49%

---

# XDMシステムトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformの[!DNL Experience Data Model] (XDM)とXDM Systemに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 他の Platform サービスに関する質問とトラブルシューティングについては、[Experience Platform トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

**[!DNL Experience Data Model](XDM)** は、顧客体験管理のための標準化されたスキーマを定義するオープンソース仕様です。[!DNL Experience Platform]が構築される方法論&#x200B;**XDM System**&#x200B;は[!DNL Platform]サービスで使用する[!DNL Experience Data Model]スキーマを操作します。 **[!DNL Schema Registry]**&#x200B;は、[!DNL Experience Platform]内の&#x200B;**[!DNL Schema Library]**&#x200B;にアクセスするためのユーザーインターフェイスとRESTful APIを提供します。 詳しくは、[XDM のドキュメント](home.md)を参照してください。

## FAQ

以下は、XDMシステムと[!DNL Schema Registry] APIの使い方に関するよくある質問に対する回答のリストです。

### フィールドをスキーマに追加するには、どうすればよいですか？

スキーマフィールドグループを使用して、スキーマにフィールドを追加できます。 各フィールドグループは1つ以上のクラスと互換性があり、それらの互換性のあるクラスの1つを実装する任意のスキーマでフィールドグループを使用できます。 Adobe Experience Platformは、独自の定義済みフィールドを持つ複数の業界フィールドグループを提供していますが、APIまたはユーザーインターフェイスを使用して新しいフィールドグループを作成することで、独自のフィールドをスキーマに追加できます。

[!DNL Schema Registry] APIでの新しいフィールドグループの作成について詳しくは、[フィールドグループエンドポイントガイド](api/field-groups.md#create)を参照してください。 UI を使用する場合は、[スキーマエディターのチュートリアル](./tutorials/create-schema-ui.md)を参照してください。

### フィールドグループとデータタイプの最も良い用途は何か。

[フィールド](./schema/composition.md#field-group) グループとは、スキーマ内の1つ以上のフィールドを定義するコンポーネントを指します。フィールドグループは、スキーマの階層でのフィールドの表示を強制します。したがって、フィールドグループが含まれるすべてのスキーマで同じ構造を示します。 フィールドグループは、`meta:intendedToExtend`属性で識別される特定のクラスとのみ互換性があります。

[データ型](./schema/composition.md#data-type)の場合も、スキーマに 1 つ以上のフィールドを提供できます。ただし、フィールドグループとは異なり、データ型は特定のクラスに制限されません。 そのため、データ型は、潜在的に異なるクラスを持つ複数のスキーマで再利用可能な一般的なデータ構造を記述するためのより柔軟なオプションとなります。

### スキーマの一意の ID とは何ですか？

すべての[!DNL Schema Registry]リソース(スキーマ、フィールドグループ、データタイプ、クラス)には、参照および参照の目的で一意のIDとして機能するURIがあります。 API でスキーマを表示すると、最上位レベルの `$id` および `meta:altId` 属性でスキーマが見つかります。

詳しくは、[!DNL Schema Registry] API開発者ガイドの[リソースID](api/getting-started.md#resource-identification)の節を参照してください。

### スキーマでは重大な変更をいつ回避し始めますか？

データセットの作成に使用されたことがない、または[[!DNL Real-time Customer Profile]](../profile/home.md)での使用が有効になっている限り、スキーマに対して無効な変更を行うことができます。 スキーマがデータセットの作成に使用されたり、[!DNL Real-time Customer Profile]での使用が有効になると、[スキーマ展開](schema/composition.md#evolution)のルールは、システムによって厳密に適用されます。

### 長いフィールドタイプの最大サイズはどれくらいですか？

長いフィールドタイプは、最大サイズが 53（+1）ビットの整数で、可能な範囲は -9007199254740992 ～ 9007199254740992 です。これは、JSON の JavaScript 実装が長整数を表す方法に制限があるためです。

フィールドの種類について詳しくは、[XDMフィールドの種類制約](./schema/field-constraints.md)のドキュメントを参照してください。

### スキーマの ID を定義するには、どうすればよいですか？

[!DNL Experience Platform]では、解釈されるデータのソースに関係なく、IDを使用して対象（通常は個人）を特定します。 キーフィールドを「ID」としてマークすることで、スキーマで ID が定義されます。IDに一般的に使用されるフィールドには、電子メールアドレス、電話番号、[[!DNL Experience Cloud ID (ECID)]](https://experienceleague.adobe.com/docs/id-service/using/home.html)、CRM ID、その他の一意のIDフィールドがあります。

フィールドは、API またはユーザーインターフェイスを使用して ID としてマークできます。

#### API で ID を定義

API では、ID は ID 記述子を作成することで確立されます。ID 記述子は、スキーマの特定のプロパティが一意の ID であることを示します。

ID 記述子は、/descriptors エンドポイントへの POST リクエストによって作成されます。作成に成功した場合は、HTTP ステータス 201（Created）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

APIでのID記述子の作成について詳しくは、[!DNL Schema Registry]開発者ガイドの[descriptors](api/descriptors.md)のドキュメントを参照してください。

#### UI で ID を定義

スキーマエディタでスキーマを開き、エディタの&#x200B;**[!UICONTROL 構造]**&#x200B;セクションで、IDとしてマークするフィールドを選択します。 右側の「**[!UICONTROL フィールドプロパティ]**」で、「**[!UICONTROL ID]**」チェックボックスを選択します。

UI で ID を管理する方法について詳しくは、スキーマエディターのチュートリアルの [ID フィールドの定義](./tutorials/create-schema-ui.md#identity-field)に関する節を参照してください。

### スキーマにプライマリ ID は必要ですか？

スキーマには 0 または 1 の ID が含まれる場合があるため、プライマリ ID はオプションです。ただし、[!DNL Real-time Customer Profile]でスキーマを使用するには、スキーマにプライマリIDが必要です。 詳しくは、スキーマエディターのチュートリアルの [ID](./tutorials/create-schema-ui.md#identity-field) に関する節を参照してください。

### スキーマを[!DNL Real-time Customer Profile]で使用できるようにするにはどうすればよいですか？

スキーマは、スキーマの`meta:immutableTags`属性にある「和集合」タグを追加することで、[[!DNL Real-time Customer Profile]](../profile/home.md)での使用が有効になります。 [!DNL Profile]でのスキーマの使用は、APIまたはユーザーインターフェイスを使用して行うことができます。

#### APIを使用して[!DNL Profile]の既存のスキーマを有効にする

PATCH リクエストを作成して、スキーマを更新し、値「union」を含む配列として `meta:immutableTags` 属性を追加します。更新が成功すると、応答に更新されたスキーマが表示され、スキーマに和集合タグが含まれるようになります。

APIを使用して[!DNL Real-time Customer Profile]でスキーマを使用できるようにする方法について詳しくは、[!DNL Schema Registry]開発者ガイドの[和集合](./api/unions.md)ドキュメントを参照してください。

#### UIを使用した[!DNL Profile]の既存のスキーマの有効化

[!DNL Experience Platform]で、左側のナビゲーションから「**[!UICONTROL スキーマ]**」を選択し、スキーマのリストから有効にするスキーマの名前を選択します。 次に、**[!UICONTROL スキーマプロパティ]**&#x200B;の下のエディターの右側で、**[!UICONTROL プロファイル]**&#x200B;を選択してオンに切り替えます。


詳しくは、スキーマエディターのチュートリアルの[リアルタイム顧客プロファイルでの使用](./tutorials/create-schema-ui.md#profile)に関する節を参照してください。

### 和集合スキーマを直接編集できますか？

和集合スキーマは読み取り専用であり、システムによって自動的に生成されます。和集合スキーマを直接編集することはできません。和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されたときに、そのクラスに対して作成されます。

XDMの和集合について詳しくは、[!DNL Schema Registry] API開発者ガイドの[和集合](./api/unions.md)の節を参照してください。

### データをスキーマに取り込むには、どのようにデータファイルをフォーマットする必要がありますか？

[!DNL Experience Platform] データファイルは、 [!DNL Parquet] またはJSON形式で受け付けます。これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。データファイル取得に関するベストプラクティスについて詳しくは、「[バッチ取得の概要](../ingestion/home.md)」を参照してください。

## エラーとトラブルシューティング

以下は、[!DNL Schema Registry] APIを使用する際に発生する可能性のあるエラーメッセージのリストです。

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

APIでルックアップパスを構築する方法について詳しくは、[!DNL Schema Registry]開発者ガイドの[コンテナ](./api/getting-started.md#container)と[リソースID](api/getting-started.md#resource-identification)の節を参照してください。

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

このエラーメッセージは、別のリソースで既に使用されているタイトルを持つリソースを作成しようとすると表示されます。タイトルは、すべてのリソースタイプで一意である必要があります。例えば、スキーマが既に使用しているタイトルを持つフィールドグループを作成しようとすると、このエラーが表示されます。

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

このエラーメッセージは、不適切に名前が割り当てられたフィールドを持つ新しいフィールドグループを作成しようとすると表示されます。 IMS組織で定義するフィールドグループは、他の業界やベンダーのリソースとの競合を避けるために、フィールドを`TENANT_ID`に名前空間する必要があります。 フィールドグループに適したデータ構造の詳細な例については、[フィールドグループエンドポイントガイド](./api/field-groups.md#create)を参照してください。


### [!DNL Real-time Customer Profile] エラー

次のエラーメッセージは、[!DNL Real-time Customer Profile]のスキーマを有効にする操作に関連しています。 詳しくは、[!DNL Schema Registry] API開発ガイドの[和集合](./api/unions.md)の節を参照してください。

#### プロファイルデータセットを有効にするには、スキーマが有効である必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "To enable profile datasets the schema should be valid"
}
```

[!DNL Real-time Customer Profile]に対して有効になっていないスキーマのプロファイルデータセットを有効にしようとすると、このエラーメッセージが表示されます。 データセットを有効にする前に、スキーマに和集合タグが含まれていることを確認してください。

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

このエラーメッセージは、[!DNL Profile]のスキーマを有効にしようとしたときに、そのプロパティの1つに参照ID記述子を持たない関係記述子が含まれている場合に表示されます。 このエラーを解決するには、問題のスキーマフィールドに参照 ID 記述子を追加します。

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

[!DNL Profile]で使用する関係記述子を含むスキーマを有効にするには、ソースフィールドとターゲットフィールドの主名前空間の名前空間を同じにする必要があります。 このエラーメッセージは、参照 ID 記述子に対して一致しない名前空間が含まれるスキーマを有効にしようとすると表示されます。この問題を解決するには、宛先スキーマの ID フィールドの `xdm:namespace` 値が、ソースフィールドの参照 ID 記述子にある `xdm:identityNamespace` プロパティの値と一致することを確認してください。

サポートされる ID 名前空間コードのリストについては、「ID 名前空間の概要」の「[標準の名前空間](../identity-service/namespaces.md)」の節を参照してください。

### Accept ヘッダーのエラー

[!DNL Schema Registry] APIのほとんどのGETリクエストでは、応答のフォーマット方法を決定するために、Acceptヘッダーが必要です。 Accept ヘッダーに関連する一般的なエラーのリストを以下に示します。様々な API リクエストの互換性のある Accept ヘッダーのリストについては、[スキーマレジストリ開発者ガイド](api/getting-started.md)の対応する節を参照してください。

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

サポートされるAcceptヘッダーのリストについては、[!DNL Schema Registry]開発ガイドの[Accept header](api/getting-started.md#accept)の節を参照してください。

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
