---
title: API を使用した顧客管理キーの設定
description: Azure テナントで CMK アプリを設定し、暗号化キー ID をAdobe Experience Platformに送信する方法を説明します。
exl-id: c9a1888e-421f-4bb4-b4c7-968fb1d61746
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 49%

---

# API を使用した顧客管理キーの設定

このドキュメントでは、API を使用してAdobe Experience Platformで顧客管理キー (CMK) 機能を有効にするプロセスについて説明します。 UI を使用してこのプロセスを完了する手順については、 [UI CMK 設定ドキュメント](./ui-set-up.md).

## 前提条件

を表示して、 [!UICONTROL 暗号化] Adobe Experience Platformのセクションで、役割を作成し、 [!UICONTROL 顧客管理キーの管理] その役割に対する権限。 次の項目を持つユーザー [!UICONTROL 顧客管理キーの管理] 権限により、組織で CMK を有効にすることができます。

Experience Platformでの役割および権限の割り当てについて詳しくは、 [権限に関するドキュメントの設定](https://experienceleague.adobe.com/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/configure-permissions.html?lang=ja).

CMK を有効にするには、 [[!DNL Azure] Key Vault を設定する必要があります](./azure-key-vault-config.md) を次の設定で使用します。

* [消去保護を有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview#purge-protection)
* [ソフトデリートを有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)
* [次を使用してアクセスを設定 [!DNL Azure] ロールベースのアクセス制御](https://learn.microsoft.com/en-us/azure/role-based-access-control/)
* [ [!DNL Azure]  Key Vault の設定](./azure-key-vault-config.md)

## CMK アプリのセットアップ {#register-app}

キー Vault を設定したら、次の手順は、にリンクする CMK アプリケーションに登録することです [!DNL Azure] テナント。

### はじめに

CMK アプリを登録するには、Platform API を呼び出す必要があります。 これらの呼び出しを行うために必要な認証ヘッダーの収集方法について詳しくは、[Platform API 認証ガイド](../../api-authentication.md)を参照してください。

認証ガイドでは、必要である `x-api-key` リクエストヘッダーに独自の一意の値を生成する方法を説明していますが、このガイドのすべての API 操作では、代わりに、静的な値 `acp_provisioning` が使用されます。ただし、`{ACCESS_TOKEN}` と `{ORG_ID}` には独自の値を指定する必要があります。

このガイドに示すすべての API 呼び出しでは、 `platform.adobe.io` はルートパスとして使用され、デフォルトは VA7 地域です。 組織が異なる地域を使用している場合、 `platform` は、組織に割り当てられたダッシュと地域コードの後に付く必要があります。 `nld2` (NLD2 または `aus5` AUS5 の場合 ( 例： `platform-aus5.adobe.io`) をクリックします。 組織の地域が不明な場合は、システム管理者に問い合わせてください。

### 認証 URL の取得 {#fetch-authentication-url}

登録プロセスを開始するには、アプリ登録エンドポイントに対して GET リクエストを実行し、組織に必要な認証 URL を取得します。

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/infrastructure/manager/byok/app-registration \ 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: acp_provisioning' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

成功した応答は、認証 URL を含む `applicationRedirectUrl` プロパティを返します。

```json
{
    "id": "byok",
    "name": "acpebae9422Caepcmkmultitenantapp",
    "applicationUri": "https://adobe.com/acpebae9422Caepcmkmultitenantapp",
    "applicationId": "e463a445-c6ac-4ca2-b36a-b5146fcf6a52",
    "applicationRedirectUrl": "https://login.microsoftonline.com/common/oauth2/authorize?response_type=code&client_id=e463a445-c6ac-4ca2-b36a-b5146fcf6a52&redirect_uri=https://adobe.com/acpebae9422Caepcmkmultitenantapp&scope=user.read"
}
```

`applicationRedirectUrl` アドレスをコピーしてブラウザーに貼り付け、認証ダイアログを開きます。**[!DNL Accept]** を選択して、CMK アプリサービスプリンシパルを [!DNL Azure] テナントに追加します。

![Microsoft権限リクエストダイアログ [!UICONTROL 確定] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/app-permission.png)

### CMK アプリを役割に割り当てます。 {#assign-to-role}

認証プロセスが完了したら、[!DNL Azure] Key Vault に戻り、左側のナビゲーションで **[!DNL Access control]** を選択します。 ここから **[!DNL Add]** を選択し、続けて **[!DNL Add role assignment]** を選択します。

![Microsoft Azure ダッシュボードと [!DNL Add] および [!DNL Add role assignment] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/add-role-assignment.png)

次の画面では、この割り当ての役割を選択するように求められます。**[!DNL Key Vault Crypto Service Encryption User]** を選択してから **[!DNL Next]** を選択し、続行します。

![Microsoft Azure ダッシュボードと [!DNL Key Vault Crypto Service Encryption User] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/select-role.png)

次の画面で、「**[!DNL Select members]**」 を選択して、右側のパネルでダイアログを開きます。 検索バーを使用して CMK アプリケーションのサービスプリンシパルを見つけ、リストから選択します。 終了したら「**[!DNL Save]**」を選択します。

>[!NOTE]
>
>リストにアプリケーションが見つからない場合は、サービスプリンシパルがテナントに受け入れられていません。 正しい権限を持っていることを確認するには、 [!DNL Azure] 管理者または担当者。

## Experience Platform の暗号化キー設定を有効にする {#send-to-adobe}

CMK アプリを [!DNL Azure] にインストールすると、暗号化キー識別子をアドビに送信できます。 左側のナビゲーションで「**[!DNL Keys]**」を選択し、次に送信するキーの名前を選択します。

![Microsoft Azure ダッシュボードと [!DNL Keys] オブジェクトと、ハイライト表示されたキー名。](../../images/governance-privacy-security/customer-managed-keys/select-key.png)

キーの最新バージョンを選択すると、その詳細ページが表示されます。ここから、オプションでキーに対して許可する操作を設定できます。

>[!IMPORTANT]
>
>キーに対して許可する必要がある最小の操作は、 **[!DNL Wrap Key]** および **[!DNL Unwrap Key]** 権限。 以下を含めることができます。 [!DNL Encrypt], [!DNL Decrypt], [!DNL Sign]、および [!DNL Verify] 必要に応じて、

「**[!UICONTROL キー識別子]**」フィールドには、キーの URI 識別子が表示されます。次の手順で使用するために、この URI 値をコピーします。

![Microsoft Azure ダッシュボードのキーの詳細と [!DNL Permitted operations] と、ハイライト表示されているキー URL セクションをコピーします。](../../images/governance-privacy-security/customer-managed-keys/copy-key-url.png)

Key Vault の URI を取得したら、POST リクエストを使用して CMK 設定エンドポイントに送信できます。

>[!NOTE]
>
>Key Vault とキーの名前のみがアドビに保存され、キーのバージョンは保存されません。

**リクエスト**

+++ CMK 設定エンドポイントにキー Vault URI を送信するためのサンプルリクエストです。

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
          "keyVaultKeyIdentifier": "https://adobecmkexample.vault.azure.net/keys/adobeCMK-key/7c1d50lo28234cc895534c00d7eb4eb4"
        }
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 設定の名前。この値は、 [後の手順](#check-status). この値は、大文字と小文字を区別します。 |
| `type` | 設定タイプ。 `BYOK_CONFIG` に設定する必要があります。 |
| `imsOrgId` | 組織 ID。この ID は、 `x-gw-ims-org-id` ヘッダー。 |
| `configData` | このプロパティには、設定に関する次の詳細が含まれます。<ul><li>`providerType`：`AZURE_KEYVAULT` に設定する必要があります。</li><li>`keyVaultKeyIdentifier`：[以前](#send-to-adobe)にコピーした Key Vault の URI。</li></ul> |

+++

**応答**

+++ 正常な応答は、設定ジョブの詳細を返します。

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
    "imsOrgId": "{ORG_ID}",
    "status": "NEW"
  },
  "status": "CREATED"
}
```

+++

ジョブは、数分以内に処理を完了する必要があります。

## 設定のステータスの確認 {#check-status}

設定リクエストのステータスを確認するには、GET リクエストを実行します。

**リクエスト**

確認する設定の `name` をパスに追加し（以下の例では `config1`）、`BYOK_CONFIG` に設定された `configType` クエリパラメーターを含める必要があります。

+++ 設定リクエストのステータスを確認するリクエストのサンプルです。

```shell
curl -X GET \
  https://platform.adobe.io/data/infrastructure/manager/customer/config/config1?configType=BYOK_CONFIG \ 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: acp_provisioning' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

+++

**応答**

+++ 正常な応答は、ジョブのステータスを返します。

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
  "imsOrgId": "{ORG_ID}",
  "subscriptionId": "cf978863-7325-47b1-8fd9-554b9fdb6c36",
  "id": "4df7886b-a122-4391-880b-47888d5c5b92",
  "rowType": "BYOK_KEY"
}
```

+++

この `status` 属性には、次の意味を持つ 4 つの値のいずれかを指定できます。

1. `RUNNING`:Platform がキーとキーの Vault にアクセスできることを検証します。
1. `UPDATE_EXISTING_RESOURCES`：システムは、組織内のすべてのサンドボックスのデータストアに Key Vault とキー名を追加しています。
1. `COMPLETED`：キー Vault とキー名がデータストアに正常に追加されました。
1. `FAILED`：問題が発生しました。主にキー、Key Vault、またはマルチテナントのアプリ設定に関連しています。

## 次の手順

上記の手順を完了すると、組織で CMK が正常に有効になります。 プライマリデータストアに取り込まれたデータは、暗号化され、 [!DNL Azure] キー Vault。 Adobe Experience Platformでのデータ暗号化について詳しくは、 [暗号化ドキュメント](../encryption.md).
