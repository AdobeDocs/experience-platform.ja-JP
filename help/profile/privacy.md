---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
topic: overview
translation-type: tm+mt
source-git-commit: 066337419431db24bde0a8d0d30b85132d08f43c
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 16%

---


# プライバシーリクエストの処理( [!DNL Real-time Customer Profile]

Adobe Experience Platform [!DNL Privacy Service] processes customer requests to access, opt out of sale, or delete their personal data as delineated by privacy regulations such as the General Data Protection Regulation (GDPR) and [!DNL California Consumer Privacy Act] (CCPA).

This document covers essential concepts related to processing privacy requests for [!DNL Real-time Customer Profile].

## はじめに

It is recommended that you have a working understanding of the following [!DNL Experience Platform] services before reading this guide:

* [[!DNL Privacy Service]](home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Identity Service]](../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] bridges customer identity data across systems and devices. [!DNL Identity Service] は、 **identity名前空間** を使用して、identity値に対するコンテキストを、その接触チャネルのシステムに関連付けることで提供します。 名前空間は、電子メールアドレス（「電子メール」）などの汎用概念を表すことや、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

For more information about identity namespaces in [!DNL Experience Platform], see the [identity namespace overview](../identity-service/namespaces.md).

## リクエストの送信 {#submit}

次の節では、APIまたはUIを使用する際にプライバシーをリクエストする方法 [!DNL Real-time Customer Profile][!DNL Privacy Service] の概要を説明します。 Before reading these sections, it is strongly recommended that you review the [Privacy Service API](../privacy-service/api/getting-started.md) or [Privacy Service UI](../privacy-service/ui/overview.md) documentation for complete steps on how to submit a privacy job, including how to properly format submitted user identity data in request payloads.

>[!IMPORTANT]
>
>Privacy Serviceは、IDのステッチを実行しないマージポリシーを使用した [!DNL Profile] データのみを処理できます。 UIを使用してプライバシー要求が処理されているかどうかを確認する場合は、[!DNL None]IDの切り替えのタイプとして「  」を持つポリシーを使用していることを確認してください。 つまり、 [!UICONTROL IDの切り替えが「] Private graph」に設定されているマージポリシーは使用できません。
>
>![](./images/privacy/no-id-stitch.png)
>
>また、プライバシー要求が完了するまでに要する時間を保証できないことにも注意する必要があります。 リクエストの処理中に [!DNL Profile] データに変更が発生した場合、それらのレコードが処理されるかどうかも保証できません。

### API の使用

APIでジョブリクエストを作成する場合、内で提供されるIDは、特定のANDを使用する `userIDs` 必要があり `namespace` ま `type`す。 で [認識される有効な](#namespaces) ID名前空間 [!DNL Identity Service] は、 `namespace` 値に指定する必要があります。一方、 `type``standard``unregistered` または(標準名前空間とカスタムの場合、それぞれ)を指定する必要があります。

>[!NOTE]
>
>IDグラフや、プロファイルフラグメントがプラットフォームデータセット内でどのように分散されるかに応じて、各顧客に対して複数のIDを指定する必要がある場合があります。 詳しくは、次のセクションの [プロファイルフラグメント](#fragments) を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。When making requests to the [!DNL Data Lake], the array must include the value &quot;ProfileService&quot;.

次のリクエストは、ストア内の1人の顧客のデータに対して新しいプライバシージョブを作成し [!DNL Profile] ます。 アレイ内の顧客に対して2つのID値が提供され `userIDs` ます。1つは標準 `Email` ID名前空間を使用し、もう1つはカスタム `Customer_ID` 名前空間を使用しています。 It also includes the product value for [!DNL Profile] (`ProfileService`) in the `include` array:

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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
            "namespace": "Customer_ID",
            "value": "12345678",
            "type": "unregistered"
          }
        ]
      }
    ],
    "include": ["ProfileService"],
    "expandIds": false,
    "priority": "normal",
    "analyticsDeleteMethod": "anonymize",
    "regulation": "ccpa"
}'
```

### UI の使用

When creating job requests in the UI, be sure to select **[!UICONTROL AEP Data Lake]** and/or **[!UICONTROL Profile]** under **[!UICONTROL Products]** in order to process jobs for data stored in the [!DNL Data Lake] or [!DNL Real-time Customer Profile], respectively.

<img src="images/privacy/product-value.png" width="450"><br>

## プライバシーリクエストのプロファイルフラグメント {#fragments}

データストアでは、個々の顧客の個人データが複数のプロファイルフラグメントで構成される場合が多くあり、これらはIDグラフを介して個人に関連付けられます。 [!DNL Profile] ストアにプライバシーリクエストを送信する場合、リクエストはプロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意する必要があります。 [!DNL Profile]

例えば、顧客属性データを3つの異なるデータセットに格納する場合、異なる識別子を使用して個々の顧客に関連付けます。

| データセット名 | プライマリIDフィールド | 保存された属性 |
| --- | --- | --- |
| データセット1 | `customer_id` | `address` |
| データセット2 | `email_id` | `firstName`、`lastName` |
| データセット3 | `email_id` | `mlScore` |

データセットの1つは主識別子 `customer_id` として使用され、残りの2つは使用され `email_id`ます。 ユーザーIDの値としてのみを使用してプライバシーリクエスト（アクセスまたは削除） `email_id` を送信する場合、 `firstName`、 `lastName`および `mlScore` 属性のみが処理されますが、影響は受けま `address` せん。

プライバシー要求ですべての関連する顧客属性を確実に処理するには、該当するすべてのデータセットに対し、それらの属性を保存できるプライマリID値を指定する必要があります（1人の顧客につき最大9個のID）。 一般的にIDとしてマークされるフィールドの詳細については、「スキーマ構成の [基本](../xdm/schema/composition.md#identity) 」の「IDフィールド」の節を参照してください。

>[!NOTE]
>
>別の [サンドボックスを使用して](../sandboxes/home.md) データを保存する場合 [!DNL Profile]`x-sandbox-name` 、各サンドボックスに対して個別のプライバシーリクエストを行い、ヘッダーに適切なサンドボックス名を示す必要があります。

## リクエスト処理の削除

When [!DNL Experience Platform] receives a delete request from [!DNL Privacy Service], [!DNL Platform] sends confirmation to [!DNL Privacy Service] that the request has been received and affected data has been marked for deletion. プライバシージョブが完了すると、レコードは [!DNL Data Lake] または [!DNL Profile] ストアから削除されます。 削除ジョブが処理中の間は、データはソフト削除されるので、どの [!DNL Platform] サービスからもアクセスできません。 ジョブステータスの追跡の詳細については、 [[!DNL Privacy Service] ドキュメント](../privacy-service/home.md#monitor) を参照してください。

>[!IMPORTANT]
>
>削除リクエストが成功すると、顧客（または顧客のセット）の収集された属性データが削除されますが、このリクエストでは、IDグラフに設定された関連付けは削除されません。
>
>例えば、顧客のを使用し、それらのIDに格納されているすべての属性データを `email_id``customer_id` 削除する削除リクエストがあります。 しかし、その後同じ条件で取り込まれるデータは、引き続き適切 `customer_id` なものと関連付けられ `email_id`る。

In future releases, [!DNL Platform] will send confirmation to [!DNL Privacy Service] after data has been physically deleted.

## 次の手順

By reading this document, you have been introduced to the important concepts involved with processing privacy requests in [!DNL Experience Platform]. ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

For information on processing privacy requests for [!DNL Platform] resources not used by [!DNL Profile], see the document on [privacy request processing in the Data Lake](../catalog/privacy.md).