---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
topic: overview
translation-type: tm+mt
source-git-commit: 397f08efa276f7885e099a0a8d9dc6d23fe0e8cc
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 35%

---


# プライバシーリクエストの処理( [!DNL Real-time Customer Profile]

Adobe Experience Platform [!DNL Privacy Service] processes customer requests to access, opt out of sale, or delete their personal data as delineated by privacy regulations such as the General Data Protection Regulation (GDPR) and [!DNL California Consumer Privacy Act] (CCPA).

This document covers essential concepts related to processing privacy requests for [!DNL Real-time Customer Profile].

## はじめに

It is recommended that you have a working understanding of the following [!DNL Experience Platform] services before reading this guide:

* [[!DNLPrivacy Service]](home.md):Adobe Experience Cloudのアプリケーション間での個人データへのアクセス、販売中止、削除に関する顧客の要求を管理します。
* [[!DNL IDサービス]](../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [[!DNLリアルタイム顧客プロファイル]](../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] bridges customer identity data across systems and devices. [!DNL Identity Service] は、 **identity名前空間** を使用して、identity値に対するコンテキストを、その接触チャネルのシステムに関連付けることで提供します。 名前空間は、電子メールアドレス（「電子メール」）などの汎用概念を表すことや、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

For more information about identity namespaces in [!DNL Experience Platform], see the [identity namespace overview](../identity-service/namespaces.md).

## リクエストの送信 {#submit}

>[!NOTE]
>
>This section covers how to create privacy requests for the [!DNL Profile] data store. リクエストペイロードで送信されたユーザー ID データを適切にフォーマットする方法など、プライバシージョブの送信方法に関する完全な手順については、[Privacy Service API](../privacy-service/api/getting-started.md) または [Privacy Service UI](../privacy-service/ui/overview.md) のドキュメントを確認することを強くお勧めします。

The following section outlines how to make privacy requests for [!DNL Real-time Customer Profile] and the [!DNL Data Lake] using the [!DNL Privacy Service] API or UI.

### API の使用

API でジョブリクエストを作成する場合、提供されるすべての `userIDs` は、適用するデータストアに応じて、特定の `namespace` と `type` を使用する必要があります。IDs for the [!DNL Profile] store must use either &quot;standard&quot; or &quot;custom&quot; for their `type` value, and a valid [identity namespace](#namespaces) recognized by [!DNL Identity Service] for their `namespace` value.


さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。When making requests to the [!DNL Data Lake], the array must include the value &quot;ProfileService&quot;.

The following request creates a new privacy job for both [!DNL Real-time Customer Profile], using the standard &quot;Email&quot; identity namespace. It also includes the product value for [!DNL Profile] in the `include` array:

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{IMS_ORG}"
      }
    ],
    "users": [
      {
        "key": "user12345",
        "action": ["access","delete"],
        "userIDs": [
          {
            "namespace": "Email",
            "value": "ajones@acme.com",
            "type": "standard"
          },
          {
            "namespace": "email_label",
            "value": "ajones@acme.com",
            "type": "unregistered"
          }
        ]
      }
    ],
    "include": ["ProfileService", "aepDataLake"],
    "expandIds": false,
    "priority": "normal",
    "analyticsDeleteMethod": "anonymize",
    "regulation": "ccpa"
}'
```

### UI の使用

When creating job requests in the UI, be sure to select **[!UICONTROL AEP Data Lake]** and/or **[!UICONTROL Profile]** under **[!UICONTROL Products]** in order to process jobs for data stored in the [!DNL Data Lake] or [!DNL Real-time Customer Profile], respectively.

<img src="images/privacy/product-value.png" width="450"><br>

## リクエスト処理の削除

When [!DNL Experience Platform] receives a delete request from [!DNL Privacy Service], [!DNL Platform] sends confirmation to [!DNL Privacy Service] that the request has been received and affected data has been marked for deletion. The records are then removed from the [!DNL Data Lake] or [!DNL Profile] store within seven days. During that seven-day window, the data is soft-deleted and is therefore not accessible by any [!DNL Platform] service.

In future releases, [!DNL Platform] will send confirmation to [!DNL Privacy Service] after data has been physically deleted.

## 次の手順

By reading this document, you have been introduced to the important concepts involved with processing privacy requests in [!DNL Experience Platform]. ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

For information on processing privacy requests for [!DNL Platform] resources not used by [!DNL Profile], see the document on [privacy request processing in the Data Lake](../catalog/privacy.md).