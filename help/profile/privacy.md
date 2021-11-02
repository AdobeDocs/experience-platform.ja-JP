---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
type: Documentation
description: Adobe Experience Platform Privacy Serviceは、多数のプライバシー規制に従って、個人データへのアクセス、販売のオプトアウト、削除の顧客リクエストを処理します。 このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する重要な概念について説明します。
exl-id: fba21a2e-aaf7-4aae-bb3c-5bd024472214
source-git-commit: d8665a349c6f453d83b64317982f3544bbcde0f7
workflow-type: tm+mt
source-wordcount: '1170'
ht-degree: 30%

---

# でのプライバシーリクエストの処理 [!DNL Real-time Customer Profile]

Adobe Experience Platform [!DNL Privacy Service] は、一般データ保護規則 (GDPR) や [!DNL California Consumer Privacy Act] (CCPA)。

このドキュメントでは、のプライバシーリクエストの処理に関する基本的な概念について説明します。 [!DNL Real-time Customer Profile] Adobe Experience Platformの

>[!NOTE]
>
>このガイドでは、プライバシーでのプロファイルデータストアに対するプライバシーリクエストのExperience Platformについてのみ説明します。 Platform データレイクに対してプライバシーリクエストもおこなう予定がある場合は、 [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md) を追加しました。
>
>他の Adobe Experience Cloud アプリケーションにプライバシーリクエストを送信する手順については、[Privacy Service のドキュメント](../privacy-service/experience-cloud-apps.md)を参照してください。

## はじめに

このガイドを読む前に、次の [!DNL Experience Platform] サービスに関する十分な理解を得ることをお勧めします。

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験のフラグメント化によって発生する基本的な課題を解決します。
* [[!DNL Real-time Customer Profile]](home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイスをまたいで顧客 ID データを結び付けます。[!DNL Identity Service]**は ID 名前空間を使用して、ID の値を元のシステムと関連付け、それらの値に対するコンテキストを提供します。**&#x200B;名前空間は、電子メールアドレス（「電子メール」）などの汎用概念を表すことや、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform] の ID 名前空間について詳しくは、 [ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。

## リクエストの送信 {#submit}

以下の節では、のプライバシーリクエストをおこなう方法の概要を説明します。 [!DNL Real-time Customer Profile] の使用 [!DNL Privacy Service] API または UI これらの節を読む前に、 [Privacy ServiceAPI](../privacy-service/api/getting-started.md) または [Privacy ServiceUI](../privacy-service/ui/overview.md) リクエストペイロードで送信されたユーザー id データを適切に書式設定する方法など、プライバシージョブを送信する手順に関する完全なドキュメントです。

>[!IMPORTANT]
>
>Privacy Serviceは処理のみ可能 [!DNL Profile] id ステッチを実行しない結合ポリシーを使用するデータ。 UI を使用してプライバシーリクエストが処理されているかどうかを確認する場合は、「[!DNL None]」 [!UICONTROL ID のステッチ] タイプ。 つまり、 [!UICONTROL ID のステッチ] が&quot;に設定されている[!UICONTROL プライベートグラフ]&quot;.
>
>![](./images/privacy/no-id-stitch.png)
>
>また、プライバシーリクエストの完了に要する時間は保証できないことに注意する必要があります。 変更が [!DNL Profile] リクエストの処理中にデータを保証することはできません。

### API の使用

API でジョブリクエストを作成する際に、内で提供される ID `userIDs` は、特定の `namespace` および `type`. 有効な [id 名前空間](#namespaces) ～に認識される [!DNL Identity Service] は、 `namespace` 値、 `type` は、次のいずれかである必要があります `standard` または `unregistered` （標準名前空間とカスタム名前空間の場合）。

>[!NOTE]
>
>ID グラフと、プロファイルフラグメントを Platform データセットでどのように配布するかに応じて、各顧客に対して複数の ID を指定する必要が生じる場合があります。 次の節を参照してください [プロファイルフラグメント](#fragments) を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。リクエストを [!DNL Data Lake]の場合、配列には「ProfileService」という値を含める必要があります。

次のリクエストは、 [!DNL Profile] ストア。 顧客に対して、 `userIDs` 配列；標準を使ったもの `Email` ID 名前空間と、カスタムを使用する他の `Customer_ID` 名前空間。 また、 [!DNL Profile] (`ProfileService`) を `include` 配列：

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
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
    "regulation": "ccpa"
}'
```

>[!IMPORTANT]
>
>Platform は、すべての [サンドボックス](../sandboxes/home.md) 組織に属している。 その結果、 `x-sandbox-name` リクエストに含まれるヘッダーは、システムでは無視されます。

### UI の使用

UI でジョブリクエストを作成する場合は、「**[!UICONTROL 製品]**」の下の「**[!UICONTROL AEP Data Lake]**」または「**[!UICONTROL プロファイル]**」を必ず選択し、[!DNL Data Lake]または[!DNL Real-time Customer Profile]に保存されたデータのジョブを処理します。

<img src="images/privacy/product-value.png" width="450"><br>

## プライバシーリクエストのプロファイルフラグメント {#fragments}

内 [!DNL Profile] データストアとは、多くの場合、個々の顧客の個人データを複数のプロファイルフラグメントで構成し、id グラフを通じて個人に関連付けます。 プライバシーリクエストを [!DNL Profile] に保存する際には、要求はプロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意する必要があります。

例えば、顧客属性データを 3 つの異なるデータセットに保存し、異なる識別子を使用してそのデータを個々の顧客に関連付ける場合を考えてみましょう。

| データセット名 | プライマリID フィールド | 保存済み属性 |
| --- | --- | --- |
| データセット 1 | `customer_id` | `address` |
| データセット 2 | `email_id` | `firstName`、`lastName` |
| データセット 3 | `email_id` | `mlScore` |

データセットの 1 つが `customer_id` を主識別子として使用する一方、他の 2 つは `email_id`. 以下のみを使用してプライバシーリクエスト（アクセスまたは削除）を送信する場合 `email_id` ユーザー ID 値として、 `firstName`, `lastName`、および `mlScore` 属性が処理され、 `address` 影響を受けない

プライバシーリクエストがすべての関連する顧客属性を確実に処理するには、該当するすべてのデータセットに対して、それらの属性を保存できるプライマリ ID 値を指定する必要があります（1 人の顧客につき最大 9 つの ID）。 詳しくは、 [スキーマ構成の基本](../xdm/schema/composition.md#identity) id として一般的にマークされるフィールドについて詳しくは、を参照してください。

## リクエスト処理の削除

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。その後、レコードが [!DNL Data Lake] または [!DNL Profile] プライバシージョブが完了したら、を保存します。 削除ジョブが処理中の間、データはソフト削除されるので、誰もアクセスできません [!DNL Platform] サービス。 詳しくは、 [[!DNL Privacy Service] ドキュメント](../privacy-service/home.md#monitor) を参照してください。

>[!IMPORTANT]
>
>削除リクエストが成功すると、顧客（または顧客のセット）の収集された属性データが削除されますが、このリクエストでは、ID グラフで確立された関連付けは削除されません。
>
>例えば、顧客の `email_id` および `customer_id` は、これらの ID の下に保存されているすべての属性データを削除します。 ただし、その後、同じ `customer_id` が適切な `email_id`と呼ばれ、関連付けはまだ存在するので、

今後のリリースでは、データが物理的に削除された後、[!DNL Platform] は [!DNL Privacy Service] へと確認を送信します。

## 次の手順

このドキュメントでは、プライバシーリクエストの処理に関する重要な概念を紹介しました。 [!DNL Experience Platform]. ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

のプライバシーリクエストの処理に関する情報 [!DNL Platform] 使用されないリソース [!DNL Profile]を参照し、 [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md).
