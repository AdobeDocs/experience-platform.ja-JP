---
title: HTTP API 接続
keywords: ストリーミング；
description: Adobe Experience Platformの HTTP API 宛先を使用して、プロファイルデータをサードパーティの HTTP エンドポイントに送信し、独自の分析を実行したり、Experience Platform外に書き出されたプロファイルデータに対して必要なその他の操作を実行したりします。
exl-id: 165a8085-c8e6-4c9f-8033-f203522bb288
source-git-commit: 30549f31e7ba7f9cfafd2e71fb3ccfb701b9883f
workflow-type: tm+mt
source-wordcount: '2296'
ht-degree: 3%

---

# HTTP API 接続

## 概要 {#overview}

>[!IMPORTANT]
>
> この宛先は次の場所でのみ使用できます： [Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) 顧客。

HTTP API の宛先は [!DNL Adobe Experience Platform] プロファイルデータをサードパーティの HTTP エンドポイントに送信する際に役立つストリーミングの宛先です。

プロファイルデータを HTTP エンドポイントに送信するには、まず [宛先に接続](#connect-destination) in [!DNL Adobe Experience Platform].

## ユースケース {#use-cases}

HTTP API の宛先を使用すると、XDM プロファイルデータとオーディエンスセグメントを汎用の HTTP エンドポイントに書き出すことができます。 ここでは、独自の分析を実行したり、Experience Platformからエクスポートされたプロファイルデータに対して必要なその他の操作を実行したりできます。

HTTP エンドポイントは、お客様独自のシステムまたはサードパーティのソリューションのいずれかになります。

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [宛先のアクティベーションワークフロー](../../ui/activate-segment-streaming-destinations.md#mapping). |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

HTTP API の宛先を使用してExperience Platformからデータを書き出すには、次の前提条件を満たす必要があります。

* REST API をサポートする HTTP エンドポイントが必要です。
* HTTP エンドポイントは、Experience Platformプロファイルスキーマをサポートする必要があります。 HTTP API の宛先では、サードパーティのペイロードスキーマへの変換はサポートされていません。 詳しくは、 [書き出されたデータ](#exported-data) 「 」セクションに、Experience Platform出力スキーマの例を示します。
* HTTP エンドポイントはヘッダーをサポートする必要があります。

>[!TIP]
>
> また、 [Adobe Experience Platform Destination SDK](/help/destinations/destination-sdk/overview.md) ：統合を設定し、HTTP エンドポイントにExperience Platformプロファイルデータを送信します。

## IP アドレスの許可リスト {#ip-address-allowlist}

お客様のセキュリティおよびコンプライアンス要件を満たすために、Experience Platformは HTTP API 宛先に対してできる静的 IP のリストを提供しま許可リストす。 参照： [ストリーミング先の IP アドレス許可リスト](/help/destinations/catalog/streaming/ip-address-allow-list.md) ：する IP の完全なリストを表示しま許可リストす。

## サポートしている認証タイプ {#supported-authentication-types}

HTTP API の宛先は、HTTP エンドポイントに対して複数の認証タイプをサポートします。

* 認証のない HTTP エンドポイント。
* Bearer トークン認証
* [OAuth 2.0 クライアント資格情報](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) 本文形式での認証 [!DNL client ID], [!DNL client secret] および [!DNL grant type] を HTTP リクエストの本文に追加します。

```shell
curl --location --request POST '<YOUR_API_ENDPOINT>' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=<CLIENT_ID>' \
--data-urlencode 'client_secret=<CLIENT_SECRET>'
```

* [OAuth 2.0 クライアント資格情報](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) 基本認証付きで、URL エンコードされた [!DNL client ID] および [!DNL client secret].

```shell
curl --location --request POST 'https://some-api.com/token' \
--header 'Authorization: Basic base64(clientId:clientSecret)' \
--header 'Content-type: application/x-www-form-urlencoded; charset=UTF-8' \
--data-urlencode 'grant_type=client_credentials'
```

* [OAuth 2.0 パスワードの付与](https://www.oauth.com/oauth2-servers/access-tokens/password-grant/).

## 宛先への接続 {#connect-destination}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。この宛先に接続する際は、次の情報を指定する必要があります。

### 認証情報 {#authentication-information}

#### Bearer トークン認証 {#bearer-token-authentication}

次を選択した場合、 **[!UICONTROL Bearer トークン]** HTTP エンドポイントに接続するための認証タイプ。以下のフィールドを入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**:

![bearer トークン認証を使用して HTTP API の宛先に接続できる UI 画面の画像](../../assets/catalog/http/http-api-authentication-bearer.png)

* **[!UICONTROL Bearer トークン]**:bearer トークンを挿入して、HTTP ロケーションに対する認証をおこないます。

#### 認証なし {#no-authentication}

次を選択した場合、 **[!UICONTROL なし]** HTTP エンドポイントに接続するための認証タイプ：

![認証なしで HTTP API の宛先に接続できる UI 画面の画像](../../assets/catalog/http/http-api-authentication-none.png)

この認証を開いた状態で選択する場合は、 **[!UICONTROL 宛先に接続]** およびは、エンドポイントへの接続を確立します。

#### OAuth 2 パスワード認証 {#oauth-2-password-authentication}

次を選択した場合、 **[!UICONTROL OAuth 2 パスワード]** HTTP エンドポイントに接続するための認証タイプ。以下のフィールドを入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**:

![パスワード認証を使用して OAuth 2 を使用し、HTTP API の宛先に接続できる UI 画面の画像](../../assets/catalog/http/http-api-authentication-oauth2-password.png)

* **[!UICONTROL トークン URL にアクセス]**:ユーザー側の URL で、トークンにアクセスし、必要に応じて更新トークンを発行します。
* **[!UICONTROL クライアント ID]**:この [!DNL client ID] システムがAdobe Experience Platformに割り当てる
* **[!UICONTROL クライアント秘密鍵]**:この [!DNL client secret] システムがAdobe Experience Platformに割り当てる
* **[!UICONTROL ユーザー名]**:HTTP エンドポイントにアクセスするユーザー名。
* **[!UICONTROL パスワード]**:HTTP エンドポイントにアクセスするためのパスワード。

#### OAuth 2 クライアント資格情報認証 {#oauth-2-client-credentials-authentication}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_http_clientcredentialstype"
>title="クライアント資格情報のタイプ"
>abstract="選択 **本文のエンコード** を呼び出します。 **基本認証** 認証ヘッダーにクライアント ID とクライアント秘密鍵を含める。 ドキュメントの例を参照してください。"

次を選択した場合、 **[!UICONTROL OAuth 2 クライアント資格情報]** HTTP エンドポイントに接続するための認証タイプ。以下のフィールドを入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**:

![クライアント資格情報認証で OAuth 2 を使用して HTTP API 宛先に接続できる UI 画面の画像](../../assets/catalog/http/http-api-authentication-oauth2-client-credentials.png)

* **[!UICONTROL トークン URL にアクセス]**:ユーザー側の URL で、トークンにアクセスし、必要に応じて更新トークンを発行します。
* **[!UICONTROL クライアント ID]**:この [!DNL client ID] システムがAdobe Experience Platformに割り当てる
* **[!UICONTROL クライアント秘密鍵]**:この [!DNL client secret] システムがAdobe Experience Platformに割り当てる
* **[!UICONTROL クライアント資格情報の種類]**:お使いのエンドポイントでサポートされる OAuth2 クライアント資格情報付与のタイプを選択します。
   * **[!UICONTROL 本文のエンコード]**:この場合、 [!DNL client ID] および [!DNL client secret] 含まれる *リクエストの本文内* を宛先に送信しました。 例については、 [サポートされる認証タイプ](#supported-authentication-types) 」セクションに入力します。
   * **[!UICONTROL 基本認証]**:この場合、 [!DNL client ID] および [!DNL client secret] 含まれる *内 `Authorization` ヘッダー* base64 エンコードされ、宛先に送信された後。 例については、 [サポートされる認証タイプ](#supported-authentication-types) 」セクションに入力します。

### 宛先の詳細 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_http_headers"
>title="ヘッダー"
>abstract="宛先の呼び出しに含めるカスタムヘッダーを、次の形式で入力します。 `header1:value1,header2:value2,...headerN:valueN`"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_http_endpoint"
>title="HTTP エンドポイント"
>abstract="プロファイルデータの送信先の HTTP エンドポイントの URL。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_http_includesegmentnames"
>title="セグメント名を含める"
>abstract="データの書き出しで、書き出すセグメントの名前を含めるかどうかを切り替えます。 このオプションを選択したデータエクスポートの例に関するドキュメントを表示します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_http_includesegmenttimestamps"
>title="セグメントのタイムスタンプを含める"
>abstract="セグメントが作成および更新された際の UNIX タイムスタンプと、セグメントがアクティベーションのために宛先にマッピングされた際の UNIX タイムスタンプをデータエクスポートに含めるかどうかを切り替えます。 このオプションを選択したデータエクスポートの例に関するドキュメントを表示します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_http_queryparameters"
>title="クエリのパラメーター"
>abstract="オプションで、HTTP エンドポイント URL にクエリパラメーターを追加できます。 使用するクエリパラメーターを次のように書式設定します。 `parameter1=value&parameter2=value`."

HTTP エンドポイントへの認証接続を確立したら、宛先に次の情報を指定します。

![HTTP 宛先の詳細に関する入力済みフィールドを示す UI 画面の画像](../../assets/catalog/http/http-api-destination-details.png)

* **[!UICONTROL 名前]**:この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明を入力します。
* **[!UICONTROL ヘッダー]**:宛先の呼び出しに含めるカスタムヘッダーを、次の形式で入力します。 `header1:value1,header2:value2,...headerN:valueN`.
* **[!UICONTROL HTTP エンドポイント]**:プロファイルデータの送信先の HTTP エンドポイントの URL。
* **[!UICONTROL クエリパラメーター]**:オプションで、HTTP エンドポイント URL にクエリパラメーターを追加できます。 使用するクエリパラメーターを次のように書式設定します。 `parameter1=value&parameter2=value`.
* **[!UICONTROL セグメント名を含める]**:データの書き出しで、書き出すセグメントの名前を含めるかどうかを切り替えます。 このオプションを選択した場合のデータエクスポートの例については、 [書き出されたデータ](#exported-data) の節を参照してください。
* **[!UICONTROL セグメントのタイムスタンプを含める]**:セグメントが作成および更新された際の UNIX タイムスタンプと、セグメントがアクティベーションのために宛先にマッピングされた際の UNIX タイムスタンプをデータエクスポートに含めるかどうかを切り替えます。 このオプションを選択した場合のデータエクスポートの例については、 [書き出されたデータ](#exported-data) の節を参照してください。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

### 宛先属性 {#attributes}

内 [[!UICONTROL 属性を選択]](../../ui/activate-streaming-profile-destinations.md#select-attributes) 手順に従い、Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、セグメント認定または他の重要なイベントに続いてプロファイルに関連する更新が発生した場合にのみ、HTTP API 宛先へのプロファイルの書き出し動作を最適化します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によって決定されました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた属性の少なくとも 1 つに対する属性の変更によって決定されました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

### 何がデータエクスポートを決定し、何がエクスポートに含まれるか {#what-determines-export-what-is-included}

特定のプロファイルに対して書き出されるデータに関しては、 *は、HTTP API 宛先へのデータ書き出しを決定するものです。* および *エクスポートに含まれるデータ*.

| 宛先の書き出しを決定する要素 | 宛先の書き出しに含まれる内容 |
|---------|----------|
| <ul><li>マッピングされた属性とセグメントは、宛先の書き出しのキューとして機能します。 つまり、マッピングされたセグメントの状態（null から適合済み、適合済み/既存から既存へ）が変更されたり、マッピングされた属性が更新された場合、宛先の書き出しがキックオフされます。</li><li>ID は現在 HTTP API の宛先にマッピングできないので、特定のプロファイルの ID の変更によって宛先の書き出しも決まります。</li><li>属性に対する変更は、同じ値であるかどうかに関わらず、属性に対する更新として定義されます。 つまり、値自体が変更されていない場合でも、属性の上書きは変更と見なされます。</li></ul> | <ul><li>（最新のメンバーシップステータスを持つ）すべてのセグメントは、データフローにマッピングされているかどうかに関係なく、 `segmentMembership` オブジェクト。</li><li>内のすべての ID `identityMap` オブジェクトも含まれます (Experience Platformは、現在、HTTP API の宛先で ID マッピングをサポートしていません )。</li><li>マッピングされた属性のみが宛先エクスポートに含まれます。</li></ul> |

{style=&quot;table-layout:fixed&quot;}

例えば、このデータフローを HTTP 宛先に対して考えてみましょう。この宛先では、3 つのセグメントがデータフローで選択され、4 つの属性が宛先にマッピングされます。

![HTTP API 宛先のデータフロー](/help/destinations/assets/catalog/http/profile-export-example-dataflow.png)

宛先へのプロファイルエクスポートは、いずれかの *3 つのマッピングされたセグメント*. ただし、データエクスポートでは、 `segmentMembership` オブジェクト ( [書き出されたデータ](#exported-data) の節を参照 )、その特定のプロファイルがそのメンバーの場合は、その他のマッピングされていないセグメントが表示されることがあります。 プロファイルが DeLorean Cars セグメントで顧客の資格を得ている一方で、「Back to the Future」映画や SF ファンセグメントのメンバーでもある場合、他の 2 つのセグメントも `segmentMembership` データエクスポートのオブジェクト（データフローでマッピングされていない場合）。

プロファイル属性の観点から、上でマッピングした 4 つの属性に対する変更によって、書き出し先が決まり、プロファイルに存在する 4 つのマッピング済み属性のいずれかがデータ書き出しに表示されます。

## 履歴データのバックフィル {#historical-data-backfill}

新しいセグメントを既存の宛先に追加する場合、または新しい宛先を作成してセグメントをマッピングする場合、Experience Platformは宛先にセグメントの資格情報の履歴データをエクスポートします。 セグメントに適合するプロファイル *前* セグメントが宛先に追加され、約 1 時間以内に宛先に書き出されます。

## 書き出したデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは、 [!DNL HTTP] の宛先を JSON 形式で指定します。 例えば、以下のエクスポートには、特定のセグメントに適合し、別の 2 つのセグメントのメンバーであり、別のセグメントから離脱したプロファイルが含まれています。 書き出しには、プロファイル属性の名、姓、生年月日、個人の電子メールアドレスも含まれます。 このプロファイルの ID は、ECID と電子メールです。

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
         "status":"existing"
      },
      "947c1c46-008d-40b0-92ec-3af86eaf41c1":{
         "lastQualificationTime":"2021-08-25T23:37:33Z",
         "status":"existing"
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

次に、 **[!UICONTROL セグメント名を含める]** および **[!UICONTROL セグメントのタイムスタンプを含める]** options:

+++ 以下のデータエクスポートのサンプルでは、 `segmentMembership` セクション

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "existing",
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

+++ 以下のデータエクスポートの例では、 `segmentMembership` セクション

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "existing",
            "createdAt": 1648553325000,
            "updatedAt": 1648553330000,
            "mappingCreatedAt": 1649856570000,
            "mappingUpdatedAt": 1649856570000,
          }
        }
      }
```

+++

## 制限および再試行ポリシー {#limits-retry-policy}

95%の確率で、Experience Platformは、各データフローの HTTP 宛先への 1 秒あたり 10.000 リクエスト未満の割合で正常に送信されたメッセージに対して、10 分未満のスループット遅延を提供しようとします。

HTTP API 宛先へのリクエストが失敗した場合、Experience Platformは失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。