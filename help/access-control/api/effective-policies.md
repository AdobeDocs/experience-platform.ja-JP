---
keywords: Experience Platform；ホーム；人気の高いトピック；効果的なポリシー；アクセス制御api
solution: Experience Platform
title: 有効なポリシーAPIエンドポイント
topic-legacy: developer guide
description: Adobe Experience Platform のアクセス制御では、Adobe Admin Console を使用して、様々な Platform 機能のロールと権限を管理できます。このドキュメントは、Adobe Experience Platform向けアクセス制御APIを使用して効果的なポリシーを表示する方法のガイドとして機能します。
exl-id: 555d73db-115d-4f4c-8bd2-b91477799591
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 68%

---

# 有効なポリシーエンドポイント

現在のユーザーに有効なポリシーを表示するには、 API で `/acl/effective-policies` エンドポイントに POST リクエストを実行します。[!DNL Access Control]取得する権限とリソースの種類は、リクエストペイロードに配列の形式で指定する必要があります。これは、以下の API 呼び出し例に示されています。

**API 形式**

```http
POST /acl/effective-policies
```

**リクエスト**

次のリクエストは、「[!UICONTROL データセットを管理]」権限に関する情報を取得し、現在のユーザーの「[!UICONTROL スキーマ]」リソースタイプにアクセスします。

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
>ペイロードの配列で指定できる権限とリソースの種類のリストについては、[許可された権限とリソースの種類](#accepted-permissions-and-resource-types)に関する付録の節を参照してください。

**応答**

リクエストが成功した場合は、リクエストで指定した権限とリソースの種類に関する情報が返されます。このレスポンスには、リクエストで指定したリソースの種類に対して現在のユーザーが持っているアクティブな権限が含まれます。リクエストペイロードに含まれる権限が現在のユーザーに対してアクティブな場合、API は、権限とその権限がアクティブであることを示すアスタリスク（`*`）を返します。リクエストで指定した権限がそのユーザーに対してアクティブでない場合は、レスポンスのペイロードから除外されます。

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

このドキュメントでは、[!DNL Access Control] APIを呼び出して、アクティブなアクセス許可とリソースタイプの関連ポリシーに関する情報を返す方法を説明しました。 [!DNL Experience Platform]のアクセス制御について詳しくは、[アクセス制御の概要](../home.md)を参照してください。

## 付録

この節では、[!DNL Access Control] APIの使用に関する補足情報を提供します。

### 許可された権限とリソースの種類

次のリストは、`/acl/active-permissions` エンドポイントに対する POST リクエストのペイロードに含めることができる権限とリソースの種類を示しています。

**権限**

```plaintext
permissions/activate-destinations
permissions/evaluate-segments
permissions/execute-decisioning-activities
permissions/export-audience-for-segment
permissions/manage-datasets
permissions/manage-decisioning-activities
permissions/manage-decisioning-options
permissions/manage-destinations
permissions/manage-dsw
permissions/manage-dule-labels
permissions/manage-dule-policies
permissions/manage-identity-namespaces
permissions/manage-privacy-workflows
permissions/manage-profile-configs
permissions/manage-profiles
permissions/manage-queries
permissions/manage-schemas
permissions/manage-segments
permissions/manage-sources
permissions/reset-sandboxes
permissions/view-datasets
permissions/view-destinations
permissions/view-dule-labels
permissions/view-dule-policies
permissions/view-identity-namespaces
permissions/view-monitoring-dashboard
permissions/view-privacy-workflows
permissions/view-profile-configs
permissions/view-profiles
permissions/view-sandboxes
permissions/view-schemas
permissions/view-segments
permissions/view-sources
```

**リソースの種類**

```plaintext
resource-types/activation-associations
resource-types/activations
resource-types/activities
resource-types/analytics-source
resource-types/audience-manager-source
resource-types/bizible-source
resource-types/connection
resource-types/customer-attributes-source
resource-types/data-science-workspace
resource-types/dataset-preview
resource-types/datasets
resource-types/dule-label
resource-types/dule-policy
resource-types/enterprise-source
resource-types/identity-descriptor
resource-types/identity-namespaces
resource-types/launch-source
resource-types/marketing-action
resource-types/marketo-source
resource-types/monitoring
resource-types/offers
resource-types/placements
resource-types/privacy-consent
resource-types/privacy-content-delivery
resource-types/privacy-job
resource-types/profile-configs
resource-types/profile-datasets
resource-types/profiles
resource-types/query
resource-types/relationship-descriptor
resource-types/sandboxes
resource-types/schemas
resource-types/segment-jobs
resource-types/segments
resource-types/streaming-source
```
