---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API
title: 計算済み属性APIエンドポイント
topic: guide
type: Documentation
description: '計算済み属性を使用すると、他の値、計算および式に基づいてフィールドの値を自動計算できます。計算済み属性は、リアルタイム顧客プロファイルデータに対して機能します。つまり、Adobe Experience Platformに保存されているすべてのレコードおよびイベントの集計値を処理できます。 '
translation-type: tm+mt
source-git-commit: e6ecc5dac1d09c7906aa7c7e01139aa194ed662b
workflow-type: tm+mt
source-wordcount: '2450'
ht-degree: 82%

---


# （アルファ）計算済み属性の端点

>[!IMPORTANT]
>
>このドキュメントで説明されている計算済み属性機能は、現在アルファ段階であり、すべてのユーザーが使用できるわけではありません。ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算および式に基づいてフィールドの値を自動計算できます。計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントをまたいで値を集計できます。

各計算済み属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式（ルール）が含まれます。これらの計算により、ライフタイム購入値、購入間隔、アプリケーション開封数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。

このガイドは、Adobe Experience Platform 内の計算済み属性をより深く理解するのに役立ちます。また、`/config/computedAttributes` エンドポイントを使用して基本的な CRUD 操作を実行するためのサンプル API 呼び出しを提供します。

## はじめに

このガイドで使用されるAPIエンドポイントは、[リアルタイム顧客プロファイルAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)の一部です。 先に進む前に、[はじめにガイド](getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しの読み方、および任意の[!DNL Experience Platform] APIの呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## 計算済み属性について

Adobe Experience Platformでは、複数のソースからデータを簡単にインポートおよびマージして[!DNL Real-time Customer Profiles]を生成できます。 各プロファイルには、顧客の連絡先情報、好み、購入履歴など、顧客に関する重要な情報が含まれ、顧客の全体像を把握することができます。

プロファイルで収集された情報には、データフィールドを直接読み取る場合にわかりやすい（「名」など）ものや、情報を生成するために複数の計算を実施するもの、他のフィールドの値に依存するもの（「ライフタイム購入合計」など）があります。このデータを一目で理解しやすくするために、[!DNL Platform]を使用すると、これらの参照と計算を自動的に実行する計算済み属性を作成し、適切なフィールドに値を返すことができます。

計算済み属性には、受信データ上で操作し、結果の値をプロファイル属性またはイベントに格納する式（ルール）の作成が含まれます。式は複数の異なる方法で定義でき、「受信イベントのみ」、「受信イベントとプロファイルデータ」、または「受信イベント、プロファイルデータ、および履歴イベント」を評価するルールを指定できます。

### 使用例

計算済み属性の使用例は、単純な計算から非常に複雑な参照まで多岐にわたります。次に、計算済み属性の使用例をいくつか示します。

1. **[!UICONTROL 割合]：計算結果の単純な属性** には、レコード上の2つの数値フィールドを取り出し、それらを分割して割合を作成することが含まれます。例えば、ある人物に送信された電子メールの合計数を、その人物が開封した電子メールの数で割ることができます。結果の計算済み属性フィールドを見ると、その人物による電子メールの開封率合がすばやく表示されます。
1. **[!UICONTROL アプリの使用]:** 別の例として、ユーザーがアプリを開いた回数を集計する機能があります。個人のオープンイベントに基づいてアプリケーションを開いた合計回数を追跡し、ユーザーが 100 回目に開いた際に特別なオファーやメッセージを配信し、ブランドとのエンゲージメントを高めるよう促すことができます。
1. **[!UICONTROL ライフタイム値]：顧客のライフタイム購入値など、実行中の合計を** 収集するのは非常に困難な場合があります。これには、新しい購入イベントが発生するたびに履歴合計を更新する必要があります。計算済みの属性を使用すると、顧客に関連する購入イベントが成功するたびに自動的に更新される、単一のフィールドにライフタイム値を保持することで、この処理をより簡単におこなうことができます。

## 計算済み属性の設定

計算済み属性を設定するには、まず、計算済み属性値を保持するフィールドを特定する必要があります。このフィールドは、Mixin を使用して、既存のスキーマにフィールドを追加するか、スキーマ内で既に定義されているフィールドを選択することで作成できます。

>[!NOTE]
>
>計算済み属性は、アドビ定義の Mixin 内のフィールドに追加することはできません。フィールドは、`tenant` 名前空間内に存在する、つまり、定義してスキーマに追加するフィールドである必要があります。

計算済みの属性フィールドを正しく定義するには、スキーマが[!DNL Profile]に対して有効にされ、スキーマの基となるクラスの和集合スキーマの一部として表示される必要があります。 [!DNL Profile]が有効なスキーマと和集合の詳細については、[スキーマのプロファイルと和集合スキーマの表示を有効にする](../../xdm/api/getting-started.md)の[!DNL Schema Registry]開発者ガイドの節を参照してください。 また、構成の基本ドキュメントの[和集合に関する節](../../xdm/schema/composition.md)を確認することをお勧めします。

このチュートリアルのワークフローは、[!DNL Profile]が有効なスキーマを使用し、計算済みの属性フィールドを含む新しいミックスインを定義し、正しい名前空間であることを確認する手順に従います。 プロファイル対応スキーマ内の適切な名前空間に、既にフィールドがある場合は、[計算済み属性を作成する](#create-a-computed-attribute)手順に直接進むことができます。

### スキーマの表示

次の手順では、Adobe Experience Platform のユーザーインターフェイスを使用してスキーマを検索し、Mixin を追加してし、フィールドを定義します。[!DNL Schema Registry] APIを使用したい場合は、[スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md)を参照して、ミックスインの作成、スキーマへのミックスインの追加、[!DNL Real-time Customer Profile]で使用するスキーマの有効化を行ってください。

ユーザインターフェイスで、左側のパネルの「**[!UICONTROL スキーマ]**」をクリックし、「**[!UICONTROL 参照]**」タブの検索バーを使用して、更新するスキーマをすばやく見つけます。

![](../images/computed-attributes/Schemas-Browse.png)

スキーマを見つけたら、名前をクリックして[!DNL Schema Editor]を開き、スキーマに編集を加えることができます。

![](../images/computed-attributes/Schema-Editor.png)

### Mixin の作成

新しい Mixin を作成するには、エディターの左側にある「**[!UICONTROL コンポジション]**」セクションで、「**[!UICONTROL Mixins]**」の隣にある「**[!UICONTROL 追加]**」をクリックします。「**[!UICONTROL Mixin を追加]**」ダイアログが開き、既存の Mixin を確認できます。新しい Mixin を定義するには、「**[!UICONTROL 新しい Mixin を作成]**」のラジオボタンをクリックします。

Mixin に名前と説明を入力し、完了したら「**[!UICONTROL Mixin を追加]**」をクリックします。

![](../images/computed-attributes/Add-mixin.png)

### 追加スキーマ

新しいミックスインが「[!UICONTROL コンポジション]」の下の「[!UICONTROL ミックスイン]」セクションに表示されます。 Mixin の名前をクリックすると、エディターの「**[!UICONTROL 構造]**」セクションに、「**[!UICONTROL フィールドを追加]**」ボタンが複数表示されます。

上位のフィールドを追加するには、スキーマ名の隣にある「**[!UICONTROL フィールドを追加]**」を選択するか、お好きなスキーマ内の任意の場所にフィールドを追加するよう選択することもできます。

「**[!UICONTROL フィールドを追加]**」をクリックすると、テナント ID の名前を付けた新しいオブジェクトが開き、フィールドが正しい名前空間にあることが示されます。そのオブジェクト内に、「**[!UICONTROL 新規フィールド]**」が表示されます。計算済み属性を定義するフィールドの場合。

![](../images/computed-attributes/New-field.png)

### フィールドの設定

エディターの右側にある「**[!UICONTROL フィールドプロパティ]**」セクションを使用して、新しいフィールドの名前、表示名、タイプなど、必要な情報を指定します。

>[!NOTE]
>
>フィールドのタイプは、計算済みの属性値と同じタイプである必要があります。例えば、計算済み属性の値が文字列の場合、スキーマで定義するフィールドは文字列にする必要があります。

完了したら、「**[!UICONTROL 適用]**」をクリックし、フィールドの名前とタイプがエディターの「**[!UICONTROL 構造]**」セクションに表示されます。

![](../images/computed-attributes/Apply.png)

### [!DNL Profile]のスキーマを有効にする

続行する前に、スキーマが[!DNL Profile]に対して有効になっていることを確認してください。 エディターの「**[!UICONTROL 構造]**」セクションでスキーマ名をクリックすると、「**[!UICONTROL スキーマプロパティ]**」タブが表示されます。**[!UICONTROL プロファイル]**&#x200B;のスライダーが青の場合、[!DNL Profile]のスキーマは有効になっています。

>[!NOTE]
>
>[!DNL Profile]のスキーマを有効にすると元に戻せないので、一度有効にした後にスライダをクリックした場合に、無効にするリスクはありません。

![](../images/computed-attributes/Profile.png)

これで、「**[!UICONTROL 保存]**」をクリックして、更新したスキーマを保存し、API を使用して残りのチュートリアルに進むことができるようになりました。

### 計算済み属性の作成 {#create-a-computed-attribute}

計算済み属性フィールドを特定し、スキーマが[!DNL Profile]に対して有効になっていることを確認したら、計算済み属性を設定できます。

まず、作成する計算済み属性の詳細を含むリクエスト本文を使用して、`/config/computedAttributes` エンドポイントに POST リクエストを送信します。

**API 形式**

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
| `name` | 計算済み属性フィールドの名前（文字列）。 |
| `path` | 計算済み属性を含むフィールドへのパス。このパスは、スキーマの `properties` 属性内にあり、パスにフィールド名を含めないでください。パスを書き込む場合は、複数レベルの `properties` 属性を省略します。 |
| `{TENANT_ID}` | テナント ID に慣れていない場合は、[スキーマレジストリ開発者ガイド](../../xdm/api/getting-started.md#know-your-tenant_id)で、テナント ID を見つける手順を参照してください。 |
| `description` | 計算済み属性の説明。複数の計算済み属性を定義した場合は特に便利です。IMS 組織内の他のユーザーが、使用する正しい計算済み属性を判断するのに役立ちます。 |
| `expression.value` | 有効な[!DNL Profile Query Language] (PQL)式。 PQL の詳細とサポートされるクエリへのリンクについては、[PQL の概要](../../segmentation/pql/overview.md)を参照してください。 |
| `schema.name` | 計算済み属性フィールドを含むスキーマの基となるクラス。例：XDM ExperienceEvent クラスに基づくスキーマの場合 `_xdm.context.experienceevent`。 |

**応答** 

正常に作成された計算済み属性は、HTTP ステータス 200（OK）と、新しく作成された計算済み属性の詳細を含む応答本文を返します。これらの詳細には、他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 `id` が含まれます。

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
| `id` | 他の API 操作中に計算済み属性を参照するために使用できる、システムで生成された一意の読み取り専用 ID が含まれます。 |
| `imsOrgId` | 計算済み属性に関連する IMS 組織は、要求で送信される値と一致する必要があります。 |
| `sandbox` | サンドボックスオブジェクトには、計算済み属性が設定されたサンドボックスの詳細が含まれます。この情報は、リクエストで送信されるサンドボックスヘッダーから取得されます。詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。 |
| `positionPath` | リクエストで送信されたフィールドに分解された `path` を含む配列。 |
| `returnSchema.meta:xdmType` | 計算済み属性が保存されるフィールドのタイプ。 |
| `definedOn` | 計算済み属性が定義された和集合スキーマを示す配列。和集合スキーマごとに 1 つのオブジェクトを含みます。つまり、異なるクラスに基づいて計算された属性が複数のスキーマに追加された場合、配列内に複数のオブジェクトが存在する可能性があります。 |
| `active` | 計算済み属性が現在アクティブかどうかを示すブール値。デフォルト値は `true` です。 |
| `type` | 作成されるリソースのタイプ。この場合は「ComputedAttribute」がデフォルト値です。 |
| `createEpoch` および `updateEpoch` | 計算済み属性が作成された時刻と最後に更新された時刻。 |


## 計算済み属性へのアクセス

API を使用して計算済み属性を操作する場合、組織で定義された計算済み属性にアクセスするためのオプションは 2 つあります。1 つ目は、すべての計算済み属性をリストする方法、もう 1 つは、一意の `id` によって固有の計算済み属性を表示する方法です。

すべての計算済み属性をリストする手順と、特定の計算済み属性の表示手順については、以降の節で説明します。

### リストの計算済み属性 {#list-computed-attributes}

IMS 組織は複数の計算済み属性を作成できます。`/config/computedAttributes` エンドポイントに対して GET リクエストを実行すると、組織の既存計算済み属性をすべてリストできます。

**API 形式**

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

成功応答には、`_page` 属性が含まれ、計算済み属性（`totalCount`）の数とページ上の計算済み属性の数（`pageSize`）を提供します。

応答には、1 つ以上のオブジェクトで構成される `children` 配列も含まれ、それぞれに 1 つの計算済み属性の詳細が含まれます。組織に計算済みの属性がない場合、`totalCount` と `pageSize` は「0」（ゼロ）で、`children` 配列は空白になります。

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
| `_page.totalCount` | IMS 組織で定義された計算済み属性の合計数です。 |
| `_page.pageSize` | 結果のこのページで返された計算済み属性の数。`pageSize` が `totalCount` と等しい場合、結果のページが 1 ページのみで、計算済みの属性がすべて返されたことを意味します。等しくない場合は、アクセス可能な結果のページが他にあります。詳細は、`_links.next` を参照してください。 |
| `children` | 1 つ以上のオブジェクトで構成される配列。各オブジェクトには、1 つの計算済み属性の詳細が含まれます。計算済みの属性が定義されていない場合、`children` 配列は空になります。 |
| `id` | 計算済み属性の作成時に自動的に割り当てられる、システムで生成された読み取り専用の一意の値。計算済み属性オブジェクトのコンポーネントの詳細については、このチュートリアルで既に説明した[計算済み属性の作成](#create-a-computed-attribute)の節を参照してください。 |
| `_links.next` | 1 ページの計算済み属性が返される場合、前述の応答例のように、`_links.next` は空のオブジェクトになります。組織に多数の計算済み属性がある場合、`_links.next` 値に対して GET リクエストを送信することでアクセスできる、複数のページで返されます。 |

### 計算済み属性の表示 {#view-a-computed-attribute}

特定の計算済み属性を表示するには、`/config/computedAttributes` エンドポイントに GET リクエストを送信し、計算済み属性 ID をリクエストパスに含めます。

**API 形式**

```http
GET /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 表示する計算済み属性の ID。 |

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

既存の計算済み属性を更新する必要がある場合、これをおこなうには、`/config/computedAttributes` エンドポイントに PATCH リクエストを作成し、更新する計算済みの属性の ID をリクエストパスに含めます。

**API 形式**

```http
PATCH /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 更新する計算済み属性の ID。 |

**リクエスト**

このリクエストでは、[JSON パッチのフォーマット設定](http://jsonpatch.com/)を使用して、「式」フィールドの「値」を更新します。

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
| `{NEW_EXPRESSION_VALUE}` | 有効な[!DNL Profile Query Language] (PQL)式。 PQL の詳細とサポートされるクエリへのリンクについては、[PQL の概要](../../segmentation/pql/overview.md)を参照してください。 |

**応答** 

成功応答は、HTTP ステータス 204（コンテンツなし）と空の応答本文が返されます。削除が成功したことを確認するには、GET リクエストを実行して、計算済み属性を ID で参照できます。

## 計算済み属性の削除

API を使用して、計算済み属性を削除することもできます。これには、`/config/computedAttributes` エンドポイントに DELETE リクエストを送信し、削除する計算済み属性の ID をリクエストパスに含めます。

>[!NOTE]
>
>計算済み属性を削除する場合は、複数のスキーマで使用されている可能性があるので注意が必要です。また、DELETE 操作を元に戻すことはできません。

**API 形式**

```http
DELETE /config/computedAttributes/{ATTRIBUTE_ID}
```

| パラメーター | 説明 |
|---|---|
| `{ATTRIBUTE_ID}` | 削除する計算済み属性の ID。 |

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

削除リクエストが成功すると、HTTP ステータス 200（OK）と空の応答本文が返されます。削除が成功したことを確認するために、GET リクエストを実行して、計算済み属性を ID で参照できます。属性が削除された場合は、HTTP ステータス 404（見つかりません）エラーが表示されます。

## 次の手順

これで、計算済み属性の基本について学び、組織に合わせて定義を開始する準備が整いました。