---
title: Adobe Experience Platformの顧客管理キー
description: Adobe Experience Platformに保存されたデータ用に独自の暗号化キーを設定する方法を説明します。
source-git-commit: 02898f5143a7f4f48c64b22fb3c59a072f1e957d
workflow-type: tm+mt
source-wordcount: '1493'
ht-degree: 1%

---

# Adobe Experience Platformでの顧客管理キー

Adobe Experience Platformに保存されたデータは、システムレベルのキーを使用して保存時に暗号化されます。 Platform 上に構築されたアプリケーションを使用している場合は、代わりに独自の暗号化キーを使用するよう選択し、データのセキュリティをより詳細に制御できます。

このドキュメントでは、Platform で顧客管理キー (CMK) 機能を有効にするプロセスについて説明します。

## プロセスの概要

CMK は、ヘルスケアシールドおよびAdobeのプライバシーとセキュリティのシールドサービスに含まれます。 お客様の組織がこれらの製品の 1 つに対するライセンスを購入した後、1 回限りのプロセスで機能を設定できます。

>[!WARNING]
>
>CMK を設定した後は、システム管理キーに戻すことはできません。 鍵を安全に管理し、内で Key Vault、Key、CMK アプリにアクセスできるようにする責任を負います [!DNL Azure] データへのアクセスが失われるのを防ぐため。

プロセスは次のとおりです。

1. [の設定 [!DNL Microsoft Azure] Key Vault](#create-key-vault) 組織のポリシーに基づいて、 [暗号化キーを生成](#generate-a-key) それは最終的にAdobeと共有される。
1. API 呼び出しを使用して [CMK アプリのセットアップ](#register-app) を [!DNL Azure] テナント。
1. API 呼び出しを使用して [暗号化キー ID をAdobeに送信](#send-to-adobe) 機能のイネーブルメントプロセスを開始します。
1. [設定のステータスの確認](#check-status) :CMK が有効になっているかどうかを確認します。

設定プロセスが完了すると、すべてのサンドボックスをまたいで Platform に転送されるすべてのデータは、 [!DNL Azure] キーの設定。 CMK を使用するには、 [!DNL Microsoft Azure] その一部となる機能 [公開プレビュープログラム](https://azure.microsoft.com/en-ca/support/legal/preview-supplemental-terms/).

## の設定 [!DNL Azure] Key Vault {#create-key-vault}

CMK は、 [!DNL Microsoft Azure] キー Vault。 作業を開始するには、 [!DNL Azure] 新しいエンタープライズアカウントを作成するには、または既存のエンタープライズアカウントを使用して、以下の手順に従って Key Vault を作成します。

>[!IMPORTANT]
>
>の Premium 層と Standard サービス層のみ [!DNL Azure] Key Vault がサポートされています。 [!DNL Azure Managed HSM], [!DNL Azure Dedicated HSM] および [!DNL Azure Payments HSM] はサポートされていません。 詳しくは、 [[!DNL Azure] ドキュメント](https://learn.microsoft.com/en-us/azure/security/fundamentals/key-management#azure-key-management-services) を参照してください。

>[!NOTE]
>
>以下のドキュメントでは、キー Vault を作成する基本的な手順についてのみ説明します。 このガイダンス以外では、組織のポリシーに従って Key Vault を設定する必要があります。

にログインします。 [!DNL Azure] ポータルで **[!DNL Key vaults]** をクリックします。

![キーボールトを検索して選択する](../images/governance-privacy-security/customer-managed-keys/access-key-vaults.png)

この **[!DNL Key vaults]** ページが表示されます。 ここからを選択します。 **[!DNL Create]**.

![キー Vault を作成](../images/governance-privacy-security/customer-managed-keys/create-key-vault.png)

提供されたフォームを使用して、名前や割り当てられたリソースグループなど、キー Vault の基本的な詳細を入力します。

>[!WARNING]
>
>ほとんどのオプションはデフォルト値のままでかまいませんが、 **必ず soft-delete および purge protection オプションを有効にしてください**. これらの機能をオンにしないと、キーボールトが削除された場合に、データへのアクセスが失われる可能性があります。
>
>![パージ保護を有効にする](../images/governance-privacy-security/customer-managed-keys/basic-config.png)

ここから、主要な Vault 作成ワークフローを続け、組織のポリシーに従って様々なオプションを設定します。

いったん **[!DNL Review + create]** 手順では、検証中にキー vault の詳細を確認できます。 検証が合格したら、「 **[!DNL Create]** をクリックしてプロセスを完了します。

![キー Vault の基本設定](../images/governance-privacy-security/customer-managed-keys/finish-creation.png)

### ネットワークオプションの構成

公開アクセスを特定の仮想ネットワークに制限するようにキー Vault が設定されている場合、または公開アクセスを完全に無効にする場合は、Microsoftにファイアウォール例外を許可する必要があります。

選択 **[!DNL Networking]** をクリックします。 の下 **[!DNL Firewalls and virtual networks]**、チェックボックスを選択します。 **[!DNL Allow trusted Microsoft services to bypass this firewall]**&#x200B;を選択し、「 **[!DNL Apply]**.

![キー Vault の基本設定](../images/governance-privacy-security/customer-managed-keys/networking.png)

### キーを生成 {#generate-a-key}

キー Vault を作成したら、新しいキーを生成できます。 次に移動： **[!DNL Keys]** 「 」タブで「 」を選択します。 **[!DNL Generate/Import]**.

![キーを生成](../images/governance-privacy-security/customer-managed-keys/view-keys.png)

提供されたフォームを使用してキーの名前を指定し、「 」を選択します。 **RSA** キータイプの。 少なくとも、 **[!DNL RSA key size]** は、少なくとも **3072** 必要なビット [!DNL Cosmos DB]. [!DNL Azure Data Lake Storage] は、RSA 3027 とも互換性があります。

>[!NOTE]
>
>キーに指定した名前を記憶しておきます。これは、後の手順で [キーをAdobeに送信](#send-to-adobe).

残りのコントロールを使用して、必要に応じて生成またはインポートするキーを設定します。 終了したら、「 」を選択します。 **[!DNL Create]**.

![キーを設定](../images/governance-privacy-security/customer-managed-keys/configure-key.png)

設定されたキーが、Vault のキーのリストに表示されます。

![追加されたキー](../images/governance-privacy-security/customer-managed-keys/key-added.png)

## CMK アプリのセットアップ {#register-app}

キー Vault を設定したら、次の手順は、にリンクする CMK アプリケーションを登録することです [!DNL Azure] テナント。

>[!NOTE]
>
>CMK アプリを登録するには、Platform API を呼び出す必要があります。 これらの呼び出しをおこなうために必要な認証ヘッダーの収集方法について詳しくは、 [Platform API 認証ガイド](../../landing/api-authentication.md).
>
>認証ガイドでは、必要に応じて独自の一意の値を生成する方法を説明しています `x-api-key` リクエストヘッダー。このガイドのすべての API 操作では、静的な値が使用されます `acp_provisioning` 代わりに、 引き続き、 `{ACCESS_TOKEN}` および `{ORG_ID}`ただし、

### 認証 URL の取得

登録プロセスを開始するには、アプリ登録エンドポイントに対してGETリクエストを実行し、組織に必要な認証 URL を取得します。

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/infrastructure/manager/byok/app-registration \ 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: acp_provisioning' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

成功すると、 `applicationRedirectUrl` 認証 URL を含むプロパティ。

```json
{
    "id": "byok",
    "name": "acpebae9422Caepcmkmultitenantapp",
    "applicationUri": "https://adobe.com/acpebae9422Caepcmkmultitenantapp",
    "applicationId": "e463a445-c6ac-4ca2-b36a-b5146fcf6a52",
    "applicationRedirectUrl": "https://login.microsoftonline.com/common/oauth2/authorize?response_type=code&client_id=e463a445-c6ac-4ca2-b36a-b5146fcf6a52&redirect_uri=https://adobe.com/acpebae9422Caepcmkmultitenantapp&scope=user.read"
}
```

をコピーして貼り付けます。 `applicationRedirectUrl` アドレスをブラウザーに送信し、認証ダイアログを開きます。 選択 **[!DNL Accept]** CMK アプリサービスプリンシパルを [!DNL Azure] テナント。

![許可リクエストを承認](../images/governance-privacy-security/customer-managed-keys/app-permission.png)

### CMK アプリをロールに割り当てます。 {#assign-to-role}

認証プロセスが完了したら、 [!DNL Azure] Key Vault を選択し、を選択します。 **[!DNL Access control]** をクリックします。 ここからを選択します。 **[!DNL Add]** 続いて **[!DNL Add role assignment]**.

![役割の割り当てを追加](../images/governance-privacy-security/customer-managed-keys/add-role-assignment.png)

次の画面で、この割り当ての役割を選択するように求められます。 選択 **[!DNL Key Vault Crypto Service Encryption User]** 選択する前に **[!DNL Next]** をクリックして続行します。

![ロールを選択](../images/governance-privacy-security/customer-managed-keys/select-role.png)

次の画面で、「 」を選択します。 **[!DNL Select members]** をクリックして、右側のパネルでダイアログを開きます。 検索バーを使用して CMK アプリケーションのサービスプリンシパルを探し、リストから選択します。 終了したら、「 」を選択します。 **[!DNL Save]**.

>[!NOTE]
>
>リストにアプリケーションが見つからない場合は、サービスプリンシパルがテナントに受け入れられていません。 ご一緒に作業してください [!DNL Azure] 管理者または担当者に問い合わせて、正しい権限を持っていることを確認します。

## Experience Platformの暗号化キー設定を有効にする {#send-to-adobe}

CMK アプリを [!DNL Azure]を使用すると、暗号化キー識別子をAdobeに送信できます。 選択 **[!DNL Keys]** 左のナビゲーションで、送信するキーの名前を入力します。

![キーを選択](../images/governance-privacy-security/customer-managed-keys/select-key.png)

キーの最新バージョンを選択すると、その詳細ページが表示されます。 ここから、オプションでキーに対して許可する操作を設定できます。 少なくとも、キーには **[!DNL Wrap Key]** および **[!DNL Unwrap Key]** 権限。

この **[!UICONTROL キー識別子]** 「 」フィールドには、キーの URI 識別子が表示されます。 この URI 値をコピーして、次の手順で使用します。

![キー URL をコピー](../images/governance-privacy-security/customer-managed-keys/copy-key-url.png)

キー Vault URI を取得したら、POSTリクエストを使用して CMK 設定エンドポイントに送信できます。

>[!NOTE]
>
>キーの Vault とキー名のみがAdobeで保存され、キーのバージョンは保存されません。

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/infrastructure/manager/customer/config \ 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: acp_provisioning' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -d '{
        "name": "Config1",
        "type": "BYOK_CONFIG",
        "imsOrgId": "{ORG_ID}",
        "configData": {
          "providerType": "AZURE_KEYVAULT",
          "keyVaultIdentifier": "https://adobecmkexample.vault.azure.net/keys/adobeCMK-key/7c1d50lo28234cc895534c00d7eb4eb4"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 設定の名前。 この値は、 [後の手順](#check-status). 値では大文字と小文字が区別されます。 |
| `type` | 設定タイプ。 `BYOK_CONFIG` に設定する必要があります。 |
| `imsOrgId` | IMS 組織 ID。これは、 `x-gw-ims-org-id` ヘッダー。 |
| `configData` | 設定に関する次の詳細が含まれます。<ul><li>`providerType`：`AZURE_KEYVAULT` に設定する必要があります。</li><li>`keyVaultIdentifier`:コピーしたキーボールト URI [以前](#send-to-adobe).</li></ul> |

**応答**

正常な応答は、設定ジョブの詳細を返します。

```json
{
  "id": "4df7886b-a122-4391-880b-47888d5c5b92",
  "config": {
    "configData": {
      "keyVaultUri": "https://adobecmkexample.vault.azure.net",
      "keyVaultKeyIdentifier": "https://adobecmkexample.vault.azure.net/keys/adobeCMK-key/7c1d50lo28234cc895534c00d7eb4eb4",
      "keyVersion": "7c1d50lo28234cc895534c00d7eb4eb4",
      "keyName": "Config1",
      "providerType": "AZURE_KEYVAULT"
    },
    "name": "acpcf978863Aaepcmkmultitenantapp",
    "type": "BYOK_CONFIG",
    "imsOrgId": "{IMS_ORG}",
    "status": "NEW"
  },
  "status": "CREATED"
}
```

ジョブは、数分以内に処理を完了する必要があります。

## 設定のステータスの確認 {#check-status}

設定リクエストのステータスを確認するには、GETリクエストを実行します。

**リクエスト**

を追加する必要があります。 `name` パス (`config1` （以下の例では）、およびを `configType` クエリパラメーターをに設定 `BYOK_CONFIG`.

```shell
curl -X GET \
  https://platform.adobe.io/data/infrastructure/manager/customer/config/config1?configType=BYOK_CONFIG \ 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: acp_provisioning' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

正常な応答は、ジョブのステータスを返します。

```json
{
  "name": "acpcf978863Aaepcmkmultitenantapp",
  "type": "BYOK_CONFIG",
  "status": "COMPLETED",
  "configData": {
    "keyVaultUri": "https://adobecmkexample.vault.azure.net",
    "keyVaultKeyIdentifier": "https://adobecmkexample.vault.azure.net/keys/adobeCMK-key/7c1d50lo28234cc895534c00d7eb4eb4",
    "keyVersion": "7c1d50lo28234cc895534c00d7eb4eb4",
    "keyName": "Config1",
    "providerType": "AZURE_KEYVAULT"
  },
  "imsOrgId": "{IMS_ORG}",
  "subscriptionId": "cf978863-7325-47b1-8fd9-554b9fdb6c36",
  "id": "4df7886b-a122-4391-880b-47888d5c5b92",
  "rowType": "BYOK_KEY"
}
```

この `status` 属性には、次の 4 つの意味の値のいずれかを指定できます。

1. `RUNNING`:Platform がキーとキーの Vault にアクセスできることを検証します。
1. `UPDATE_EXISTING_RESOURCES`:システムは、組織内のすべてのサンドボックスにわたって、キー Vault とキー名をデータストアに追加しています。
1. `COMPLETED`:キー Vault とキー名がデータストアに追加されました。
1. `FAILED`:主にキー、キー Vault、またはマルチテナントのアプリセットアップに関連する問題が発生しました。

## 次の手順

上記の手順を完了すると、組織で CMK が正常に有効になります。 Platform に取り込まれたデータは、暗号化され、 [!DNL Azure] キー Vault。 データへの Platform アクセスを取り消す場合は、内のキー Vault から、アプリケーションに関連付けられているユーザの役割を削除できます。 [!DNL Azure].

アプリケーションへのアクセスを無効にした後、Platform でデータにアクセスできなくなるまで、数分から 24 時間かかる場合があります。 同じ時間遅延が、アプリケーションへのアクセスを再度有効にした場合に、データが再び使用可能になるのに適用されます。

>[!WARNING]
>
>Key Vault、Key、または CMK アプリが無効になり、Platform でデータにアクセスできなくなると、そのデータに関連するダウンストリーム操作はできなくなります。 設定を変更する前に、データへの Platform アクセスを取り消すことによるダウンストリームの影響を理解しておく必要があります。
