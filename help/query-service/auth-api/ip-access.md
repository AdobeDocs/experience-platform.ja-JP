---
keywords: Experience Platform; セキュリティ；IP アクセス；QS-Auth; API ガイド；クエリサービス；IP 範囲
title: IP アクセス エンドポイント
description: IP アクセス API エンドポイントを使用して、クエリサービスでサンドボックスアクセスの IP 範囲を管理する方法を説明します。
role: Developer
exl-id: fc15ab50-c125-4f00-a311-81fd41697c7d
source-git-commit: d0f4a295928b000b6091172800e453d79dc44e3a
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 5%

---

# IP アクセスエンドポイント

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

指定したクエリサービスサンドボックス内でデータアクセスを保護するには、IP アクセスエンドポイントを使用して、許可された IP 範囲を管理します。 この API を使用して、組織 ID に関連付けられた IP 範囲を取得、設定または削除できます。

IP アクセス API を使用して、次のアクションを実行できます。

- **すべての IP 範囲を取得**
- **新しい IP 範囲を設定**
- **既存の IP 範囲を削除**

このドキュメントでは、`/security/ip-access` エンドポイントから送受信できるリクエストと応答について説明します。

>[!NOTE]
>
>この API を呼び出すには、ユーザートークンが必要です。 各ヘッダーに必要な値の取得については、[ はじめる前に ](./getting-started.md) を参照してください。

## すべての IP 範囲の取得 {#fetch-all-ip-ranges}

サンドボックスに設定されているすべての IP 範囲のリストを取得します。 IP 範囲が設定されていない場合、デフォルトですべての IP が許可され、応答は `allowedIpRanges` で空のリストを返します。

**API 形式**

```http
GET /security/ip-access
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/queryauth/security/ip-access \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答に成功すると、HTTP ステータス 200 が、サンドボックスの許可された IP 範囲のリストと共に返されます。

```json
{
  "imsOrg": "{ORG_ID}",
  "sandboxName": "prod",
  "channel": "data_distiller",
  "allowedIpRanges": [
    {"ipRange": "136.23.110.0/23", "description": "VPN-1 gateway IPs"},
    {"ipRange": "101.10.1.1"}
  ]
}
```

次の表に、応答スキーマのプロパティの説明と例を示します。

| プロパティ | 説明 | 例 |
|------------------|---------------------------------------------|-----------------------------------------------------------------------------------------------|
| `imsOrg` | サンドボックスの組織 ID。 | `{ORG_ID}` |
| `sandboxName` | IP 制限が適用されるサンドボックスの名前。 | `prod` |
| `channel` | IP 制限のアクセス モード。 現在、許可されている値は `data_distiller` のみです。 この値は、PSQL 接続または JDBC 接続に IP 制限が適用されることを示します。 | `data_distiller` |
| `allowedIpRanges` | 許可される IP のリスト（CIDR 形式または固定 IP 形式）。 各エントリには、オプションの説明を含めることができます。 | `[{"ipRange": "136.23.110.0/23", "description": "VPN-1 gateway IPs"}]` |

>[!NOTE]
>
>`allowedIpRanges` フィールドには、次の 2 種類の IP 仕様を含めることができます。<br><ul><li>**CIDR**:IP 範囲を定義するための標準的な CIDR 表記（例：`"136.23.110.0/23"`）。</li><li>**固定 IP**：個々のアクセス権限に対する単一の IP （例：`"101.10.1.1"`）。</li></ul>

## 新しい IP 範囲を設定

サンドボックスの新しいリストを設定して、既存の IP 範囲を上書きします。 この操作には、変更されていない IP 範囲を含む、IP 範囲の完全なリストが必要です。

**API 形式**

```http
PUT /security/ip-access
```

**リクエスト**

```shell
curl -X PUT https://platform.adobe.io/data/foundation/queryauth/security/ip-access \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "ipRanges": [
       {"ipRange": "136.23.110.0/23", "description": "VPN-1 gateway IPs"},
       {"ipRange": "17.102.17.0/23", "description": "VPN-2 gateway IPs"},
       {"ipRange": "101.10.1.1"},
       {"ipRange": "163.77.30.9", "description": "Test server IP"}
     ]
   }'
```

**応答**

応答が成功すると、HTTP ステータス 200 が、新しく設定された IP 範囲の詳細と共に返されます。

```json
{
  "imsOrg": "{ORG_ID}",
  "sandboxName": "prod",
  "channel": "data_distiller",
  "allowedIpRanges": [
    {"ipRange": "136.23.110.0/23", "description": "VPN-1 gateway IPs"},
    {"ipRange": "17.102.17.0/23", "description": "VPN-2 gateway IPs"},
    {"ipRange": "101.10.1.1"},
    {"ipRange": "163.77.30.9", "description": "Test server IP"}
  ]
}
```

## IP 範囲の削除 {#delete-ip-ranges}

サンドボックスの設定済み IP 範囲をすべて削除します。 このアクションは、IP 範囲を削除し、削除した IP リストを返します。

>[!NOTE]
>
>削除操作は組織（`imsOrg`） ID に適用され、サンドボックス用に設定されたすべての IP 範囲に影響します。

**API 形式**

```http
DELETE /security/ip-access
```

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/foundation/queryauth/security/ip-access \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、HTTP ステータス 200 が、削除された IP 範囲の詳細と共に返されます。

```json
{
  "imsOrg": "{ORG_ID}",
  "sandboxName": "prod",
  "channel": "data_distiller",
  "deletedIpRanges": [
    {"ipRange": "136.23.110.0/23", "description": "VPN-1 gateway IPs"},
    {"ipRange": "17.102.17.0/23", "description": "VPN-2 gateway IPs"},
    {"ipRange": "101.10.1.1"},
    {"ipRange": "163.77.30.9", "description": "Test server IP"}
  ]
}
```
