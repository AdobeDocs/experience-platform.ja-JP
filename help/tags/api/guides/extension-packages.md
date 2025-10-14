---
title: Reactor API でのプライベート拡張機能パッケージの共有
description: Reactor API でプライベート拡張機能パッケージを共有するように他の企業を認証する方法を説明します。
source-git-commit: ea9a2bb00d3ce59e28ea4cda0d30945e77aa95cb
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 3%

---


# プライベート拡張機能パッケージの共有

>[!NOTE]
>
>拡張機能パッケージ所有者組織の `develop_extensions` 権限を持つユーザーは、その拡張機能パッケージの拡張機能パッケージ使用権限を作成、リストおよび削除できます。 `manage_properties` 権限を持つ承認済み組織内のユーザーは、組織での拡張機能パッケージの使用権限をリストし、その拡張機能パッケージを使用する場合は `accepted` に、使用しない場合は `rejected` に、状態を更新することのみが許可されます。

拡張機能パッケージの所有者は、Reactor API を通じて、他社の非公開バージョンを利用する権限を付与できます。 1 つの拡張機能パッケージの使用ライセンスが承認された各ビジネスに付与され、この承認は、パッケージの現在および将来のプライベートバージョンすべてに適しています。

このガイドでは、拡張機能パッケージの使用権限を設定する方法の概要を説明します。 認証の構造の JSON の例など、Reactor API で認証を管理する方法に関するガイダンスについて詳しくは、[&#x200B; 拡張機能パッケージ使用承認エンドポイントガイド &#x200B;](../endpoints/extension-package-usage-authorizations.md) を参照してください。

## 認証の作成 {#create-authorization}

新しい認証を作成するには、`develop_extensions` 権限が必要です。 次の例は、認証する会社の `authorized_org_id` を使用して、拡張機能パッケージの拡張機能パッケージの使用状況認証を作成する方法を示しています。

**API 形式**

```http
POST /extension_packages/{EXTENSION_PACKAGE_ID}/extension_package_usage_authorizations
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_PACKAGE_ID` | 認証を作成する拡張機能パッケージの `ID`。 |

{style="table-layout:auto"}

**リクエスト**

次のリクエストは、指定された会社の新しい拡張機能パッケージ認証を作成します。

```shell
curl -X POST \
  https://reactor.adobe.io/extension_packages/{EXTENSION_PACKAGE_ID}/extension_package_usage_authorizations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -d '{
        "data": {
          "attributes": {
            "authorized_org_id": "{ORG_ID}"
          },
          "type": "extension_package_usage_authorizations"
        }
      } 
```

| プロパティ | 説明 |
| --- | --- |
| `attributes.authorized_org_id` | 認証する組織の `ID`。 |

{style="table-layout:auto"}

認証の初期状態は `pending_approval` の段階です。 会社は、拡張機能パッケージを使用する前に、認証を承認する必要があります。 会社のユーザーは、認証が承認保留中の間はプライベート拡張機能パッケージを参照できますが、インストールできず、拡張機能カタログに見つかりません。

## 承認を承認 {#approve-authorization}

承認を承認するには、`manage_properties` の権限が必要です。 認証済み会社として、拡張機能パッケージの使用認証に、認証の `ID` を含むPATCHリクエストを送信し、state を `approved` に設定する必要があります。

**API 形式**

```http
PATCH //extension_package_usage_authorizations/{EXTENSION_PACKAGE_USAGE_AUTHORIZATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_PACKAGE_USAGE_AUTHORIZATION_ID` | 承認する認証の `ID`。 |

{style="table-layout:auto"}

**リクエスト**

次のPATCHリクエストは、認証の `state` を `approved` に設定します。

```shell
curl -X PATCH \
  https://reactor.adobe.io/extension_package_usage_authorizations/{EXTENSION_PACKAGE_USAGE_AUTHORIZATION_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-Api-Key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
  -d '{
        "data": {
          "attributes": {
	          "state": "approved"
	        },
	        "id": ":extension_package_usage_authorization_id",
	        "type": "extension_package_usage_authorizations"
        }
      }
```

認証が承認されると、認証済み会社として、プロパティに拡張機能パッケージをインストールできるようになります。

>[!NOTE]
>
>承認が拒否されると、承認された会社は拡張機能パッケージを使用できなくなります。

## 認証の削除 {#delete-authorization}

拡張機能パッケージの所有者は、拡張機能パッケージの使用認証を削除することで、いつでも認証を取り消すことができます。 これにより、承認済み会社は、カタログ内の拡張機能パッケージの非公開バージョンを表示して、自社のプロパティにインストールできなくなります。 ただし、既にインストールされているプライベートバージョンは、削除後も引き続き期待どおりに動作します。

**API 形式**

```http
DELETE //extension_package_usage_authorizations/{EXTENSION_PACKAGE_USAGE_AUTHORIZATION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `EXTENSION_PACKAGE_USAGE_AUTHORIZATION_ID` | 削除する認証の `ID`。 |

{style="table-layout:auto"}

**リクエスト**

次のDELETEリクエストは、会社の認証権限を削除します。

```shell
curl -X DELETE \
  https://reactor.adobe.io/extension_package_usage_authorizations/{EXTENSION_PACKAGE_USAGE_AUTHORIZATION_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-Api-Key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H "Content-Type: application/vnd.api+json" \
```
