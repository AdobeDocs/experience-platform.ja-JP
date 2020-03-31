---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Experience Data Model(XDM)システムトラブルシューティングガイド
topic: troubleshooting
translation-type: tm+mt
source-git-commit: f7c87cc86bfc5017ec5c712d05e39be5c14a7147

---


# Experience Data Model(XDM)システムトラブルシューティングガイド

このドキュメントでは、エクスペリエンスデータモデル(XDM)システムに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 Adobe Experience Platformの他のサービスに関するご質問やトラブルシューティングについては、 [Experience Platformトラブルシューティングガイドを参照してください](../landing/troubleshooting.md)。

**Experience Data Model(XDM)は、顧客体験管理のための標準化されたスキーマを定義するオープンソース仕様です。** Experience Platformが構築される方法、 **XDM System**、はPlatform Servicesで使用するExperience Data Modelスキーマを運用します。 **スキーマレジストリは****** 、Experience Platform内のスキーマライブラリにアクセスするためのユーザーインターフェイスとRESTful APIを提供します。 See the [XDM documentation](home.md) for more information.

## FAQ

次に、XDMシステムとスキーマレジストリAPIの使用に関するよくある質問への回答を示します。

### フィールドをスキーマに追加する方法

ミックスインを使用して、スキーマにフィールドを追加できます。 各ミックスインは1つ以上のクラスと互換性があり、それらの互換性のあるクラスの1つを実装する任意のスキーマでミックスインを使用できます。 Adobe Experience Platformは、独自の定義済みフィールドを含む複数の業界ミックスインを提供しますが、APIまたはユーザーインターフェイスを使用して新しいミックスインを作成することで、独自のフィールドをスキーマに追加できます。

APIでの新しいミックスインの作成について詳しくは、スキーマレジストリAPI [開発者ガイドの「mixin](api/create-mixin.md) ドキュメントの作成」を参照してください。 UIを使用している場合は、「 [スキーマエディタ](./tutorials/create-schema-ui.md)」

### ミックスインとデータタイプの最適な用途は何か。

[ミックスイン](./schema/composition.md#mixin) は、スキーマ内の1つ以上のフィールドを定義するコンポーネントです。 ミックスインは、スキーマの階層でフィールドがどのように表示されるかを強制するので、どのスキーマにも同じ構造を持ちます。 ミックスインは、属性で識別される特定のクラスとのみ互換性があ `meta:intendedToExtend` ります。

[データ型は](./schema/composition.md#data-type) 、1つ以上のフィールドをスキーマに提供します。 ただし、ミックスインとは異なり、データ型は特定のクラスに制限されません。 これにより、データ型は、潜在的に異なるクラスを持つ複数のスキーマで再利用可能な、一般的なデータ構造を記述する、より柔軟なオプションとなります。

### スキーマの固有ID

すべてのスキーマレジストリリソース(スキーマ、ミックスイン、データ型、クラス)には、参照および参照の目的で一意のIDとして機能するURIが含まれています。 APIでスキーマを表示すると、最上位レベルと属性で見つか `$id` りま `meta:altId` す。

詳しくは、スキーマレジストリAPI開 [発者ガイド](api/getting-started.md#schema-identification) （英語のみ）のスキーマ識別の節を参照してください。

### 変更の中断をスキーマ開始が妨げるのはいつですか。

スキーマの変更は、データセットの作成に使用されたことがない限り、またはリアルタイム顧客プロファイルでの使用が有効になっている限り、 [行うことができます](../profile/home.md)。 スキーマがデータセットの作成に使用されたり、リアルタイム顧客プロファイルでの使用が可能になったりすると、 [スキーマ展開のルールは](schema/composition.md#evolution) 、システムによって厳密に適用されます。

### 長いフィールドタイプの最大サイズはどれくらいか。

長いフィールドタイプは、最大サイズが53(+1)ビットの整数で、-9007199254740992 ～ 9007199254740992の範囲で指定します。 これは、JSONのJavaScript実装が長い整数を表す方法に制限があるためです。

フィールドの種類について詳しくは、スキーマレジストリAPI [開発者ガイドの](api/appendix.md#field-types) 「XDMフィールドの種類の定義」の節を参照してください。

### 自分のスキーマのIDを定義する方法

エクスペリエンスプラットフォームでは、IDは、解釈されるデータのソースに関係なく、対象（通常は個人）を識別するために使用されます。 キーフィールドを「ID」としてスキーマにマークすることで、ユーザー内で定義されます。 IDに一般的に使用されるフィールドには、電子メールアドレス、電話番号、 [Experience Cloud ID(ECID)](https://marketing.adobe.com/resources/help/en_US/mcvid/)、CRM ID、その他の一意のIDフィールドがあります。

フィールドは、APIまたはユーザーインターフェイスを使用してIDとしてマークできます。

#### APIでのIDの定義

APIでは、IDはID記述子を作成することで確立されます。 ID記述子は、特定のプロパティが一意の識別子であることをスキーマに伝えるシグナルです。

ID記述子は、/descriptorsエンドポイントへのPOST要求によって作成されます。 成功した場合は、HTTPステータス201（作成済み）と、新しい記述子の詳細を含む応答オブジェクトを受け取ります。

APIでのID記述子の作成について詳しくは、スキーマレジストリ開発ガイドの記述子に関する [ドキュメントの節](api/descriptors.md) を参照してください。

#### UIでのIDの定義

スキーマエディタでスキーマを開き、IDとしてマークするエディタの **Structure** （構造）セクションのフィールドをクリックします。 右側 **の「フィールド** ・プロパティ」で、「ID」チェックボックスをクリッ **クします** 。

UIでのIDの管理について詳しくは、「IDフィールドの定義」の節を参照し [てください](./tutorials/create-schema-ui.md#identity-field) 。詳しくは、スキーマエディタのチュートリアルを参照してください。

### 自分のスキーマにプライマリIDは必要ですか？

プライマリIDはオプションです。スキーマには0または1が含まれる場合があります。 ただし、スキーマがリアルタイム顧客スキーマで使用できるようにするには、その顧客にプライマリIDが必要です。 詳しくは、 [スキーマ](./tutorials/create-schema-ui.md#identity-field) ・エディタのチュートリアルの「identity」の節を参照してください。

### リアルタイム顧客プロファイルでスキーマを使用できるようにする方法を教えてください。

スキーマは、プロファイルの属性に「 [和集合](../profile/home.md) 」タグを追加することで、リアルタイム顧客スキーマでの使用を `meta:immutableTags` 有効にできます。 スキーマでの使用を有効にするプロファイルは、APIまたはユーザーインターフェイスを使用して行うことができます。

#### APIを使用した既存のスキーマのプロファイルの有効化

PATCHリクエストを作成して、スキーマを更新し、値「 `meta:immutableTags` 和集合」を含む配列として属性を追加します。 更新が成功すると、応答に更新されたスキーマが表示され、この中に和集合タグが含まれます。

APIを使用してスキーマをリアルタイム顧客プロファイルで使用できるようにする方法について詳しくは、スキーマレジストリ開発者ガイドの [和集合](./api/unions.md) ドキュメントを参照してください。

#### UIを使用した既存のスキーマのプロファイルの有効化

Experience Platformで、左側のナビゲーシ **ョンで** 「スキーマ」をクリックし、スキーマのリストから有効にするスキーマの名前を選択します。 次に、エディタの右側の[ **スキーマのプロパティ**]で、 **プロファイルをクリック** してオンに切り替えます。


詳しくは、「スキーマエディタのチュートリアル」の「リ [アルタイム顧客プロファイルでの使用](./tutorials/create-schema-ui.md#profile) 」の節を参照してください。

### 和集合スキーマを直接編集できる

和集合スキーマは読み取り専用で、システムによって自動的に生成されます。 直接編集することはできません。 和集合スキーマは、特定のクラスを実装するスキーマに「和集合」タグが追加されると、そのクラスに対して作成されます。

XDMの和集合について詳しくは、和集合レジストリAPI開 [発者](./api/unions.md) ガイドのスキーマの節を参照してください。

### データをデータファイルに取り込むために、どのようにデータファイルをフォーマットする必要がありますか。スキーマ

エクスペリエンスプラットフォームは、ParketまたはJSON形式のデータファイルを受け付けます。 これらのファイルの内容は、データセットが参照するスキーマーに準拠している必要があります。 データファイルの取り込みのベストプラクティスについて詳しくは、バッチ取り込みの概 [要を参照してくださ](../ingestion/home.md)い。

## エラーとトラブルシューティング

次に、スキーマレジストリAPIを使用する際に発生する可能性のあるエラーメッセージのリストを示します。

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

このエラーは、システムが特定のリソースを見つけられなかった場合に表示されます。 リソースが削除されたか、API呼び出しのパスが無効です。 再試行する前に、API呼び出しの有効なパスを入力したことを確認してください。 リソースの正しいIDを入力したこと、およびパスが適切なコンテナ（グローバルまたはテナント）と適切に名前が付けられていることを確認します。

APIでの参照パスの作成について詳しくは、『 [コンテナ](./api/getting-started.md#container) ・レジストリ開発者ガイド』の [「スキーマとスキーマの識別](api/getting-started.md#schema-identification) 」の節を参照してください。

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

このエラーメッセージは、別のリソースで既に使用されているタイトルを持つリソースを作成しようとすると表示されます。 タイトルは、すべてのリソースタイプで一意である必要があります。 例えば、スキーマが既に使用しているタイトルを持つミックスインを作成しようとすると、このエラーが表示されます。

### カスタムフィールドでは、トップレベルのフィールドを使用する必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "For custom fields, you must use a top level field named _{TENANT_ID}
       and all the other fields must be defined under it"
}
```

このエラーメッセージは、不適切な名前のフィールドを含む新しいミックスインを作成しようとすると表示されます。 IMS組織で定義されたミックスインは、他の業界やベンダーのリソースとの競合を避けるた `TENANT_ID` めに、フィールドをに名前空間する必要があります。 ミックスインに適したデータ構造の詳細な例は、スキーマレジストリAPI開発ガイドの [mixinの作成に関するドキュメント](api/create-mixin.md) の節を参照してください。


### リアルタイム顧客プロファイルエラー

次のエラーメッセージは、リアルタイム顧客のプロファイルのスキーマを有効にする操作に関連しています。 詳しくは、 [和集合](./api/unions.md) (スキーマレジストリAPI開発者ガイド)の「開発者」の節を参照してください。

#### プロファイルデータセットを有効にするには、スキーマが有効である必要があります

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "To enable profile datasets the schema should be valid"
}
```

このエラーメッセージは、リアルタイム顧客プロファイルが有効になっていないスキーマの顧客データセットを有効にしようとすると表示されます。 データセットを有効にする前に、スキーマに和集合タグが含まれていることを確認してください。

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

このエラーメッセージは、プロファイル用のスキーマを有効にしようとしたときに、そのプロパティの1つに参照ID記述子のない関係記述子が含まれている場合に表示されます。 このエ追加ラーを解決するための、問題のスキーマフィールドへの参照ID記述子です。

#### 参照ID記述子フィールドの名前空間と宛先スキーマは、

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

プロファイルで使用する関係記述子を含むスキーマを有効にするには、ソースフィールドの名前空間とターゲットフィールドのプライマリ名前空間が同じである必要があります。 このエラーメッセージは、参照ID記述子に対して一致しないスキーマを含む名前空間を有効にしようとすると表示されます。 この問題を解決す `xdm:namespace` るには、宛先スキーマのIDフィールドの値が、ソースフィールドの参照ID記 `xdm:identityNamespace` 述子にあるプロパティの値と一致することを確認してください。

サポートされるID名前空間コードのリストについては、ID名前空間の概要の標準 [名前空間](../identity-service/namespaces.md) の節を参照してください。

### ヘッダーエラーの受け入れ

スキーマレジストリAPIのほとんどのGET要求では、応答の形式を決定するためにAcceptヘッダが必要です。 次に、Acceptヘッダーに関連する一般的なリストを示します。 様々なAPIリクエストに対する互換性のあるAcceptヘッダのリストについては、『 [スキーマレジストリ開発者ガイド』の対応する節を参照してください](api/getting-started.md)。

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

#### 不明な受け入れメディアが指定されました

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Unknown Accept media supplied: xed+json"
}
```

このエラーメッセージは、Acceptヘッダーが無効な場合に表示されます。 再試行する前に、作成しようとしているAPIリクエストと互換性のあるAcceptヘッダーが正しく入力されていることを確認してください。

#### 不明な受け入れ形式が使用可能です

```json
{
    "type": "/placeholder/type/uri",
    "status": 406,
    "title": "NotAcceptableError",
    "detail": "Unknown Accept format available "
}
```

このエラーメッセージは、記述子を検索する際に、Acceptヘッダーが正しく指定されていない場合に表示されます。 再試行する前に、サポートされている記述子の受け入れ [ヘッダーの1つを正しく入力し](./api/descriptors.md) 、正しく入力してください。

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

このエラーメッセージは、バージョン番号がAcceptヘッダーに含まれていない場合に表示されます。 特定の要素(スキーマなど)は、個々のインスタンスを検索する際にバージョンを指定する必要があります。 バージョン番号を含むAcceptヘッダーは、次のようになります。

```plaintext
application/vnd.adobe.xed+json; version=1
```

サポートされるAcceptヘッダのリストについては、『スキーマレジストリ開 [発者ガイド](api/getting-started.md#accept) 』の「Accept header」の節を参照してください。

#### Acceptヘッダーにバージョンを指定しないでください

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "version must not be supplied in the accept header. Example: 
      application/vnd.adobe.xed-full+json"
}
```

リソースのリスト(GET)時にAcceptヘッダーにバージョンを含めようとすると、このエラーが表示されます。 バージョンは、単一のリソースに対してルックアップ要求を試行する場合にのみ必要です。 エラーを解決するには、Acceptヘッダーからバージョンを削除します。