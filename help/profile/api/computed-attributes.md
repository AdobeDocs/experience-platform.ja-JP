---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: d0ccaa5511375253a2eca8f1235c2f953b734709

---


# （アルファ）計算済み属性

>[!IMPORTANT]
>このドキュメントで説明されている計算済み属性機能は、現在アルファ値で表示され、すべてのユーザが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算および式に基づいてフィールドの値を自動的に計算できます。 計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードと集計の値をイベントできます。

各計算済み属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式(「rule」)が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーション開封数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。

このガイドは、Adobe Experience Platform内の計算済み属性をより深く理解するのに役立ち、エンドポイントを使用して基本的なCRUD操作を実行するためのサンプルAPI呼び出しを含 `/config/computedAttributes` みます。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 続行する前に、リアルタイムのお客様 [プロファイル開発ガイドを参照してください](getting-started.md)。

特に、プロファイル開発ガ [イドの](getting-started.md) 「はじめに」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIを正常に呼び出すために必要なヘッダーに関する重要な情報が含まれています。

## 計算済み属性について

Adobe Experience Platformを使用すると、複数のソースからデータを簡単に読み込んで結合し、リアルタイムの顧客プロファイルを生成できます。 各プロファイルには、顧客の連絡先情報、好み、購入履歴など、顧客に関する重要な情報が含まれ、顧客の360度の表示を提供します。

プロファイルで収集された情報の一部は、データフィールドを直接読み取る場合（「名」など）に理解しやすくなり、他のデータは、複数の計算を行うか、他のフィールドや値に依存して情報を生成する必要があります（「全期間購入合計」など）。 このデータを一目で理解しやすくするために、プラットフォームでは、これらの参照と計算を自動的に実行する計算済み属性 **** （計算済み属性）を作成し、適切なフィールドに値を返すことができます。

計算済み属性には、入力データに対して動作し、結果の値をプロファイルの属性またはイベントに格納する式（「ルール」）の作成が含まれます。 式は複数の異なる方法で定義でき、受信イベントのみ、受信イベントとプロファイルデータ、または受信イベント、プロファイルデータ、履歴イベントを評価するルールを指定できます。

### 使用例

計算済み属性の使用例は、単純な計算から非常に複雑な参照まで多岐にわたります。 次に、計算済み属性の使用例をいくつか示します。

1. **割合：** 単純な計算済み属性には、レコード上の2つの数値フィールドを取り出し、それらを分割して割合を作成することが含まれます。 例えば、個人に送信された電子メールの合計数を、個人が開く電子メールの数で割ることができます。 結果の計算済み属性フィールドを見ると、個人が開封した電子メールの合計の割合がすばやく表示されます。
1. **アプリの使用：** 別の例としては、ユーザーが集計を開いた回数をアプリケーション化する機能があります。 個々のオープンイベントに基づいてアプリのオープン数を追跡することで、100回目のオープン時に特別なオファーやメッセージをユーザーに配信し、貴社のブランドに対するより深い関与を促すことができます。
1. **全期間値：** 顧客の全期間購入値など、実行合計の収集は非常に困難な場合があります。 これには、新しい購入イベントが発生するたびに履歴合計を更新する必要があります。イベントは 計算済みの属性を使用すると、顧客に関連する購入イベントが成功するたびに自動的に更新される単一のフィールドにライフタイム値を保持することで、この処理をより簡単に行うことができます。

## 計算済み属性の設定

計算済み属性を設定するには、まず、計算済み属性値を保持するフィールドを指定する必要があります。 このフィールドは、ミックスインを使用して作成し、既存のスキーマにフィールドを追加するか、スキーマ内で既に定義済みのフィールドを選択して作成できます。

>[!NOTE]
>計算済み属性は、アドビ定義のミックスイン内のフィールドに追加できません。 フィールドは、定義して `tenant` 名前空間に追加するフィールドである必要があります。つまり、スキーマ内に入れる必要があります。

計算済みの属性フィールドを正しく定義するには、スキーマがプロファイル可能で、スキーマの基となるクラスの和集合スキーマの一部として表示される必要があります。 プロファイル対応のスキーマと和集合の詳細については、「スキーマレジストリ開発者ガイド」の節で、プロファイルと和集合のスキーマの表示に関する [スキーマの有効化に関する節を参照してください](../../xdm/api/getting-started.md)。 また、構成の基本ドキュメントの [和集合](../../xdm/schema/composition.md) に関する節を確認することをお勧めします。

このチュートリアルのワークフローでは、プロファイル対応のスキーマを使用し、計算済み属性フィールドを含む新しいミックスインを定義し、正しい名前空間であることを確認する手順に従います。 プロファイルが有効なスキーマ内に、既に正しい名前空間のフィールドがある場合は、計算済み属性を作成する手順に直接進む [ことができます](#create-a-computed-attribute)。

### 表示スキーマ

次の手順では、Adobe Experience Platformのユーザーインターフェイスを使用してスキーマを検索し、ミックスインを追加し、フィールドを定義します。 スキーマレジストリAPIの使用を希望する場合は、『 [スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md) 』を参照して、Mixinの作成、スキーマへのMixinの追加、リアルタイム顧客プロファイルでのスキーマの有効化の手順を確認してください。

ユーザインターフェイスで、左側のパネルの **スキーマ** ( ** )をクリックし、「参照」タブの検索バーを使用して、更新するスキーマをすばやく見つけます。

![](../images/computed-attributes/Schemas-Browse.png)

スキーマを見つけたら、名前をクリックしてスキーマエディタを開き、スキーマを編集します。

![](../images/computed-attributes/Schema-Editor.png)

### ミックスインの作成

新しいMixinを作成するには、エデ **追加ィターの左側にある****** 「コンポジション」セクションで、Mixinsの横のをクリックします。 既存のミックス **追加インを表示できる** [ミックスイン]ダイアログボックスが開きます。 新しいMixinを定義するには、[新し **いMixinを作成** ]のラジオボタンをクリックします。

mixinに名前と説明を入力し、完了したら「 **追加mixin** 」をクリックします。

![](../images/computed-attributes/Add-mixin.png)

### 追加スキーマ

これで、新しいMixinが「コンポジション」の下の「Mixins *」セクションに表示さ* れます **。 Mixinの名前をクリックすると、エディタの **** 「構造」( *Structure* )セクションに複数のフィールドボタンが表示されます。

最上 **** 追加位のフィールドを追加するには、スキーマ名の横にあるフィールドを選択します。スキーマ内の任意の場所にフィールドを追加することもできます。

フィール **追加ドをクリックすると、テナント** IDの名前を付けた新しいオブジェクトが開き、フィールドが正しい名前空間になっていることが示されます。 そのオブジェクト内に、「新規」フ *ィールドが表示さ* れます。 計算済み属性を定義するフィールドの場合。

![](../images/computed-attributes/New-field.png)

### フィールドの設定

エディタ *ーの右側の* 「フィールドプロパティ」セクションを使用して、新しいフィールドの名前、表示名、タイプなど、必要な情報を指定します。

>[!NOTE]
>フィールドのタイプは、計算済みの属性値と同じタイプである必要があります。 例えば、計算された属性値が文字列の場合、スキーマで定義するフィールドは文字列である必要があります。

完了したら、「 **Apply** 」をクリックし、フィールドの名前とタイプがエディタの「 *Structure* 」セクションに表示されます。

![](../images/computed-attributes/Apply.png)

### スキーマのプロファイル

続行する前に、スキーマが有効になっていることを確認します。 エディタの *Structure* （構造）セクションでスキーマ名をクリックし、「 *スキーマプロパティ* 」タブが表示されます。 プロファイルスラ **イダ** が青の場合は、スキーマが有効になっています。プロファイル

>[!NOTE]
>プロファイル用のスキーマを有効にすると元に戻せないので、一旦有効にした後でスライダをクリックした場合は、無効にする必要はありません。

![](../images/computed-attributes/Profile.png)

「保存」をクリック **して** 、更新したスキーマを保存し、APIを使用して残りのチュートリアルに進むことができるようになりました。

### 計算済み属性の作成 {#create-a-computed-attribute}

計算済み属性フィールドを特定し、スキーマのプロファイルが有効になっていることを確認したら、計算済み属性を設定できます。

まず、作成する計算済み属性の詳細を含む `/config/computedAttributes` リクエスト本文を使用して、エンドポイントにPOSTリクエストを作成します。

**API形式**

```http
POST /config/computedAttributes
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/computedAttributes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}'\
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "name" : "birthdayCurrentMonth",
        "path" : "_{TENANT_ID}",
        "description" : "Computed attribute to capture if the customer birthday is in the current month.",
        "expression" : {
            "type" : "PQL", 
            "format" : "pql/text", 
            "value":  "person.birthDate.getMonth() = currentMonth()"
        },
        "schema": 
          {
            "name":"_xdm.context.profile"
          }
          
      }'
```

| プロパティ | 説明 |
|---|---|
| `name` | 文字列としての、計算済み属性フィールドの名前。 |
| `path` | 計算済み属性を含むフィールドへのパス。 このパスは、スキーマの属性内に `properties` あり、パスにフィールド名を含めないでください。 パスを書き込む場合は、複数レベルの属性を省略し `properties` ます。 |
| `{TENANT_ID}` | テナントIDに慣れていない場合は、『 [スキーマレジストリ開発者ガイド』のテナントIDを見つける手順を参照してください](../../xdm/api/getting-started.md#know-your-tenant_id)。 |
| `description` | 計算済み属性の説明。 これは、複数の計算済み属性が定義された後に特に便利です。これは、IMS組織内の他のユーザーが、使用する正しい計算済み属性を判断するのに役立ちます。 |
| `expression.value` | 有効なプロファイルクエリ言語(PQL)式。 PQLの詳細とサポートされるクエリへのリンクについては、 [PQLの概要を参照してください](../../segmentation/pql/overview.md)。 |
| `schema.name` | 計算済み属性フィールドを含むスキーマの基となるクラス。 例：XDM ExperienceEventク `_xdm.context.experienceevent` ラスに基づくスキーマの場合。 |

**応答**

正常に作成された計算済み属性は、HTTPステータス200(OK)と、新しく作成された計算済み属性の詳細を含む応答本体を返します。 これらの詳細には、他のAPI操作中に計算済み属性を参照するために `id` 使用できる、一意の読み取り専用のシステム生成が含まれます。

```json
{
    "id": "2afcf410-450e-4a39-984d-2de99ab58877",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "birthdayCurrentMonth",
    "path": "_{TENANT_ID}",
    "positionPath": [
        "_{TENANT_ID}"
    ],
    "description": "Computed attribute to capture if the customer birthday is in the current month.",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "person.birthDate.getMonth() = currentMonth()"
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "returnSchema": {
        "meta:xdmType": "string"
    },
    "definedOn": [
        {
            "meta:resourceType": "unions",
            "meta:containerId": "tenant",
            "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "encodedDefinedOn":"?\b?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?8{?E:",
    "dependencies": [],
    "dependents": [],
    "active": true,
    "type": "ComputedAttribute",
    "createEpoch": 1572555223,
    "updateEpoch": 1572555225
}
```

| プロパティ | 説明 |
|---|---|
| `id` | 他のAPI操作中に計算済み属性を参照するために使用できる、一意の読み取り専用のシステム生成ID。 |
| `imsOrgId` | 計算済み属性に関連するIMS組織は、要求で送信される値と一致する必要があります。 |
| `sandbox` | sandboxオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。 この情報は、リクエストで送信されるサンドボックスヘッダーから取得されます。 詳しくは、サンドボックスの概要を参照 [してください](../../sandboxes/home.md)。 |
| `positionPath` | リクエストで送信されたフ `path` ィールドに分解された要素を含む配列。 |
| `returnSchema.meta:xdmType` | 計算済み属性が保存されるフィールドのタイプ。 |
| `definedOn` | 計算済み属性が定義された和集合スキーマを示す配列。 和集合スキーマごとに1つのオブジェクトを含みます。つまり、異なるクラスに基づいて計算された属性が複数のスキーマに追加された場合、配列内に複数のオブジェクトが存在する可能性があります。 |
| `active` | 計算済み属性が現在アクティブかどうかを示すboolean値。 デフォルト値はです `true`。 |
| `type` | 作成されるリソースのタイプ。この場合は「ComputedAttribute」がデフォルト値です。 |
| `createEpoch` および `updateEpoch` | 計算済み属性が作成された時刻と最終更新された時刻。 |


## 計算済み属性へのアクセス

APIを使用して計算済み属性を操作する場合、組織で定義された計算済み属性にアクセスするためのオプションが2つあります。 1つ目は、すべての計算済み属性をリストする方法、もう1つは、固有の計算済み属性を表示する方法で `id`す。

すべての計算済み属性のリストと、特定の計算済み属性の表示の両方の手順について、以降の節で説明します。

### リストの計算済み属性 {#list-computed-attributes}

IMS組織は複数の計算済み属性を作成でき、エンドポイントに対してGETリクエストを実行すると、組織の `/config/computedAttributes` 既存の計算済み属性をすべてリストできます。

**API形式**

```http
GET /config/computedAttributes
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/ \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

成功した応答には、計算 `_page` された属性の合計数(`totalCount`)と、ページ上の計算された属性の数(`pageSize`)を提供する属性が含まれる。

応答には、1つ以上のオ `children` ブジェクトで構成される配列も含まれ、それぞれに計算済みの1つの属性の詳細が含まれます。 組織に計算済みの属性がない場合、 `totalCount` とは0 `pageSize` （ゼロ）で、配列は `children` 空になります。

```json
{
    "_page": {
        "totalCount": 2,
        "pageSize": 2
    },
    "children": [
        {
            "id": "2afcf410-450e-4a39-984d-2de99ab58877",
            "imsOrgId": "{IMS_ORG}",
            "sandbox": {
                "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "name": "birthdayCurrentMonth",
            "path": "person._{TENANT_ID}",
            "positionPath": [
                "person",
                "_{TENANT_ID}"
            ],
            "description": "Computed attribute to capture if the customer birthday is in the current month.",
            "expression": {
                "type": "PQL",
                "format": "pql/text",
                "value": "person.birthDate.getMonth() = currentMonth()"
            },
            "schema": {
                "name": "_xdm.context.profile"
            },
            "returnSchema": {
                "meta:xdmType": "string"
            },
            "definedOn": [
                {
                    "meta:resourceType": "unions",
                    "meta:containerId": "tenant",
                    "$ref": "https://ns.adobe.com/xdm/context/profile__union"
                }
            ],
            "encodedDefinedOn":"?\b?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?8{?E:",
            "dependencies": [],
            "dependents": [],
            "active": true,
            "type": "ComputedAttribute",
            "createEpoch": 1572555223,
            "updateEpoch": 1572555225
        },
        {
            "id": "ae0c6552-cf49-4725-8979-116366e8e8d3",
            "imsOrgId": "{IMS_ORG}",
            "sandbox": {
                "sandboxId": "",
                "sandboxName": "",
                "type": "production",
                "default": true
            },
            "name": "productDownloads",
            "path": "_{TENANT_ID}",
            "positionPath": [
                "_{TENANT_ID}"
            ],
            "description": "Calculate total product downloads.",
            "expression": {
                "type" : "PQL", 
                "format" : "pql/text", 
                "value":  "let Y = xEvent[_coresvc.event.subType = \"DOWNLOAD\"].groupBy(_coresvc.attributes[name = \"product\"].value).map({
                  \"downloaded\": this.head()._coresvc.attributes[name = \"product\"].head().value,
                  \"downloadsSum\": this.count(),
                  \"downloadsToday\": this[timestamp occurs today].count(),
                  \"downloadsPast30Days\": this[timestamp occurs < 30 days before now].count(),
                  \"downloadsPast60Days\": this[timestamp occurs < 60 days before now].count(),
                  \"downloadsPast90Days\": this[timestamp occurs < 90 days before now].count() }) in { \"uniqueProductDownloadSum\": Y.count(), \"products\": Y }"
            },
            "returnSchema": {
                "meta:xdmType": "string"
            },
            "definedOn": [
                {
                    "meta:resourceType": "unions",
                    "meta:containerId": "tenant",
                    "$ref": "https://ns.adobe.com/xdm/context/profile__union"
                }
            ],
            "schema": {
                "name": "_xdm.context.profile"
            },
            "encodedDefinedOn": "\u001f?\b\u0000\u0000\u0000\u0000\u0000\u0000\u0000?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?\u0005\u00008{?E:\u0000\u0000\u0000",
            "dependencies": [],
            "dependents": [],
            "active": true,
            "type": "ComputedAttribute",
            "createEpoch": 1571945277,
            "updateEpoch": 1571945280
        }
    ],
    "_links": {
        "next": {}
    }
}
```

| プロパティ | 説明 |
|---|---|
| `_page.totalCount` | IMS組織で定義された計算済み属性の合計数です。 |
| `_page.pageSize` | 結果のこのページで返された計算済み属性の数。 がと等 `pageSize` しい場合、 `totalCount`結果のページが1ページのみで、計算済みの属性がすべて返されたことを意味します。 等しくない場合は、結果のページが追加でアクセスできます。 詳しくは、を `_links.next` 参照してください。 |
| `children` | 1つ以上のオブジェクトで構成される配列。各オブジェクトには、1つの計算済み属性の詳細が含まれます。 計算済みの属性が定義されていない場合、配 `children` 列は空です。 |
| `id` | 計算済み属性の作成時に自動的に割り当てられる、一意の読み取り専用のシステム生成値。 計算済み属性オブジェクトのコンポーネントの詳細については、このチュートリアルの前の「計算済み [属性の作成](#create-a-computed-attribute) 」の節を参照してください。 |
| `_links.next` | 計算済み属性の1ページが返される場合、 `_links.next` は空のオブジェクトで、前述の応答例に示されています。 組織に多数の計算済み属性がある場合、値に対してGETリクエストを行うことでアクセスできる複数のページに対して返さ `_links.next` れます。 |

### 表示計算済み属性 {#view-a-computed-attribute}

特定の計算済み属性を表示するには、エンドポイントにGETリクエストを作成し、計算済み `/config/computedAttributes` 属性IDをリクエストパスに含めます。

**API形式**

```http
GET /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 表示する計算済み属性のID。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/2afcf410-450e-4a39-984d-2de99ab58877 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

```json
{
    "id": "2afcf410-450e-4a39-984d-2de99ab58877",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "ff0f6870-c46d-11e9-8ca3-036939a64204",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "birthdayCurrentMonth",
    "path": "_{TENANT_ID}",
    "positionPath": [
        "_{TENANT_ID}"
    ],
    "description": "Computed attribute to capture if the customer birthday is in the current month.",
    "expression": {
        "type": "PQL",
        "format": "pql/text",
        "value": "person.birthDate.getMonth() = currentMonth()"
    },
    "schema": {
        "name": "_xdm.context.profile"
    },
    "returnSchema": {
        "meta:xdmType": "string"
    },
    "definedOn": [
        {
            "meta:resourceType": "unions",
            "meta:containerId": "tenant",
            "$ref": "https://ns.adobe.com/xdm/context/profile__union"
        }
    ],
    "encodedDefinedOn":"?\b?VR)JMS?R?())(????+?KL?OJ?K???H??O??+I?(?/(?O??I??/????S?8{?E:",
    "dependencies": [],
    "dependents": [],
    "active": true,
    "type": "ComputedAttribute",
    "createEpoch": 1572555223,
    "updateEpoch": 1572555225
}
```

## 計算済み属性の更新

既存の計算済み属性を更新する必要がある場合、これを行うには、エンドポイントにPATCHリクエストを作成し、更新する計算済みの属性のIDをリクエストパスに含めます。 `/config/computedAttributes`

**API形式**

```http
PATCH /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 更新する計算済み属性のID。 |

**リクエスト**

この要求では、 [JSONパッチの形式設定を使用して](http://jsonpatch.com/) 、「式」フィールドの「値」を更新します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/ae0c6552-cf49-4725-8979-116366e8e8d3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}'\
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \  
  -d '[
        {
          "op": "add",
          "path": "/expression",
          "value": 
          {
            "type" : "PQL", 
            "format" : "pql/text", 
            "value":  "{NEW_EXPRESSION_VALUE}"
          }
        }
      ]'
```

| プロパティ | 説明 |
|---|---|
| `{NEW_EXPRESSION_VALUE}` | 有効なプロファイルクエリ言語(PQL)式。 PQLの詳細とサポートされるクエリへのリンクについては、 [PQLの概要を参照してください](../../segmentation/pql/overview.md)。 |

**応答**

更新が成功すると、HTTPステータス204（コンテンツなし）と空の応答本文が返されます。 更新が成功したことを確認する場合は、計算済み属性をIDで表示するGETリクエストを実行できます。

## 計算済み属性の削除

APIを使用して、計算済み属性を削除することもできます。 これは、エンドポイントにDELETEリクエストを行い、 `/config/computedAttributes` 削除する計算済み属性のIDをリクエストパスに含めることで行われます。

>[!N注意]
>計算済み属性を削除する場合は、複数のスキーマで使用されている可能性があるので注意が必要です。また、DELETE操作を元に戻すことはできません。

**API形式**

```http
DELETE /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 削除する計算済み属性のID。 |

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/ups/config/computedAttributes/ae0c6552-cf49-4725-8979-116366e8e8d3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -H 'x-sandbox-name: {SANDBOX_NAME}' \ 
```

**応答**

削除要求が成功すると、HTTPステータス200(OK)と空の応答本文が返されます。 削除が成功したことを確認するために、GETリクエストを実行して、計算済み属性をIDで参照できます。 属性が削除された場合は、HTTPステータス404（見つかりません）エラーが表示されます。

## 次の手順

これで、計算済み属性の基本を学んだので、組織に合わせて定義を開始する準備が整いました。