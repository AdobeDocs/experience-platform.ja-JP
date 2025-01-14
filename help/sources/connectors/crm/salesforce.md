---
title: Salesforce Source コネクタの概要
description: API またはユーザーインターフェイスを使用してSalesforceをAdobe Experience Platformに接続する方法について説明します。
exl-id: 597778ad-3cf8-467c-ad5b-e2850967fdeb
source-git-commit: f62e13e97cc82fef759d06b94337f4cc25d4fb10
workflow-type: tm+mt
source-wordcount: '1533'
ht-degree: 11%

---

# [!DNL Salesforce]

>[!IMPORTANT]
>
>Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Salesforce] ソースを使用できるようになりました。 AWSで実行されるExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platformインフラストラクチャについて詳しくは、[Experience Platformマルチクラウドの概要 ](../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティの CRM システムからのデータ取り込みをサポートしています。CRM プロバイダーのサポートは [!DNL Salesforce] を含みます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Salesforce] から XDM へのフィールドマッピング

[!DNL Salesforce] と Platform の間にソース接続を確立するには、[!DNL Salesforce] のソースデータフィールドを、Platform に取り込む前に、適切なターゲット XDM フィールドにマッピングする必要があります。

[!DNL Salesforce] データセットと Platform 間のフィールドマッピングルールについて詳しくは、次を参照してください。

- [連絡先](../adobe-applications/mapping/salesforce.md#contact)
- [リード数](../adobe-applications/mapping/salesforce.md#lead)
- [アカウント](../adobe-applications/mapping/salesforce.md#account)
- [商談](../adobe-applications/mapping/salesforce.md#opportunity)
- [商談連絡先の役割](../adobe-applications/mapping/salesforce.md#opportunity-contact-role)
- [キャンペーン](../adobe-applications/mapping/salesforce.md#campaign)
- [キャンペーンメンバー](../adobe-applications/mapping/salesforce.md#campaign-member)
- [アカウント連絡先の関係](../adobe-applications/mapping/salesforce.md#account-contact-relation)

## [!DNL Salesforce] 名前空間とスキーマ自動生成ユーティリティの設定

[!DNL Salesforce] ソースを [!DNL B2B-CDP] の一部として使用するには、まず [!DNL Postman] ユーティリティを設定して、[!DNL Salesforce] 名前空間とスキーマを自動生成する必要があります。 次のドキュメントでは、[!DNL Postman] ユーティリティの設定に関する追加情報を示します。

- この [GitHub リポジトリ ](https://github.com/adobe/experience-platform-postman-samples/tree/master/Postman%20Collections/CDP%20Namespaces%20and%20Schemas%20Utility) から、名前空間およびスキーマ自動生成ユーティリティのコレクションと環境をダウンロードできます。
- 必要なヘッダーの値を収集する方法やサンプル API 呼び出しを読み取る方法など、Platform API の使用について詳しくは、[Platform API の概要 ](../../../landing/api-guide.md) を参照してください。
- Platform API の資格情報の生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。
- Platform API の [!DNL Postman] の設定方法について詳しくは、[ 開発者コンソールとの設定  [!DNL Postman]](../../../landing/postman.md) のチュートリアルを参照してください。

Platform 開発者コンソールをセットアップす [!DNL Postman] と、適切な環境値の [!DNL Postman] 環境への適用を開始できます。

+++変数テーブルガイドの表示

次の表に、値の例と、[!DNL Postman] 環境へのデータ入力に関する追加情報を示します。

| 変数 | 説明 | 例 |
| --- | --- | --- |
| `CLIENT_SECRET` | `{ACCESS_TOKEN}` ータの生成に使用される一意の ID。 サー `{CLIENT_SECRET}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{CLIENT_SECRET}` |
| `JWT_TOKEN` | JSON web トークン（JWT）は、{ACCESS_TOKEN} ータの生成に使用される認証資格情報です。 サー `{JWT_TOKEN}` スの生成方法について詳しくは、[Experience PlatformAPI の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `{JWT_TOKEN}` |
| `API_KEY` | Experience PlatformAPI への呼び出しの認証に使用される一意の ID。 サー `{API_KEY}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `c8d9a2f5c1e03789bd22e8efdd1bdc1b` |
| `ACCESS_TOKEN` | Experience PlatformAPI を呼び出すために必要な認証トークン。 サー `{ACCESS_TOKEN}` スの取得方法について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md) に関するチュートリアルを参照してください。 | `Bearer {ACCESS_TOKEN}` |
| `META_SCOPE` | [!DNL Marketo] に関しては、この値は固定で、常に `ent_dataservices_sdk` に設定されます。 | `ent_dataservices_sdk` |
| `CONTAINER_ID` | `global` コンテナには、標準AdobeおよびExperience Platformパートナー提供のすべてのクラス、スキーマフィールドグループ、データタイプおよびスキーマが格納されます。 [!DNL Marketo] に関しては、この値は固定で、常に `global` に設定されます。 | `global` |
| `PRIVATE_KEY` | API に対する [!DNL Postman] インスタンスのExperience Platformに使用される資格情報。 コンテン {PRIVATE_KEY} の取得方法については、開発者コンソールの設定および [ 開発者コンソールの設定および  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。 | `{PRIVATE_KEY}` |
| `TECHNICAL_ACCOUNT_ID` | Adobe I/Oへの統合に使用する資格情報。 | `D42AEVJZTTJC6LZADUBVPA15@techacct.adobe.com` |
| `IMS` | Identity Management System （IMS）は、Adobe サービスに対して認証を行うためのフレームワークを提供します。 [!DNL Marketo] に関しては、この値は固定で、常に `ims-na1.adobelogin.com` に設定されます。 | `ims-na1.adobelogin.com` |
| `IMS_ORG` | 製品およびサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる法人組織。 `{ORG_ID}` ーザー情報の取得方法については、[Developer Console の設定および  [!DNL Postman]](../../../landing/postman.md) に関するチュートリアルを参照してください。 | `ABCEH0D9KX6A7WA7ATQE0TE@adobeOrg` |
| `SANDBOX_NAME` | 使用している仮想サンドボックスパーティションの名前。 | `prod` |
| `TENANT_ID` | 作成するリソースの名前空間が適切に設定され、組織内に含まれていることを確認するために使用される ID。 | `b2bcdpproductiontest` |
| `PLATFORM_URL` | API 呼び出しを行う URL エンドポイント。 この値は固定で、常に `http://platform.adobe.io/` に設定されます。 | `http://platform.adobe.io/` |
| `munchkinId` | [!DNL Marketo] アカウントの一意の ID。 インスタンスの取得方法について詳しくは、[ インスタンスの認証 ](../adobe-applications/marketo/marketo-auth.md) に関するチュー `munchkinId` リアルを参照してください  [!DNL Marketo]  | `123-ABC-456` |
| `sfdc_org_id` | [!DNL Salesforce] アカウントの組織 ID。 [!DNL Salesforce] 組織 ID の取得について詳しくは、次の [[!DNL Salesforce]  ガイド ](https://help.salesforce.com/articleView?id=000325251&amp;type=1&amp;mode=1) を参照してください。 | `00D4W000000FgYJUA0` |
| `has_abm` | [!DNL Marketo Account-Based Marketing] を購読しているかどうかを示すブール値。 | `false` |
| `has_msi` | [!DNL Marketo Sales Insight] を購読しているかどうかを示すブール値。 | `false` |

{style="table-layout:auto"}

+++

### スクリプトの実行

[!DNL Postman] コレクションと環境を設定すると、[!DNL Postman] インターフェイスを使用してスクリプトを実行できます。

[!DNL Postman] インターフェイスで、自動生成ユーティリティのルートフォルダーを選択し、上部のヘッダーから「**[!DNL Run]**」を選択します。

![root-folder](../../images/tutorials/create/salesforce/root-folder.png)

[!DNL Runner] インターフェイスが表示されます。 ここから、すべてのチェックボックスが選択されていることを確認してから選択し **[!DNL Run Namespaces and Schemas Autogeneration Utility]** す。

![run-generator](../../images/tutorials/create/salesforce/run-generator.png)

リクエストが成功すると、ベータ版の仕様に従って B2B 名前空間とスキーマが作成されます。

## Amazon Web Services上でのExperience Platform用の [!DNL Salesforce] ソースの設定 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWSで実行されるExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platformインフラストラクチャについて詳しくは、[Experience Platformマルチクラウドの概要 ](../../../landing/multi-cloud.md) を参照してください。

Amazon Web Services（AWS）でExperience Platform用に [!DNL Salesforce] アカウントを設定する方法については、次の手順に従います。

### 前提条件

[!DNL Salesforce] アカウントをAWS リージョンのExperience Platformに接続するには、次が必要です。

- API アクセス権を持つ [!DNL Salesforce] アカウント。
- その後、JWT_BEARER OAuth フローを有効にするために使用できる [!DNL Salesforce Connected App]。
- データにアクセスするた [!DNL Salesforce] に必要な権限。

また、[!DNL Salesforce] アカウントをAmazon Web Services（AWS）のExperience Platformに接続するには、次の IP アドレスを許可リストに追加する必要があります。

- `34.193.63.59`
- `44.217.93.240`
- `44.194.79.229`

### [!DNL Salesforce Connected App] の作成

まず、以下を使用して PEM ファイルの証明書とキーペアを作成します。

```shell
openssl req -newkey rsa:4096 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem  
```

1. [!DNL Salesforce] ダッシュボードで、「設定」（![ 設定アイコン）を選択します。](/help/images/icons/settings.png)）を選択してから、「**[!DNL Setup]**」を選択します。
2. [!DNL App Manager] に移動し、「**[!DNL New Connection App]**」を選択します。
3. アプリの名前を指定し、残りのフィールドに自動入力できるようにします。
4. [!DNL Enable OAuth Settings] のボックスを有効にします。
5. コールバック URL を設定します。 これは JWT では使用されないので、`https://localhost` を使用できます。
6. [!DNL Use Digital Signatures] のボックスを有効にします。
7. 前に作成した cert.pem ファイルをアップロードします。

#### 必要な権限を追加

次の権限を追加します。

1. API を使用したユーザーデータの管理（api）
2. カスタム権限へのアクセス （custom_permissions）
3. ID URL サービスへのアクセス（ID、プロファイル、メール、アドレス、電話）
4. 一意の ID （openid）へのアクセス
5. いつでもリクエストを実行（refresh_token、offline_access）

権限が追加されたら、必ずこのチェックボックスを有効に **[!DNL Issue JSON Web Token (JWT)-based access tokens for named user]** ます。

次に、「**[!DNL Save]**」、「**[!DNL Continue]**」の順に選択し、「**[!DNL Manage Customer Details]**」を選択します。 消費者の詳細パネルを使用して、以下を取得します。

- **コンシューマーキー**：後でExperience Platformに対して [!DNL Salesforce] アカウントを認証する際に、このコンシューマーキーをクライアント ID として使用します。
- **コンシューマーシークレット**：後でExperience Platformに対して [!DNL Salesforce] アカウントを認証する際に、このコンシューマーシークレットをクライアント ID として使用します。

### 接続されたアプリへの [!DNL Salesforce] ユーザーの認証

接続されたアプリを使用するための認証を取得するには、次の手順に従います。

1. **[!DNL Manage Connected Apps]** に移動します。
2. **[!DNL Edit]** を選択します。
3. **[!DNL Permitted Users]** を **[!DNL Admin approved users are pre-authorized]** として設定し、「**[!DNL Save]**」を選択します。
4. **[!DNL Settings]/[!DNL Manage Users]/[!DNL Profiles]** に移動します。
5. ユーザーに関連付けられたプロファイルを編集します。
6. **[!DNL Connected App Access]** に移動し、前の手順で作成したアプリを選択します。

### JWT ベアラートークンの生成

JWT ベアラートークンを生成するには、次の手順に従います。

#### キーペアを pkcs12 に変換する

JWT ベアラートークンを生成するには、まず次のコマンドを使用して、証明書とキーのペアを pkcs12 形式に変換する必要があります。 この手順では、プロンプトが表示されたら **書き出しパスワードを設定** する必要もあります。

```shell
openssl pkcs12 -export -in cert.pem -inkey key.pem -name jwtcert >jwtcert.p12
```

#### pkcs12 に基づく java キーストアの作成

次に、次のコマンドを使用して、生成したばかりの pkcs12 に基づいて java キーストアを作成します。 この手順では、プロンプトが表示されたら **宛先キーストアパスワードを設定** も入力する必要があります。 さらに、ソースキーストアのパスワードとして、以前の書き出しパスワードを指定する必要があります。

```shell
keytool -importkeystore -srckeystore jwtcert.p12 -destkeystore keystore.jks -srcstoretype pkcs12 -alias jwtcert
```

#### keystroke.jks に jwtcert エイリアスが含まれていることを確認します

次に、次のコマンドを使用して、`keystroke.jks` に `jwtcert` エイリアスが含まれていることを確認します。 この手順では、前の手順で生成した宛先キーストアのパスワードを指定するように求められます。

```shell
keytool -keystore keystore.jks -list
```

#### 署名済みトークンの生成

最後に、以下の Java クラス JWTExample を使用して、署名済みトークンを生成します。

```java
package org.example;
 
import org.apache.commons.codec.binary.Base64;
 
import java.io.*;
import java.security.*;
import java.text.MessageFormat;
 
public class Main {
 
    public static void main(String[] args) {
 
        String header = "{\"alg\":\"RS256\"}";
        String claimTemplate = "'{'\"iss\": \"{0}\", \"sub\": \"{1}\", \"aud\": \"{2}\", \"exp\": \"{3}\"'}'";
 
        try {
            StringBuffer token = new StringBuffer();
 
            //Encode the JWT Header and add it to our string to sign
            token.append(Base64.encodeBase64URLSafeString(header.getBytes("UTF-8")));
 
            //Separate with a period
            token.append(".");
 
            //Create the JWT Claims Object
            String[] claimArray = new String[5];
            claimArray[0] = "{CLIENT_ID}";
            claimArray[1] = "{AUTHORIZED_SALESFORCE_USERNAME}";
            claimArray[2] = "{SALESFORCE_LOGIN_URL}";
            claimArray[3] = Long.toString((System.currentTimeMillis() / 1000) + 2629746*4);
            MessageFormat claims;
            claims = new MessageFormat(claimTemplate);
            String payload = claims.format(claimArray);
 
            //Add the encoded claims object
            token.append(Base64.encodeBase64URLSafeString(payload.getBytes("UTF-8")));
 
            //Load the private key from a keystore
            KeyStore keystore = KeyStore.getInstance("JKS");
            keystore.load(new FileInputStream("path/to/keystore"), "keystorepassword".toCharArray());
            PrivateKey privateKey = (PrivateKey) keystore.getKey("jwtcert", "privatekeypassword".toCharArray());
 
            //Sign the JWT Header + "." + JWT Claims Object
            Signature signature = Signature.getInstance("SHA256withRSA");
            signature.initSign(privateKey);
            signature.update(token.toString().getBytes("UTF-8"));
            String signedPayload = Base64.encodeBase64URLSafeString(signature.sign());
 
            //Separate with a period
            token.append(".");
 
            //Add the encoded signature
            token.append(signedPayload);
 
            System.out.println(token.toString());
 
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

| プロパティ | 設定  |
| --- | --- |
| `claimArray[0]` | クライアント ID で `claimArray[0]` を更新します。 |
| `claimArray[1]` | `claimArray[1]` を、アプリに対して許可されている [!DNL Salesforce] のユーザー名に更新します。 |
| `claimArray[2]` | [!DNL Salesforce] のログイン URL で `claimArray[2]` を更新します。 |
| `claimArray[3]` | `claimArray[3]` を、エポック時間からミリ秒単位で書式設定された有効期限で更新します。 例えば、`3660624000000` は 12-31-2085 です。 |
| `/path/to/keystore` | `/path/to/keystore` を keystore.jks の正しいパスに置き換えます |
| `keystorepassword` | `keystorepassword` を宛先キーストアのパスワードに置き換えます。 |
| `privatekeypassword` | `privatekeypassword` をソースキーストアのパスワードに置き換えます。 |

## 次の手順

[!DNL Salesforce] アカウントの前提条件の設定が完了したら、[!DNL Salesforce] アカウントをExperience Platformに接続して CRM データを取り込む手順に進むことができます。 詳しくは、以下のドキュメントを参照してください。

### API を使用して [!DNL Salesforce] と Platform を接続する

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Salesforce] と Platform を接続する方法について説明します。

- [Flow Service API を使用したSalesforce ベース接続の作成](../../tutorials/api/create/crm/salesforce.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)

### UI を使用した [!DNL Salesforce] の Platform への接続

- [UI でのSalesforce ソースコネクタの作成](../../tutorials/ui/create/crm/salesforce.md)
- [UI での CRM 接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)