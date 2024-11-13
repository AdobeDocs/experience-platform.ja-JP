---
keywords: Experience Platform; セキュリティ；IP アクセス；検証；API ガイド；クエリサービス；IP 検証
title: IP 検証エンドポイント
description: IP 検証 API エンドポイントを使用して、クエリサービスのサンドボックスの IP アクセスを検証する方法について説明します。
role: Developer
exl-id: 4ce9ab1c-e333-4ed5-a430-43ffec36a46d
source-git-commit: bf696c8836407a2fea82e9078201cb1f5004bcf8
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 4%

---

# IP 検証エンドポイント

>[!AVAILABILITY]
>
>この機能は、Data Distiller アドオンを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。

IP 検証 API エンドポイントを使用して、指定した IP アドレスがクエリサービスで指定したサンドボックスへのアクセスを許可されているかどうかを確認します。 このチェックでは、アクセス制限が適用されるか、IP アドレスがサンドボックス内のデータへのアクセスを許可されるかを確認します。

## サンドボックスアクセス用の IP を検証 {#validate-ip-for-sandbox-access}

IP 検証エンドポイントを使用して、指定したサンドボックスのデータに特定の IP アドレスからアクセスできるかどうかを確認します。 サンドボックスに IP 制限が設定されていない場合、すべての IP アドレスがデフォルトで許可されます。 既存の IP または CIDR 制限がある場合、この API は指定された IP アドレスが設定された範囲と一致するかどうかを確認します。

>[!NOTE]
>
>このエンドポイントには、**ユーザートークン** または **サービストークン** でアクセスできます。 特定の役割要件は必要ありません。

**API 形式**

```http
POST /security/validate/ip-access
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/query/security/validate/ip-access \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
       "ipAddress": "197.2.0.2"
     }'
```

**応答**

応答が成功すると、HTTP ステータス 200 が、IP が許可されているかどうかを示すブール値と共に返されます。

>[!NOTE]
>
>指定した IP がサンドボックスへのアクセスを許可されている場合は、応答の `isAllowed` フィールドが `true` を返し、許可されていない場合は `false` を返します。 この API は、サンドボックス環境のアクセスを動的に検証し、セキュリティコンプライアンスを確保することをサポートしています。

```json
{
"isAllowed": true
}
```
