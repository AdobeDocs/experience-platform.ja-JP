---
description: 宛先に対して認証メカニズムを設定する方法を説明し、選択した認証方法に応じて UI でユーザーに表示される内容を確認します。
title: 顧客認証設定
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '1094'
ht-degree: 100%

---


# 顧客認証設定

Experience Platform は、パートナーおよびお客様が使用できる認証プロトコルに優れた柔軟性を提供します。任意の業界標準の認証方法（[!DNL OAuth2]、ベアラートークン認証、パスワード認証、その他多数）をサポートするように宛先を設定できます。

このページでは、好みの認証方法を使用して宛先を設定する方法について説明します。宛先を作成する際に使用する認証設定に基づいて、Experience Platform UI で宛先に接続する際に、顧客には、様々なタイプの認証ページが表示されます。

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、以下の宛先設定の概要ページを参照してください。

* [Destination SDK を使用したストリーミング宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-destination-configuration)

顧客は、Platform から宛先にデータを書き出す前に、[宛先接続](../../../ui/connect-destination.md)チュートリアルで説明されている手順に従うことで、Experience Platform と宛先の間で新しい接続を作成する必要があります。

Destination SDK で[宛先を作成](../../authoring-api/destination-configuration/create-destination-configuration.md)する場合、`customerAuthenticationConfigurations` セクションが、[認証画面](../../../ui/connect-destination.md#authenticate)で顧客に何が表示されるかを定義します。宛先認証タイプに応じて、顧客は、様々な認証の詳細を指定する必要があります。以下に例を示します。

* [基本認証](#basic)を使用する宛先の場合、ユーザーは、Experience Platform UI 認証ページで直接ユーザー名およびパスワードを指定する必要があります。
* [ベアラー認証](#bearer)を使用する宛先の場合、ユーザーは、ベアラートークンを指定する必要があります。
* [OAuth2 認証](#oauth2)を使用する宛先の場合、ユーザーは、資格情報を使用してログインできる、宛先のログインページにリダイレクトされます。
* [Amazon S3](#s3) 宛先の場合、ユーザーは、[!DNL Amazon S3] アクセスキーおよび秘密鍵を指定する必要があります。
* [Azure Blob](#blob) 宛先の場合、ユーザーは、[!DNL Azure Blob] 接続文字列を指定する必要があります。

`/authoring/destinations` エンドポイントを介して顧客認証の詳細を設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての顧客認証設定を説明し、宛先に設定する認証方法に基づいて、Experience Platform UI で顧客に何が表示されるかを示します。

>[!IMPORTANT]
>
>顧客認証設定では、パラメーターを設定する必要はありません。宛先設定を[作成](../../authoring-api/destination-configuration/create-destination-configuration.md)または[更新](../../authoring-api/destination-configuration/update-destination-configuration.md)する際に、このページに表示されるスニペットを API 呼び出しにコピー＆ペーストできます。ユーザーには、Platform UI に対応する認証画面が表示されます。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○ |

## 認証ルール設定 {#authentication-rule}

このページで説明されている任意の顧客認証設定を使用する場合、以下に示すように、常に、[宛先配信](destination-delivery.md)の `authenticationRule` パラメーターを `"CUSTOMER_AUTHENTICATION"` に設定します。

```json {line-numbers="true" highlight="4"
{
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}"
      }
   ]
}
```

## 基本認証 {#basic}

Experience Platform のリアルタイム（ストリーミング）統合では、基本認証がサポートされます。

基本認証タイプを設定する場合、ユーザーは、宛先に接続するために、ユーザー名およびパスワードを入力する必要があります。

![基本認証での UI レンダリング](../../assets/functionality/destination-configuration/basic-authentication-ui.png)

宛先用に基本認証を設定するには、以下に示すように、`/destinations` エンドポイントを介して `customerAuthenticationConfigurations` セクションを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"BASIC"
   }
]
```

## ベアラー認証 {#bearer}

ベアラー認証タイプを設定する場合、ユーザーは接続先から取得したベアラートークンを入力する必要があります。

![ベアラー認証での UI レンダリング](../../assets/functionality/destination-configuration/bearer-authentication-ui.png)

宛先用にベアラー認証を設定するには、以下に示すように、`/destinations` エンドポイントを介して `customerAuthenticationConfigurations` セクションを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"BEARER"
   }
]
```

## OAuth 2 認証 {#oauth2}

ユーザーが「**[!UICONTROL 宛先に接続]**」を選択すると、以下の Twitter カスタムオーディエンスの宛先の例のように、宛先への OAuth 2 認証フローがトリガーされます。宛先エンドポイントへの OAuth 2 認証の設定について詳しくは、専用の [Destination SDK OAuth 2 認証ページ](oauth2-authentication.md)をお読みください。

![OAuth 2 認証での UI レンダリング](../../assets/functionality/destination-configuration/oauth2-authentication-ui.png)

宛先用に [!DNL OAuth2] 認証を設定するには、以下に示すように、`/destinations` エンドポイントを介して `customerAuthenticationConfigurations` セクションを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"OAUTH2"
   }
]
```

## Amazon S3 認証 {#s3}

Experience Platform では、ファイルベースの宛先に対して、[!DNL Amazon S3] 認証がサポートされています。

Amazon S3 認証タイプを設定する際に、ユーザーは S3 資格情報を入力する必要があります。

![S3 認証での UI レンダリング](../../assets/functionality/destination-configuration/s3-authentication-ui.png)

宛先用に [!DNL Amazon S3] 認証を設定するには、以下に示すように、`/destinations` エンドポイントを介して `customerAuthenticationConfigurations` セクションを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"S3"
   }
]
```

## Azure Blob 認証  {#blob}

Experience Platform では、ファイルベースの宛先に対して、[!DNL Azure Blob Storage] 認証がサポートされています。

Azure Blob 認証タイプを設定する際に、ユーザーは接続文字列を入力する必要があります。

![Blob 認証での UI レンダリング](../../assets/functionality/destination-configuration/blob-authentication-ui.png)

宛先用に [!DNL Azure Blob] 認証を設定するには、以下に示すように、`/destinations` エンドポイントで `customerAuthenticationConfigurations` パラメーターを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"AZURE_CONNECTION_STRING"
   }
]
```

## [!DNL Azure Data Lake Storage] 認証 {#adls}

Experience Platform では、ファイルベースの宛先に対して、[!DNL Azure Data Lake Storage] 認証がサポートされています。

[!DNL Azure Data Lake Storage] 認証タイプを設定する際に、ユーザーは、Azure サービスプリンシパル資格情報およびテナント情報を入力する必要があります。

![[!DNL Azure Data Lake Storage] 認証での UI レンダリング](../../assets/functionality/destination-configuration/adls-authentication-ui.png)

宛先用に [!DNL Azure Data Lake Storage]（ADLS）認証を設定するには、以下に示すように、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"AZURE_SERVICE_PRINCIPAL"
   }
]
```

## パスワード認証を使用した SFTP

Experience Platform では、ファイルベースの宛先に対して、パスワードを使用した [!DNL SFTP] 認証がサポートされています。

パスワード認証タイプで SFTP を設定する際に、ユーザーは SFTP のユーザー名とパスワード、SFTP ドメインとポート（デフォルトポートは 22）を入力する必要があります。

![パスワード認証を使用した SFTP での UI レンダリング](../../assets/functionality/destination-configuration/sftp-password-authentication-ui.png)

パスワードを使用した SFTP 認証を宛先に設定するには、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを以下のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"SFTP_WITH_PASSWORD"
   }
]
```

## SSH キー認証を使用した SFTP

Experience Platform では、ファイルベースの宛先に対して、[!DNL SSH] キーを使用した [!DNL SFTP] 認証がサポートされています。

SSH キー認証タイプで SFTP を設定する際に、ユーザーは SFTP のユーザー名と SSH キー、および SFTP ドメインとポート（デフォルトポートは 22）を入力する必要があります。

![SSH キー認証を使用した SFTP での UI レンダリング](../../assets/functionality/destination-configuration/sftp-key-authentication-ui.png)

SSH キーを使用した SFTP 認証を宛先に設定するには、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを以下のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"SFTP_WITH_SSH_KEY"
   }
]
```

## [!DNL Google Cloud Storage] 認証 {#gcs}

Experience Platform では、ファイルベースの宛先に対して、[!DNL Google Cloud Storage] 認証がサポートされています。

[!DNL Google Cloud Storage] 認証タイプを設定する際に、ユーザーは、[!DNL Google Cloud Storage] [!UICONTROL アクセスキー ID] および[!UICONTROL シークレットアクセスキー]を入力する必要があります。

![Google Cloud Storage 認証での UI レンダリング](../../assets/functionality/destination-configuration/google-cloud-storage-ui.png)

宛先用に [!DNL Google Cloud Storage] 認証を設定するには、以下に示すように、`/destinations` エンドポイントで `customerAuthenticationConfigurations` パラメーターを設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"GOOGLE_CLOUD_STORAGE"
   }
]
```

## 次の手順 {#next-steps}

この記事を読むことで、宛先プラットフォームに対するユーザー認証の設定方法について、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)