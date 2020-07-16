---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Experience Data Model(XDM)システムトラブルシューティングガイド
topic: troubleshooting
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 0%

---


# [!DNL Experience Data Model] (XDM)システムトラブルシューティングガイド

このドキュメントでは、 [!DNL Experience Data Model] (XDM)システムに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 Adobe Experience Platform中の他のサービスに関する質問とトラブルシューティングについては、 [Experience Platformトラブルシューティングガイドを参照してください](../landing/troubleshooting.md)。

**[!DNL Experience Data Model](XDM)**は、顧客体験管理のための標準化されたスキーマを定義するオープンソース仕様です。 構築され[!DNL Experience Platform]た方法論、**XDM System **、は、サー[!DNL Experience Data Model]ビスで使用する[!DNL Platform]スキーマを運用します。 に&#x200B;**[!DNL Schema Registry]**は、ユーザーインターフェイスと、内部にアクセスするRESTful APIが用意さ&#x200B;**[!DNL Schema Library]**れてい[!DNL Experience Platform]ます。 See the[XDM documentation](home.md)for more information.

## FAQ

以下は、XDMシステムと [!DNL Schema Registry] APIの使用に関するよくある質問に対する回答のリストです。

### スキーマにフィールドを追加する方法を教えてください。

ミックスインを使用して、スキーマにフィールドを追加できます。 各ミックスインは1つ以上のクラスと互換性があり、それらの互換性のあるクラスの1つを実装する任意のスキーマでミックスインを使用できます。 Adobe Experience Platformは、独自の定義済みフィールドを持つ複数の業界ミックスインを提供しますが、APIまたはユーザーインターフェイスを使用して新しいミックスインを作成することで、独自のフィールドをスキーマに追加できます。

APIでの新しいミックスインの作成について詳しくは、 [API開発者ガイドの「mixin](api/create-mixin.md) ドキュメントの [!DNL Schema Registry] 作成」を参照してください。 UIを使用している場合は、 [スキーマエディタのチュートリアルを参照してください](./tutorials/create-schema-ui.md)。

### ミックスインとデータ型の最適な使用方法は何か。

[ミックスイン](./schema/composition.md#mixin) :スキーマ内の1つ以上のフィールドを定義するコンポーネントです。 ミックスインでは、スキーマの階層でのフィールドの表示が強制されるので、に含まれるすべてのスキーマで同じ構造を示します。 ミックスインは、特定のクラスとのみ互換性があり、その属性で識別され `meta:intendedToExtend` ます。

[データ型は](./schema/composition.md#data-type) 、1つのスキーマに対して1つ以上のフィールドを提供することもできます。 ただし、ミックスインとは異なり、データ型は特定のクラスに制約されません。 これにより、データ型は、クラスが異なる複数のスキーマで再利用可能な、一般的なデータ構造を記述するための、より柔軟なオプションとなります。

### スキーマの一意のIDは何ですか。

すべての [!DNL Schema Registry] リソース(スキーマ、ミックスイン、データ型、クラス)には、参照および参照の目的で一意のIDとして機能するURIがあります。 APIでスキーマを表示する場合、最上位レベルと `$id``meta:altId` 属性にあります。

詳しくは、 [API開発者ガイドの](api/getting-started.md#schema-identification) スキーマID [!DNL Schema Registry] の節を参照してください。

### スキーマ開始によって変更が中断されるのはいつですか。

スキーマに対する変更は、データセットの作成やでの使用が有効になっていない限り行うことができ [!DNL Real-time Customer Profile](../profile/home.md)ます。 スキーマがデータセットの作成に使用されたり、での使用が有効になったりすると [!DNL Real-time Customer Profile]、 [スキーマ展開のルールはシステムによって厳密に適用されます](schema/composition.md#evolution) 。

### 長いフィールドの種類の最大サイズはどのくらいか

longフィールド型は、最大サイズが53(+1)ビットの整数で、-9007199254740992 ～ 9007199254740992の範囲で指定します。 これは、JSONのJavaScript実装がlong整数を表す方法に制限があるためです。

フィールドの種類について詳しくは、 [API開発者ガイドのXDMフィールドの種類の](api/appendix.md#field-types)[!DNL Schema Registry] 定義に関する節を参照してください。

### スキーマのIDを定義する方法を教えてください。

で [!DNL Experience Platform]は、IDは、解釈されるデータのソースに関係なく、対象（通常は個人）を識別するために使用されます。 キーフィールドを「ID」としてマークすることで、スキーマ内で定義されます。 IDに一般的に使用されるフィールドには、電子メールアドレス、電話番号、 [!DNL Experience Cloud ID (ECID)](https://docs.adobe.com/content/help/ja-JP/id-service/using/home.html)CRM ID、その他の一意のIDフィールドがあります。

フィールドは、APIまたはユーザーインターフェイスを使用してIDとしてマークできます。

#### APIでのIDの定義

APIでは、ID記述子を作成することでIDが確立されます。 ID記述子は、スキーマの特定のプロパティが一意の識別子であることを伝えます。

ID記述子は、/descriptorsエンドポイントへのPOST要求によって作成されます。 成功した場合は、HTTPステータス201 （作成済み）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

APIでのID記述子の作成について詳しくは、 [開発者ガイドの「](api/descriptors.md) 記述子に関するドキュメント [!DNL Schema Registry] 」の節を参照してください。

#### UIでのIDの定義

スキーマエディタでスキーマを開き、エディタの **[!UICONTROL 構造]** (Structure)セクションで、IDとしてマークするフィールドをクリックします。 右側の「 **[!UICONTROL フィールドプロパティ]** 」で、「 **[!UICONTROL ID]** 」チェックボックスをクリックします。

UIでのIDの管理について詳しくは、「スキーマエディターのチュートリアル」の「IDフィールドの [定義](./tutorials/create-schema-ui.md#identity-field) 」の節を参照してください。

### スキーマにプライマリIDが必要か

プライマリのIDはオプションです。スキーマのIDは0または1に設定できるためです。 ただし、での使用を有効にするには、スキーマにプライマリIDが存在する必要があり [!DNL Real-time Customer Profile]ます。 詳しくは、スキーマエディタのチュートリアルの [identity](./tutorials/create-schema-ui.md#identity-field) セクションを参照してください。

### で使用するスキーマを有効にする方法を教えてく [!DNL Real-time Customer Profile]ださい。

スキーマは、スキーマの [!DNL Real-time Customer Profile](../profile/home.md)`meta:immutableTags` 属性にある「和集合」タグを追加することで、での使用が有効になります。 でのスキーマの使用を有効にするに [!DNL Profile] は、APIまたはユーザーインターフェイスを使用します。

#### APIを使用するための既存のスキーマ [!DNL Profile] の有効化

スキーマを更新し、値「和集合」を含む配列として `meta:immutableTags` 属性を追加するために、PATCHリクエストを実行します。 更新が成功すると、応答に更新されたスキーマが表示され、和集合タグが含まれます。

APIを使用してでのスキーマの使用を有効にする方法について詳し [!DNL Real-time Customer Profile]くは、 [開発者ガイドの](./api/unions.md) 和集合 [!DNL Schema Registry] ドキュメントを参照してください。

#### UIを使用するための既存のスキーマ [!DNL Profile] の有効化

で、左 [!DNL Experience Platform]のナビゲーションで **** スキーマをクリックし、スキーマのリストから有効にするスキーマの名前を選択します。 次に、エディタの右側の[ **[!UICONTROL スキーマプロパティ]**]で、[ **[!UICONTROL プロファイル]** ]をクリックしてオンに切り替えます。


詳しくは、 [スキーマエディタのチュートリアルで、「リアルタイム顧客プロファイルで](./tutorials/create-schema-ui.md#profile) の [!UICONTROL 使用] 」の節を参照してください。

### 和集合スキーマを直接編集できますか。

和集合スキーマは読み取り専用で、システムによって自動的に生成されます。 直接編集することはできません。 和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されると、そのクラスに対して作成されます。

XDMの和集合について詳しくは、 [API開発者ガイドの](./api/unions.md) 和集合の節を参照してください [!DNL Schema Registry] 。

### データをスキーマに取り込むために、データファイルをどのようにフォーマットする必要がありますか。

[!DNL Experience Platform] は、 [!DNL Parquet] またはJSON形式のデータファイルを受け入れます。 これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。 データファイル取り込みのベストプラクティスについて詳しくは、 [バッチ取り込みの概要を参照してください](../ingestion/home.md)。

## エラーとトラブルシューティング

以下は、 [!DNL Schema Registry] APIを使用する際に発生する可能性があるエラーメッセージのリストです。

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

このエラーは、システムが特定のリソースを見つけられなかった場合に表示されます。 リソースが削除されたか、API呼び出しのパスが無効です。 もう一度やり直す前に、API呼び出しの有効なパスが入力されていることを確認してください。 リソースの正しいIDが入力されていること、およびパスが適切なコンテナ（グローバルまたはテナント）と正しく名前が付けられていることを確認してください。

APIでルックアップパスを作成する方法について詳しくは、 [開発者ガイドの](./api/getting-started.md#container) コンテナ [と](api/getting-started.md#schema-identification) スキーマの識別 [!DNL Schema Registry] に関する節を参照してください。

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

このエラーメッセージは、別のリソースで既に使用されているタイトルを持つリソースを作成しようとすると表示されます。 タイトルは、すべてのリソースの種類で一意である必要があります。 例えば、スキーマが既に使用しているタイトルを持つミックスインを作成しようとすると、このエラーが表示されます。

### カスタムフィールドには最上位レベルのフィールドを使用する必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "For custom fields, you must use a top level field named _{TENANT_ID}
       and all the other fields must be defined under it"
}
```

このエラーメッセージは、不適切に名前が割り当てられたフィールドを使用して新しいmixinを作成しようとすると表示されます。 IMS組織で定義されるミックスインは、他の業界やベンダーのリソースとの競合を避けるために、フィールド `TENANT_ID` をと名前空間する必要があります。 ミックスインに適したデータ構造の詳細な例は、 [API開発者ガイドのmixinの](api/create-mixin.md)[!DNL Schema Registry] 作成に関するドキュメントの節を参照してください。


### [!DNL Real-time Customer Profile] エラー

次のエラーメッセージは、のスキーマの有効化に関連する操作に関連してい [!DNL Real-time Customer Profile]ます。 詳しくは、 [API開発者ガイドの](./api/unions.md) 和集合 [!DNL Schema Registry] の節を参照してください。

#### プロファイルデータセットを有効にするには、スキーマが有効である必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "To enable profile datasets the schema should be valid"
}
```

このエラーメッセージは、有効になっていないスキーマのプロファイルデータセットを有効にしようとすると表示され [!DNL Real-time Customer Profile]ます。 データセットを有効にする前に、スキーマに和集合タグが含まれていることを確認してください。

#### 参照ID記述子が必要です

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

このエラーメッセージは、のスキーマを有効にしようとしたときに、そのプロパティの1つに参照ID記述子を持たない関係記述子が含まれ [!DNL Profile] ている場合に表示されます。 このエラー追加を解決するために、問題のスキーマフィールドへの参照ID記述子です。

#### 参照ID記述子フィールドと宛先スキーマの名前空間は、

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

で使用する関係記述子を含むスキーマを有効にするに [!DNL Profile]は、ソースフィールドの名前空間とターゲットフィールドのプライマリ名前空間を同じにする必要があります。 このエラーメッセージは、参照ID記述子に対して、一致しない名前空間を含むスキーマを有効にしようとすると表示されます。 この問題を解決するには、宛先スキーマのIDフィールドの `xdm:namespace` 値が、ソースフィールドの参照ID記述子の `xdm:identityNamespace` プロパティの値と一致することを確認してください。

サポートされるID名前空間コードのリストについては、ID名前空間の概要の [標準名前空間](../identity-service/namespaces.md) の節を参照してください。

### ヘッダーエラーの受け入れ

システムが応答のフォーマット方法を決定するためには、 [!DNL Schema Registry] APIのほとんどのGETリクエストにAcceptヘッダーが必要です。 次に、Acceptヘッダーに関連する一般的なエラーのリストを示します。 互換性のある様々なAPIリクエストのAcceptヘッダーのリストについては、『 [スキーマレジストリ開発者ガイド](api/getting-started.md)』の対応するセクションを参照してください。

#### 受け入れヘッダパラメータが必要です

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Accept header parameter is required"
}
```

このエラーメッセージは、APIリクエストにAcceptヘッダーがない場合に表示されます。 再試行する前に、Acceptヘッダーが含まれていることを確認してください。

#### 不明な受け付けメディアが指定されました

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Unknown Accept media supplied: xed+json"
}
```

このエラーメッセージは、Acceptヘッダーが無効な場合に表示されます。 再試行する前に、作成しようとしているAPIリクエストと互換性のある「承認」ヘッダーが正しく入力されていることを確認してください。

#### 不明な受け入れ形式が使用可能です

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Unknown Accept format available "
}
```

このエラーメッセージは、記述子を検索する際にAcceptヘッダーが正しく指定されていない場合に表示されます。 もう一度やり直す前に、 [サポートされている記述子のAcceptヘッダーの1つを正しく入力していることを確認し](./api/descriptors.md) 、

#### Acceptヘッダーでバージョンを指定する必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "version must be supplied in the accept header. Example: 
      application/vnd.adobe.xed-full-notext+json; version=1"
}
```

このエラーメッセージは、バージョン番号がAcceptヘッダーに含まれていない場合に表示されます。 スキーマなどの特定の要素では、個々のインスタンスを検索する際にバージョンを指定する必要があります。 バージョン番号を含むAcceptヘッダーは、次のようになります。

```plaintext
application/vnd.adobe.xed+json; version=1
```

サポートされるAcceptヘッダーのリストについては、 [開発者ガイドのAcceptヘッダー](api/getting-started.md#accept)[!DNL Schema Registry] セクションを参照してください。

#### Acceptヘッダーでバージョンを指定することはできません

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "version must not be supplied in the accept header. Example: 
      application/vnd.adobe.xed-full+json"
}
```

GET (Accept header when listing (GET)リソースをリストする際に、Acceptヘッダーにバージョンを含めようとすると、このエラーが表示されます。 バージョンは、単一のリソースに対してルックアップ要求を試行する場合にのみ必要です。 エラーを解決するには、Acceptヘッダーからバージョンを削除します。