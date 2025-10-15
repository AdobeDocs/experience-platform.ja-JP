---
keywords: Amazon Kinesis;Kinesis の宛先；Kinesis
title: Amazon Kinesis 接続
description: Amazon Kinesis ストレージへのリアルタイムアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングします。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: b40117ef-6ad0-48a9-bbcb-97c6f6d1dce3
source-git-commit: 7502810ff329a31f2fdaf6797bc7672118555e6a
workflow-type: tm+mt
source-wordcount: '1978'
ht-degree: 52%

---

# [!DNL Amazon Kinesis] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
> この宛先を使用できるのは [Adobe Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) の顧客のみです。

[!DNL Kinesis Data Streams] による [!DNL Amazon Web Services] サービスを使用すると、データレコードの大きなストリームをリアルタイムで収集および処理できます。

[!DNL Amazon Kinesis] ストレージへのリアルタイムアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングできます。

* [!DNL Amazon Kinesis] について詳しくは、[Amazon ドキュメント &#x200B;](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) を参照してください。
* プログラムによってに接続する [!DNL Amazon Kinesis] は、[&#x200B; ストリーミング宛先 API チュートリアル &#x200B;](../../api/streaming-destinations.md) を参照してください。
* Experience Platform ユーザーインターフェイスを使用して [!DNL Amazon Kinesis] に接続するには、以下の節を参照してください。

![UI でのAmazon Kinesis](../../assets/catalog/cloud-storage/amazon-kinesis/catalog.png)

## ユースケース {#use-cases}

[!DNL Amazon Kinesis] などのストリーミング宛先を使用すると、価値の高いセグメント化イベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、ある見込み客がダウンロードしたホワイトペーパーによって、「コンバージョンする傾向の高い」セグメントに見込み客を選定するとします。 見込み客が含まれるオーディエンスを [!DNL Amazon Kinesis] の宛先にマッピングすると、[!DNL Amazon Kinesis] でこのイベントを受け取ります。 そこで、エンタープライズの IT システムで最も効果が高いと思われる通り、日曜大工のアプローチを採用し、イベントの上にビジネスロジックを記述できます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#ip-address-allowlist}

お客様のセキュリティおよびコンプライアンスの要件を満たすために、Experience Platformは [!DNL Amazon Kinesis] しい宛先に許可リストできる静的 IP のリストを提供します。 許可リストに使用できる IP のリストについて詳しくは、[ストリーミング宛先用 IP アドレスの許可リスト](/help/destinations/catalog/streaming/ip-address-allow-list.md)を参照してください。

## 必要な [!DNL Amazon Kinesis] 権限 {#required-kinesis-permission}

[!DNL Amazon Kinesis] ストリームに正常に接続してデータを書き出すには、Experience Platformに次のアクションの権限が必要です。

* `kinesis:ListStreams`
* `kinesis:PutRecord`
* `kinesis:PutRecords`

これらのパーミッションは [!DNL Kinesis] コンソールを通じて整理され、Experience Platform ユーザーインターフェイスで Kinesis の出力先を設定した後、Experience Platformによってチェックされます。

次の例は、[!DNL Kinesis] の宛先にデータを正常に書き出すために必要な最小アクセス権を示しています。

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kinesis:ListStreams",
                "kinesis:PutRecord",
                "kinesis:PutRecords"
            ],
            "Resource": [
                "arn:aws:kinesis:us-east-2:901341027596:stream/*"
            ]
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `kinesis:ListStreams` | Amazon Kinesis データストリームを一覧表示するアクション。 |
| `kinesis:PutRecord` | 単一のデータレコードを Kinesis データストリームに書き込むアクション。 |
| `kinesis:PutRecords` | 1 回の呼び出しで複数のデータレコードを Kinesis データストリームに書き込むアクション。 |

{style="table-layout:auto"}

データストリームのアクセス制御 [!DNL Kinesis] ついて詳しくは、次の [[!DNL Kinesis]  ドキュメント &#x200B;](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html) を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。この宛先に接続する際は、次の情報を指定する必要があります。

### 認証情報 {#authentication-information}

以下のフィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![Amazon Kinesis 認証の詳細に関する入力済みフィールドを示す UI 画面の画像 &#x200B;](../../assets/catalog/cloud-storage/amazon-kinesis/kinesis-authentication-fields.png)

* **[!DNL Amazon Web Services]アクセスキーと秘密鍵**: [!DNL Amazon Web Services] で `access key - secret access key` ペアを生成して、[!DNL Amazon Kinesis] アカウントにExperience Platform アクセス権を付与します。 詳しくは、[Amazon Web Services に関するドキュメント](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。
* **[!UICONTROL Region]**：データのストリーミング先 [!DNL Amazon Web Services] リージョンを示します。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_kinesis_includesegmentnames"
>title="セグメント名を含める"
>abstract="書き出すオーディエンスの名前をデータの書き出しに含めるかどうかを切り替えます。このオプションを選択したデータの書き出しの例に関するドキュメントを表示します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_kinesis_includesegmenttimestamps"
>title="セグメントのタイムスタンプを含める"
>abstract="オーディエンスが作成および更新された際の UNIX タイムスタンプと、アクティブ化のためにオーディエンスが宛先にマップされた際の UNIX タイムスタンプをデータの書き出しに含めるかどうかを切り替えます。このオプションを選択したデータの書き出しの例に関するドキュメントを表示します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Amazon Kinesis の宛先の詳細に関する入力済みフィールドを示す UI 画面の画像 &#x200B;](../../assets/catalog/cloud-storage/amazon-kinesis/kinesis-destination-details.png)

* **[!UICONTROL 名前]**:[!DNL Amazon Kinesis] への接続の名前を指定します
* **[!UICONTROL 説明]**:[!DNL Amazon Kinesis] への接続の説明を指定します。
* **[!UICONTROL ストリーム]**:[!DNL Amazon Kinesis] アカウントの既存のデータストリームの名前を指定します。 Experience Platformはこのストリームにデータを書き出します。
* **[!UICONTROL セグメント名を含める]**：書き出すオーディエンスの名前をデータの書き出しに含めるかどうかを切り替えます。 このオプションを選択したデータの書き出しの例については、[書き出されたデータ](#exported-data)の節を参照してください。
* **[!UICONTROL セグメントのタイムスタンプを含める]**：オーディエンスが作成および更新された際の UNIX タイムスタンプと、アクティブ化のためにオーディエンスが宛先にマッピングされた際の UNIX タイムスタンプをデータの書き出しに含めるかどうかを切り替えます。 このオプションを選択したデータの書き出しの例については、[書き出されたデータ](#exported-data)の節を参照してください。

<!--

>[!IMPORTANT]
>
>Experience Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* [&#x200B; 同意ポリシーの評価 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) は、現在、Amazon Kinesis 宛先への書き出しではサポートされていません。 [詳細情報](/help/destinations/ui/activate-streaming-profile-destinations.md#consent-policy-evaluation)

この宛先にオーディエンスをアクティブ化する手順については、[&#x200B; ストリーミングプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 &#x200B;](../../ui/activate-streaming-profile-destinations.md) を参照してください。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、オーディエンスの選定または他の重要なイベントに続いてプロファイルに関連する更新が発生した際に宛先へデータを書き出すためにのみ、[!DNL Amazon Kinesis] ースの宛先へのプロファイルの書き出し動作を最適化します。 プロファイルは、以下の状況で宛先に書き出されます。

* 宛先にマッピングされた 1 つ以上のオーディエンスのオーディエンスメンバーシップの変更によって、プロファイルの更新が決定された場合。 例えば、プロファイルは、宛先にマッピングされたいずれかのオーディエンスに適合しているか、宛先にマッピングされたいずれかのオーディエンスから退出しています。
* プロファイルの更新が、[ID マップ](/help/xdm/field-groups/profile/identitymap.md)の変更によって決定する場合。例えば、宛先にマッピングされたオーディエンスの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた属性のうち、少なくとも 1 つの属性が変更されたことで判断されました。例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合で、適切な更新が行われたプロファイルのみが宛先に書き出されます。例えば、宛先フローにマッピングされたオーディエンスに 100 人のメンバーがいて、5 つの新しいプロファイルがセグメントに適合している場合、宛先への書き出しは増分で行われ、5 つの新しいプロファイルのみが含まれます。

変更箇所に関わらず、マッピングされたすべての属性がプロファイルに対して書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされた属性がすべて書き出されます。

### データの書き出しを決定する要素と、書き出しに含まれる内容 {#what-determines-export-what-is-included}

特定のプロファイルについて書き出されるデータに関しては、*[!DNL Amazon Kinesis] ースの宛先へのデータ書き出しを決定する要因は何か* および *書き出しに含まれるデータはどれか* という 2 つの異なる概念を理解することが重要です。

| 宛先の書き出しを決定する要素 | 宛先の書き出しに含まれる内容 |
|---------|----------|
| <ul><li>マッピングされた属性とセグメントは、宛先の書き出しのキューとして機能します。つまり、プロファイルの `segmentMembership` ステータスが `realized` または `exiting` に変更されたり、マッピングされた属性が更新されたりすると、宛先の書き出しが開始されます。</li><li>現在 ID は [!DNL Amazon Kinesis] の宛先にマッピングできないので、特定のプロファイルの ID が変わると、宛先の書き出しも決まります。</li><li>属性の変更は、同じ値であるかどうかに関わらず、属性に対する更新として定義されます。つまり、値自体が変更されていない場合でも、属性の上書きは変更と見なされます。</li></ul> | <ul><li>`segmentMembership` オブジェクトには、アクティブ化データフローでマッピングされたセグメントが含まれます。このセグメントについて、プロファイルのステータスが選定またはセグメント出口イベントの後に変更されました。なお、これらのセグメントが、アクティブ化データフローでマッピングされたセグメントと同じ[結合ポリシー](/help/profile/merge-policies/overview.md)に属する場合、プロファイルが適していた他のマッピングされていないセグメントを宛先の書き出しに含めることができます。 </li><li>`identityMap` オブジェクト内のすべての ID も含まれます（Experience Platformでは現在、[!DNL Amazon Kinesis] の宛先での ID マッピングをサポートしていません）。</li><li>マッピングされた属性のみが宛先の書き出しに含まれます。</li></ul> |

{style="table-layout:fixed"}

>[!BEGINSHADEBOX]

例えば、[!DNL Amazon Kinesis] の宛先に対するこのデータフローについて考えてみましょう。ここでは、3 つのオーディエンスがデータフローで選択され、4 つの属性が宛先にマッピングされます。

![Amazon Kinesis 宛先のデータフロー &#x200B;](../../assets/catalog/http/profile-export-example-dataflow.png)

宛先へのプロファイルの書き出しは、*3 つのマッピングされたセグメント*&#x200B;のいずれかに適合またはいずれかを離脱するプロファイルによって決定されます。データの書き出しでは、`segmentMembership` オブジェクト（以下の [&#x200B; 書き出されたデータ &#x200B;](#exported-data) の節を参照）に、その特定のプロファイルがメンバーであり、書き出しをトリガーしたオーディエンスと同じ結合ポリシーを共有している場合、他のマッピングされたオーディエンスが表示されることがあります。 プロファイルが **デロリアンを保有する顧客** セグメントの対象となり、かつ **基本サイトアクティブおよび市区町村 – ダラス** セグメントのメンバーでもある場合、他の 2 つのオーディエンスもデータ書き出しの `segmentMembership` オブジェクトに存在します。これは、これらが **デロリアンを保有する顧客** セグメントと同じ結合ポリシーを共有する場合、データフローでマッピングされるためです。

プロファイル属性の観点から、上記でマッピングした 4 つの属性に対する変更によって、書き出しの宛先が決定し、プロファイルに存在する 4 つのマッピング済み属性のいずれかがデータ書き出しに表示されます。

>[!ENDSHADEBOX]

## 履歴データのバックフィル {#historical-data-backfill}

既存の宛先に新しいオーディエンスを追加する場合、または新しい宛先を作成し、それにオーディエンスをマッピングする場合、Experience Platformは、履歴オーディエンス選定データを宛先に書き出します。 オーディエンス *以前* に適合し、オーディエンスが宛先に追加されたプロファイルは、約 1 時間以内に宛先に書き出されます。

## 書き出したデータ {#exported-data}

書き出された [!DNL Experience Platform] データは、JSON 形式で [!DNL Amazon Kinesis] の宛先に格納されます。例えば、以下の書き出しには、特定のセグメントに適合し、別の 2 つのセグメントのメンバーであり、別のセグメントから離脱したプロファイルが含まれています。 書き出しには、プロファイル属性の名、姓、生年月日、個人メールアドレスも含まれます。 このプロファイルの ID は、ECID とメールです。

```json
{
  "person": {
    "birthDate": "YYYY-MM-DD",
    "name": {
      "firstName": "John",
      "lastName": "Doe"
    }
  },
  "personalEmail": {
    "address": "john.doe@acme.com"
  },
  "segmentMembership": {
   "ups":{
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93":{
         "lastQualificationTime":"2022-01-11T21:24:39Z",
         "status":"exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae":{
         "lastQualificationTime":"2022-01-02T23:37:33Z",
         "status":"realized"
      },
      "947c1c46-008d-40b0-92ec-3af86eaf41c1":{
         "lastQualificationTime":"2021-08-25T23:37:33Z",
         "status":"realized"
      },
      "5114d758-ce71-43ba-b53e-e2a91d67b67f":{
         "lastQualificationTime":"2022-01-11T23:37:33Z",
         "status":"realized"
      }
   }
},
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```

書き出されたデータのその他の例を以下に示します。これらは「**[!UICONTROL セグメント名を含める]**」および「**[!UICONTROL セグメントのタイムスタンプを含める]**」オプションに対して接続データフローで選択した UI 設定によって異なります。

+++ 以下のデータの書き出しの例では、`segmentMembership` セクションにオーディエンス名が含まれています

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "realized",
            "createdAt": 1648553325000,
            "updatedAt": 1648553330000,
            "mappingCreatedAt": 1649856570000,
            "mappingUpdatedAt": 1649856570000,
            "name": "First name equals John"
          }
        }
      }
```

+++

+++ 以下のデータの書き出しの例では、`segmentMembership` セクションにオーディエンスのタイムスタンプが含まれています

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "realized",
            "createdAt": 1648553325000,
            "updatedAt": 1648553330000,
            "mappingCreatedAt": 1649856570000,
            "mappingUpdatedAt": 1649856570000,
          }
        }
      }
```

+++

## 制限と再試行ポリシー {#limits-retry-policy}

Experience Platform は 95％ の確率で、HTTP 宛先の各データフローにおいて、送信に成功したメッセージのスループット待ち時間を 10 分未満、リクエスト数を 1 秒あたり 10,000 件未満で提供しようと試みます。

HTTP API 宛先へのリクエストが失敗した場合、Experience Platform は失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。

>[!MORELIKETHIS]
>
>* [Amazon Kinesis に接続し、Flow Service API を使用してデータを有効化する &#x200B;](../../api/streaming-destinations.md)
>* [Azure Event Hubs の宛先 &#x200B;](./azure-event-hubs.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)
