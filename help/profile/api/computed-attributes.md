---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: d464a6b4abd843f5f8545bc3aa8000f379a86c6d
workflow-type: tm+mt
source-wordcount: '2431'
ht-degree: 1%

---


# （アルファ）計算済み属性の端点

>[!IMPORTANT]
>このドキュメントで説明されている計算済み属性機能は、現在アルファベットで表示されており、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。 計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントに対して集計値を実行できます。

計算済みの各属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式(「rule」)が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーション開封回数などに関する質問に簡単に回答できます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。

このガイドは、Adobe Experience Platform内の計算済み属性をより深く理解するのに役立ち、エンドポイントを使用して基本的なCRUD操作を実行するためのサンプルAPI呼び出しを含み `/config/computedAttributes` ます。

## はじめに

このガイドで使用されるAPIエンドポイントは、 [リアルタイム顧客プロファイルAPIの一部](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)です。 先に進む前に、 [はじめに](getting-started.md) 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## 計算済み属性について

Adobe Experience Platformを使用すると、複数のソースからデータを簡単にインポートおよびマージして、リアルタイム顧客プロファイルを生成できます。 各プロファイルには、個人に関する重要な情報（連絡先情報、好み、購入履歴など）が含まれ、360度の表示を顧客に提供します。

データフィールドを直接読み取る場合（「名」など）は、プロファイルで収集される情報の一部がわかりやすくなります。一方、情報を生成するには、複数の計算を行うか、他のフィールドや値に依存する必要があります（「全期間購入合計」など）。 このデータを一目で理解しやすくするために、Platformを使用すると、これらの参照と計算を自動的に実行する **計算済み属性** 、および適切なフィールドに値を返すことができます。

計算済みの属性には、受信データに対して動作し、結果の値をプロファイルの属性またはイベントに格納する式（「ルール」）の作成が含まれます。 式は複数の異なる方法で定義できます。ルールでは、受信イベントのみ、受信イベントとプロファイルデータ、または受信イベント、プロファイルデータ、履歴イベントを評価するよう指定できます。

### 使用例

計算済み属性の使用例は、単純な計算から非常に複雑な参照まで様々です。 次に、計算済み属性の使用例をいくつか示します。

1. **割合：** 単純な計算済み属性には、レコード上の2つの数値フィールドを取り、それらを分割して割合を作成することが含まれます。 例えば、1人の個人に送信された電子メールの合計数を取得し、その電子メール数を個人が開く電子メールの数で割ることができます。 結果の計算済み属性フィールドを調べると、個人が開封した電子メールの合計の割合がすばやく表示されます。
1. **アプリケーションの使用：** 別の例としては、ユーザーがアプリを開いた回数を集計する機能もあります。 申し込みを開いた回数の合計を個々のオープンイベントに基づいて追跡することで、100回目のオープン時に特別なオファーやメッセージをユーザーに提供でき、ブランドに対するより深い関与を促すことができます。
1. **ライフタイム値：** 顧客の全期間購入値など、現在の合計を収集するのは非常に困難な場合があります。 これには、新しい購入イベントが発生するたびに履歴の合計を更新する必要があります。 計算済みの属性を使用すると、顧客に関連する購入イベントが成功するたびに自動的に更新される単一のフィールドにライフタイム値を保持することで、この作業をより簡単に行うことができます。

## 計算済み属性の設定

計算済み属性を設定するには、まず、計算済み属性値を保持するフィールドを指定する必要があります。 このフィールドは、ミックスインを使用して作成し、既存のスキーマにフィールドを追加するか、スキーマ内で既に定義済みのフィールドを選択することで作成できます。

>[!NOTE]
>計算済み属性は、アドビ定義ミックスイン内のフィールドに追加できません。 フィールドは `tenant` 名前空間内にある必要があります。つまり、定義してスキーマに追加するフィールドである必要があります。

計算済み属性フィールドを正しく定義するには、スキーマがプロファイル可能で、スキーマの基となるクラスの和集合スキーマの一部として表示される必要があります。 プロファイルが有効なスキーマと和集合の詳細については、『スキーマレジストリ開発者ガイド』のセクションで、和集合スキーマのプロファイルと表示に関するスキーマを [有効にする方法を確認してください](../../xdm/api/getting-started.md)。 また、スキーマ構成の基本ドキュメントの和集合に関する [節を確認することをお勧めします](../../xdm/schema/composition.md) 。

このチュートリアルのワークフローでは、プロファイル対応スキーマを使用し、計算済み属性フィールドを含む新しいミックスインを定義し、正しい名前空間であることを確認する手順に従います。 プロファイル対応スキーマ内に、既に正しい名前空間のフィールドが存在する場合は、直接、計算済み属性を [作成する手順に進むことができます](#create-a-computed-attribute)。

### スキーマの表示

以下の手順では、Adobe Experience Platformのユーザーインターフェイスを使用してスキーマを検索し、Mixinを追加し、フィールドを定義します。 スキーマレジストリAPIの使用を希望する場合は、『 [スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md) 』を参照して、Mixinの作成、スキーマへのMixinの追加、Real-time Customer Commentarプロファイルでのスキーマの有効化の手順を確認してください。

ユーザーインターフェイスで、左側のパネルの **スキーマ** ( ** )をクリックし、「参照」タブの検索バーを使用して、更新するスキーマをすばやく見つけます。

![](../images/computed-attributes/Schemas-Browse.png)

スキーマを見つけたら、そのスキーマの名前をクリックしてスキーマエディタを開き、に編集を加えることができます。

![](../images/computed-attributes/Schema-Editor.png)

### Mixinの作成

新しいMixinを作成するには、エディターの左側にある **Composition***(コンポジション*** )セクションのMixinsの横にあるをクリックします。 これにより、 **ミックスイン** ダイアログが開き、既存のミックスインを表示できます。 新しいミックスインを定義するには、[新しいミックスインを **作成** ]のラジオボタンをクリックします。

mixinに名前と説明を入力し、完了したら **追加「** mixin」をクリックします。

![](../images/computed-attributes/Add-mixin.png)

### スキーマ追加の計算済み属性フィールド

これで、「 *組版」の下の「* Mixins *」セクションに新しいMixinが表示されます*。 Mixinの名前をクリックすると、エディタの **構造***(* Structure)セクションに複数のフィールド(Mixin)ボタンが表示されます。

最上位 **フィールドを追加するには、スキーマ名の横にある** フィールドを選択します。スキーマ内の任意の場所にフィールドを追加することもできます。

フィー **** ルドを追加クリックすると、テナントIDという名前の新しいオブジェクトが開き、フィールドが正しい名前空間にあることが示されます。 そのオブジェクト内に *新規フィールド* が表示されます。 計算済み属性を定義するフィールドの場合。

![](../images/computed-attributes/New-field.png)

### フィールドの設定

エディターの右側にある「 *フィールドプロパティ* 」セクションを使用して、名前、表示名、タイプなど、新しいフィールドに必要な情報を入力します。

>[!NOTE]
>フィールドの型は、計算済み属性値と同じ型である必要があります。 例えば、計算された属性値が文字列の場合、スキーマで定義するフィールドは文字列である必要があります。

完了したら、 **適用** (Apply *)をクリックし、フィールドの名前とタイプがエディタの* 構造(Structure)セクションに表示されます。

![](../images/computed-attributes/Apply.png)

### プロファイルのスキーマを有効にする

先に進む前に、スキーマのプロファイルが有効になっていることを確認してください。 エディタの「 *構造* 」セクションでスキーマ名をクリックし、「 *スキーマのプロパティ* 」タブが表示されるようにします。 **プロファイル** ・スライダが青の場合は、スキーマのプロファイルが有効になっています。

>[!NOTE]
>プロファイルのスキーマを有効にした場合、元に戻すことはできないので、一度有効にした後でスライダをクリックした場合に、無効にするリスクはありません。

![](../images/computed-attributes/Profile.png)

「 **保存** 」をクリックして、更新したスキーマを保存し、APIを使用して残りのチュートリアルに進むことができるようになりました。

### 計算済み属性の作成 {#create-a-computed-attribute}

計算済み属性フィールドを識別し、スキーマがプロファイル可能であることを確認した後、計算済み属性を設定できます。

まず、作成する計算済み属性の詳細を含むリクエスト本文を使用して、 `/config/computedAttributes` エンドポイントにPOSTリクエストを行います。

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
| `name` | 文字列としての計算済み属性フィールドの名前。 |
| `path` | 計算済み属性を含むフィールドへのパス。 このパスはスキーマの `properties` 属性内にあり、パスにフィールド名を含めないでください。 パスを書き込む場合は、複数レベルの `properties` 属性を省略します。 |
| `{TENANT_ID}` | テナントIDに慣れていない場合は、『 [スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md#know-your-tenant_id)』で、テナントIDを探す手順を参照してください。 |
| `description` | 計算済み属性の説明。 これは特に、複数の計算済み属性が定義された場合に役立ちます。これは、IMS組織内の他のユーザーが、使用する正しい計算済み属性を判断するのに役立ちます。 |
| `expression.value` | 有効なプロファイルクエリ言語(PQL)式。 PQLの詳細およびサポートされるクエリへのリンクについては、 [PQLの概要を参照してください](../../segmentation/pql/overview.md)。 |
| `schema.name` | 計算済み属性フィールドを含むスキーマが基になるクラス。 例： `_xdm.context.experienceevent` の値は、XDM ExperienceEventクラスに基づくスキーマに対して設定されます。 |

**応答**

正常に作成された計算済み属性は、HTTPステータス200(OK)と、新しく作成された計算済み属性の詳細を含む応答本体を返します。 これらの詳細には、読み取り専用のシステム生成の一意の情報が含まれ `id` 、これは他のAPI操作中に計算済みの属性を参照するのに使用できます。

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
| `sandbox` | sandboxオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。 この情報は、リクエストで送信されるサンドボックスのヘッダーから取得されます。 詳しくは、 [サンドボックスの概要を参照してください](../../sandboxes/home.md)。 |
| `positionPath` | リクエストで送信されたフィールド `path` に分解された、要素を含む配列。 |
| `returnSchema.meta:xdmType` | 計算済み属性が格納されるフィールドのタイプ。 |
| `definedOn` | 計算済み属性が定義された和集合スキーマを示す配列。 和集合スキーマごとに1つのオブジェクトが含まれます。つまり、異なるクラスに基づいて計算済みの属性が複数のスキーマに追加された場合、配列内に複数のオブジェクトが存在する可能性があります。 |
| `active` | 計算済み属性が現在アクティブかどうかを示すboolean値。 By default the value is `true`. |
| `type` | 作成されるリソースのタイプ。この場合は「ComputedAttribute」がデフォルト値です。 |
| `createEpoch` および `updateEpoch` | 計算済み属性が作成された時刻と最後に更新された時刻。 |


## 計算済み属性へのアクセス

APIを使用して計算済み属性を操作する場合、組織で定義されている計算済み属性にアクセスするための2つのオプションがあります。 1つ目は、すべての計算済み属性をリストすること、2つ目は、固有の計算済み属性を表示することで `id`す。

すべての計算済み属性のリストと、特定の計算済み属性の表示の両方の手順を、以下の節で説明します。

### リスト計算済み属性 {#list-computed-attributes}

IMS組織は、複数の計算済み属性を作成できます。エンドポイントに対してGETリクエストを実行すると、組織の既存の計算済み属性をすべてリストでき `/config/computedAttributes` ます。

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

成功した応答には、計算済み属性の総数( `_page` )と、ページ上の計算済み属性の数(`totalCount``pageSize`)を提供する属性が含まれます。

この応答には、1つ以上のオブジェクトから成る `children` 配列も含まれ、それぞれに計算済みの1つの属性の詳細が含まれます。 組織に計算済みの属性がない場合、 `totalCount` およびは0 （ゼロ） `pageSize` になり、 `children` 配列は空になります。

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
| `_page.pageSize` | 結果のこのページで返される計算済み属性の数。 がと等し `pageSize``totalCount`い場合、結果のページが1ページのみで、計算済み属性がすべて返されたことを意味します。 同じでない場合は、アクセスできる結果のページが他にもあります。 See `_links.next` for details. |
| `children` | 1つ以上のオブジェクトで構成される配列。それぞれ計算済みの単一の属性の詳細が含まれます。 計算済み属性が定義されていない場合、 `children` 配列は空です。 |
| `id` | 計算済み属性の作成時に自動的に割り当てられる、一意の読み取り専用のシステム生成値。 計算済み属性オブジェクトのコンポーネントの詳細については、このチュートリアルで前述した、計算済み属性の [作成に関する節を参照してください](#create-a-computed-attribute) 。 |
| `_links.next` | 計算済み属性のページが1ページだけ返された場合、 `_links.next` は空のオブジェクトです。上記の応答例を参照してください。 組織に多数の計算済み属性がある場合、それらの属性は、値に対してGETリクエストを行うことでアクセスできる複数のページに返され `_links.next` ます。 |

### 計算済み属性の表示 {#view-a-computed-attribute}

エンドポイントにGETリクエストを作成し、計算済みの属性IDをリクエストパスに含めることで、特定の計算済み属性を表示することも `/config/computedAttributes` できます。

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

既存の計算済み属性を更新する必要がある場合、これを行うには、エンドポイントにPATCHリクエストを作成し、更新する計算済み属性のIDをリクエストパスに含め `/config/computedAttributes` ます。

**API形式**

```http
PATCH /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 更新する計算済み属性のID。 |

**リクエスト**

このリクエストでは、 [JSONパッチの形式設定を使用して](http://jsonpatch.com/) 、「式」フィールドの「値」を更新します。

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
| `{NEW_EXPRESSION_VALUE}` | 有効なプロファイルクエリ言語(PQL)式。 PQLの詳細およびサポートされるクエリへのリンクについては、 [PQLの概要を参照してください](../../segmentation/pql/overview.md)。 |

**応答**

更新が成功すると、HTTPステータス204（コンテンツなし）と空の応答本文が返されます。 更新が成功したことを確認する場合は、GETリクエストを実行して、計算済み属性をIDで表示できます。

## 計算済み属性の削除

APIを使用して、計算済み属性を削除することもできます。 これは、削除する計算済み属性のIDを要求パスに含め、 `/config/computedAttributes` エンドポイントにDELETE要求を行うことで行われます。

>[!N注意]
>計算済み属性を削除する場合は、注意が必要です。計算済み属性が複数のスキーマで使用されている可能性があり、DELETE操作を元に戻すことはできません。

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

削除が成功すると、HTTPステータス200(OK)と空の応答本文が返されます。 削除が成功したことを確認するために、GETリクエストを実行し、計算済み属性をそのIDで参照できます。 属性が削除された場合は、HTTPステータス404（見つかりません）エラーが発生します。

## 次の手順

計算済み属性の基本を学んだので、組織に合わせて定義を開始する準備が整いました。