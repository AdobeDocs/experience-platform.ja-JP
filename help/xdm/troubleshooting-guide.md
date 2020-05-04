---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Experience Data Model(XDM)システムトラブルシューティングガイド
topic: troubleshooting
translation-type: tm+mt
source-git-commit: 14cd3d17c7d9ba602d02925abddec9e0b246a8c8

---


# Experience Data Model(XDM)システムトラブルシューティングガイド

このドキュメントでは、エクスペリエンスデータモデル(XDM)システムに関するよくある質問と、一般的なエラーのトラブルシューティングガイドに回答します。 Adobe Experience Platformの他のサービスに関するご質問やトラブルシューティングについては、 [Experience Platformトラブルシューティングガイドを参照してください](../landing/troubleshooting.md)。

**Experience Data Model(XDM)** は、顧客体験管理の標準化されたスキーマを定義するオープンソース仕様です。 Experience Platformが構築される方法論、 **XDM System**、はPlatform Servicesで使用するExperience Data Modelスキーマを運用します。 **スキーマレジストリは** 、Experience Platform **内の** スキーマライブラリにアクセスするためのユーザーインターフェイスとRESTful APIを提供します。 See the [XDM documentation](home.md) for more information.

## FAQ

XDMシステムとスキーマレジストリAPIの使用に関するよくある質問に対する回答をリストで示します。

### スキーマにフィールドを追加する方法を教えてください。

ミックスインを使用して、スキーマにフィールドを追加できます。 各ミックスインは1つ以上のクラスと互換性があり、それらの互換性のあるクラスの1つを実装する任意のスキーマでミックスインを使用できます。 Adobe Experience Platformは、独自の定義済みフィールドを持つ複数の業界ミックスインを提供しますが、APIまたはユーザーインターフェイスを使用して新しいミックスインを作成することで、独自のフィールドをスキーマに追加できます。

APIでの新しいミックスインの作成について詳しくは、スキーマレジストリAPI開発者ガイドのmixin [ドキュメントの](api/create-mixin.md) 作成を参照してください。 UIを使用している場合は、 [スキーマエディタのチュートリアルを参照してください](./tutorials/create-schema-ui.md)。

### ミックスインとデータ型の最適な使用方法は何か。

[ミックスイン](./schema/composition.md#mixin) :スキーマ内の1つ以上のフィールドを定義するコンポーネントです。 ミックスインでは、スキーマの階層でのフィールドの表示が強制されるので、に含まれるすべてのスキーマで同じ構造を示します。 ミックスインは、特定のクラスとのみ互換性があり、その属性で識別され `meta:intendedToExtend` ます。

[データ型は](./schema/composition.md#data-type) 、1つのスキーマに対して1つ以上のフィールドを提供することもできます。 ただし、ミックスインとは異なり、データ型は特定のクラスに制約されません。 これにより、データ型は、クラスが異なる複数のスキーマで再利用可能な、一般的なデータ構造を記述するための、より柔軟なオプションとなります。

### スキーマの一意のIDは何ですか。

すべてのスキーマレジストリリソース(スキーマ、ミックスイン、データ型、クラス)には、参照および参照の目的で一意のIDとして機能するURIがあります。 APIでスキーマを表示する場合、最上位レベルと `$id``meta:altId` 属性にあります。

詳しくは、スキーマレジストリAPI開発者ガイドの [スキーマID](api/getting-started.md#schema-identification) (ID)の節を参照してください。

### スキーマ開始によって変更が中断されるのはいつですか。

スキーマに対する変更は、データセットの作成に使用されたことがない限り、または [リアルタイム顧客プロファイルでの使用が有効になっている限り行うことができます](../profile/home.md)。 スキーマがデータセットの作成に使用されたり、リアルタイムのお客様プロファイルでの使用が可能になったりすると、 [スキーマ展開のルール](schema/composition.md#evolution) はシステムによって厳密に適用されます。

### 長いフィールドの種類の最大サイズはどのくらいか

longフィールド型は、最大サイズが53(+1)ビットの整数で、-9007199254740992 ～ 9007199254740992の範囲で指定します。 これは、JSONのJavaScript実装がlong整数を表す方法に制限があるためです。

フィールドの種類について詳しくは、スキーマレジストリAPI開発者ガイドの [XDMフィールドの種類の](api/appendix.md#field-types) 定義に関する節を参照してください。

### スキーマのIDを定義する方法を教えてください。

エクスペリエンスプラットフォームでは、解釈されるデータのソースに関係なく、IDを使用して、対象（通常は個人）を識別します。 キーフィールドを「ID」としてマークすることで、スキーマ内で定義されます。 IDに一般的に使用されるフィールドには、電子メールアドレス、電話番号、 [Experience Cloud ID(ECID)](https://docs.adobe.com/content/help/ja-JP/id-service/using/home.html)、CRM ID、その他の一意のIDフィールドがあります。

フィールドは、APIまたはユーザーインターフェイスを使用してIDとしてマークできます。

#### APIでのIDの定義

APIでは、ID記述子を作成することでIDが確立されます。 ID記述子は、スキーマの特定のプロパティが一意の識別子であることを伝えます。

ID記述子は、/descriptorsエンドポイントへのPOST要求によって作成されます。 成功した場合は、HTTPステータス201 （作成済み）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

APIでのID記述子の作成について詳しくは、スキーマレジストリ開発者ガイドの [記述子に関するドキュメントの節を参照してください](api/descriptors.md) 。

#### UIでのIDの定義

スキーマエディタでスキーマを開き、エディタの **構造** (Structure)セクションで、IDとしてマークするフィールドをクリックします。 右側の「 **フィールドプロパティ** 」で、「 **ID** 」チェックボックスをクリックします。

UIでのIDの管理について詳しくは、「スキーマエディターのチュートリアル」の「IDフィールドの [定義](./tutorials/create-schema-ui.md#identity-field) 」の節を参照してください。

### スキーマにプライマリIDが必要か

プライマリIDはオプションです。スキーマには0または1が含まれる場合があるためです。 ただし、スキーマをリアルタイム顧客プロファイルで使用するには、スキーマにプライマリIDが存在する必要があります。 詳しくは、スキーマエディタのチュートリアルの [identity](./tutorials/create-schema-ui.md#identity-field) セクションを参照してください。

### スキーマをリアルタイム顧客プロファイルで使用できるようにする方法を教えてください。

スキーマは、スキーマの [属性に「和集合」タグを追加することで、](../profile/home.md) リアルタイム顧客プロファイルでの使用が有効になります `meta:immutableTags` 。 プロファイルでスキーマを使用できるようにするには、APIまたはユーザーインターフェイスを使用します。

#### APIを使用した既存のスキーマのプロファイルの有効化

スキーマを更新し、値「和集合」を含む配列として `meta:immutableTags` 属性を追加するために、PATCHリクエストを実行します。 更新が成功すると、応答に更新されたスキーマが表示され、和集合タグが含まれます。

APIを使用してスキーマをリアルタイム顧客プロファイルで使用できるようにする方法について詳しくは、『スキーマレジストリ開発者ガイド』の [和集合](./api/unions.md) ドキュメントを参照してください。

#### UIを使用した既存のスキーマのプロファイルの有効化

Experience Platformで、左側のナビゲーションで **スキーマ** をクリックし、スキーマのリストから有効にするスキーマの名前を選択します。 次に、エディタの右側の[ **スキーマプロパティ**]で、[ **プロファイル** ]をクリックしてオンに切り替えます。


詳しくは、スキーマエディタのチュートリアルの「リアルタイム顧客プロファイルで [の](./tutorials/create-schema-ui.md#profile) 使用」の節を参照してください。

### 和集合スキーマを直接編集できますか。

和集合スキーマは読み取り専用で、システムによって自動的に生成されます。 直接編集することはできません。 和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されると、そのクラスに対して作成されます。

XDMの和集合について詳しくは、『スキーマレジストリAPI開発者ガイド』の [和集合の節を参照してください](./api/unions.md) 。

### データをスキーマに取り込むために、データファイルをどのようにフォーマットする必要がありますか。

Experience Platformは、Parket形式またはJSON形式のデータファイルを受け付けます。 これらのファイルの内容は、データセットが参照するスキーマに準拠している必要があります。 データファイル取り込みのベストプラクティスについて詳しくは、 [バッチ取り込みの概要を参照してください](../ingestion/home.md)。

## エラーとトラブルシューティング

以下は、スキーマレジストリAPIを使用する際に発生する可能性があるエラーメッセージのリストです。

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

APIでルックアップパスを作成する方法について詳しくは、『スキーマレジストリ開発者ガイド』の [コンテナ](./api/getting-started.md#container) と [スキーマの識別](api/getting-started.md#schema-identification) に関する節を参照してください。

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

このエラーメッセージは、不適切に名前が割り当てられたフィールドを使用して新しいmixinを作成しようとすると表示されます。 IMS組織で定義されるミックスインは、他の業界やベンダーのリソースとの競合を避けるために、フィールド `TENANT_ID` をと名前空間する必要があります。 ミックスインに適したデータ構造の詳細な例は、スキーマレジストリAPI開発者ガイドのmixinの [作成に関するドキュメントの節を参照してください](api/create-mixin.md) 。


### 顧客プロファイルエラーのリアルタイム処理

次のエラーメッセージは、リアルタイム顧客プロファイルのスキーマの有効化に関連する操作に関連しています。 詳しくは、スキーマレジストリAPI開発者ガイドの [和集合](./api/unions.md) （英語）の節を参照してください。

#### プロファイルデータセットを有効にするには、スキーマが有効である必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "To enable profile datasets the schema should be valid"
}
```

このエラーメッセージは、リアルタイム顧客プロファイルが有効になっていないスキーマのプロファイルデータセットを有効にしようとすると表示されます。 データセットを有効にする前に、スキーマに和集合タグが含まれていることを確認してください。

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

このエラーメッセージは、プロファイル用のスキーマを有効にしようとしたときに、そのプロパティの1つに参照ID記述子を持たない関係記述子が含まれている場合に表示されます。 このエラー追加を解決するために、問題のスキーマフィールドへの参照ID記述子です。

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

プロファイルで使用する関係記述子を含むスキーマを有効にするには、ソースフィールドの名前空間とターゲットフィールドの主名前空間が同じである必要があります。 このエラーメッセージは、参照ID記述子に対して、一致しない名前空間を含むスキーマを有効にしようとすると表示されます。 この問題を解決するには、宛先スキーマのIDフィールドの `xdm:namespace` 値が、ソースフィールドの参照ID記述子の `xdm:identityNamespace` プロパティの値と一致することを確認してください。

サポートされるID名前空間コードのリストについては、ID名前空間の概要の [標準名前空間](../identity-service/namespaces.md) の節を参照してください。

### ヘッダーエラーの受け入れ

スキーマレジストリAPIのほとんどのGETリクエストでは、応答のフォーマット方法を決定するために、Acceptヘッダーが必要です。 次に、Acceptヘッダーに関連する一般的なエラーのリストを示します。 互換性のある様々なAPIリクエストのAcceptヘッダーのリストについては、『 [スキーマレジストリ開発者ガイド](api/getting-started.md)』の対応するセクションを参照してください。

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

サポートされるAcceptヘッダーのリストについては、『スキーマレジストリ開発ガイド』の「 [Accept header](api/getting-started.md#accept) 」セクションを参照してください。

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