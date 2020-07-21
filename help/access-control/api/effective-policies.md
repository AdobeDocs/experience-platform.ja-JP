---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 表示効果の高いポリシー
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 1%

---


# 表示効果の高いポリシー

現在のユーザーに対して有効なポリシーを表示するには、 `/acl/effective-policies`[!DNL Access Control] APIのエンドポイントにPOSTリクエストを行います。 取得する権限とリソースタイプは、配列の形式で要求ペイロードに指定する必要があります。 これは、以下のAPI呼び出しの例で示されます。

**API形式**

```http
POST /acl/effective-policies
```

**リクエスト**

次のリクエストは、「データセットを[!UICONTROL 管理]」権限に関する情報を取得し、現在のユーザーの「[!UICONTROL スキーマ]」リソースタイプにアクセスします。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/access-control/acl/effective-policies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
    "/permissions/manage-datasets",
    "/resource-types/schemas"
  ]'
```

>[!NOTE]
>
>ペイロード配列で提供できる権限とリソースタイプの完全なリストについては、 [許可された権限とリソースタイプに関する付録の節を参照してください](#accepted-permissions-and-resource-types)。

**応答**

成功した応答は、要求で提供された権限とリソースタイプに関する情報を返します。 この応答には、現在のユーザーが、要求で指定されたリソースタイプに対して持つアクティブな権限が含まれます。 要求ペイロードに含まれる権限が現在のユーザーに対してアクティブな場合、APIは、その権限がアクティブであることを示すアストリック(`*`)と共に権限を返します。 要求で指定された権限のうち、ユーザーに対してアクティブでないものは、応答ペイロードに含まれません。

```json
{
    "policies": {
        "/resource-types/schemas": [
            "read",
            "write",
            "delete"
        ],
        "/permissions/manage-datasets": [
            "*"
        ]
    }
}
```

## 次の手順

このドキュメントでは、アクティブな権限とリソースタイプの関連ポリシーに関する情報を返すために [!DNL Access Control] APIを呼び出す方法について説明しました。 のアクセス制御について詳し [!DNL Experience Platform]くは、 [アクセス制御の概要](../home.md)を参照してください。

## 付録

この節では、 [!DNL Access Control] APIの使用に関する補足情報を提供します。

### 許可された権限とリソースの種類

次に、エンドポイントへのPOST要求のペイロードに含めることができる権限とリソースタイプのリストを示し `/acl/active-permissions` ます。

**Permissions**

```plaintext
"permissions/activate-destinations"
"permissions/export-audience-for-segments"
"permissions/manage-datasets"
"permissions/manage-destinations"
"permissions/manage-identity-namespaces"
"permissions/manage-profiles"
"permissions/manage-sandboxes"
"permissions/manage-schemas"
"permissions/reset-sandboxes"
"permissions/view-datasets"
"permissions/view-destinations"
"permissions/view-identity-namespaces"
"permissions/view-monitoring-dashboard"
"permissions/view-profiles"
"permissions/view-sandboxes"
"permissions/view-schemas"
```

**リソースタイプ**

```plaintext
"resource-types/classes"
"resource-types/connections"
"resource-types/data-types"
"resource-types/dataset-data"
"resource-types/datasets"
"resource-types/destinations"
"resource-types/dule-labels"
"resource-types/identity-descriptors"
"resource-types/identity-namespaces"
"resource-types/mixins"
"resource-types/monitoring"
"resource-types/profile-configs
"resource-types/profile-datasets"
"resource-types/profiles"
"resource-types/relationship-descriptors"
"resource-types/reset-sandboxes"
"resource-types/sandboxes"
"resource-types/schemas"
"resource-types/segment-jobs"
"resource-types/segments"
```
