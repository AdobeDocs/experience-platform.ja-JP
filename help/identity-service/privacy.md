---
keywords: Experience Platform;ホーム;人気のトピック
title: ID サービスでのプライバシーリクエストの処理
description: Adobe Experience Platform Privacy Serviceは、多数のプライバシー規制に従って、個人データへのアクセス、販売のオプトアウト、削除の顧客リクエストを処理します。 このドキュメントでは、ID サービスのプライバシーリクエストの処理に関する基本的な概念について説明します。
source-git-commit: 49f5de6c4711120306bfc3e6759ed4e83e8a19c2
workflow-type: tm+mt
source-wordcount: '666'
ht-degree: 40%

---


# でのプライバシーリクエストの処理 [!DNL Identity Service]

Adobe Experience Platform [!DNL Privacy Service] は、一般データ保護規則 (GDPR) や [!DNL California Consumer Privacy Act] (CCPA)。

このドキュメントでは、のプライバシーリクエストの処理に関する基本的な概念について説明します。 [!DNL Identity Service] Adobe Experience Platformの

>[!NOTE]
>
>このガイドでは、Experience Platformの ID データストアに対してプライバシーリクエストを実行する方法についてのみ説明します。 Platform データレイクに対してプライバシーリクエストもおこなう予定の場合、または [!DNL Real-time Customer Profile]( [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md) そして [プロファイルのプライバシーリクエストの処理](../profile/privacy.md) を追加しました。
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

以下の節では、のプライバシーリクエストをおこなう方法の概要を説明します。 [!DNL Identity Service] の使用 [!DNL Privacy Service] API または UI これらの節を読む前に、 [Privacy ServiceAPI](../privacy-service/api/getting-started.md) または [Privacy ServiceUI](../privacy-service/ui/overview.md) リクエストペイロードのユーザーデータを適切に書式設定する方法など、プライバシージョブを送信する手順に関する完全なドキュメントです。

### API の使用

API でジョブリクエストを作成する際に、内で提供される ID `userIDs` は、特定の `namespace` および `type`. 有効な [id 名前空間](#namespaces) ～に認識される [!DNL Identity Service] は、 `namespace` 値、 `type` は、次のいずれかである必要があります `standard` または `unregistered` （標準名前空間とカスタム名前空間の場合）。

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。にリクエストする際 [!DNL Identity]の場合、配列には値を含める必要があります `Identity`.

次のリクエストは、GDPR の下に、 [!DNL Identity] ストア。 顧客に対して、 `userIDs` 配列；標準を使ったもの `Email` id 名前空間と、他の id 名前空間を使用 `ECID` 名前空間に含まれ、 [!DNL Identity] (`Identity`) を `include` 配列：

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer <key>' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: acp_privacy_ui_gdpr' \
  -H 'x-gw-ims-org-id: sample@AdobeOrg' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "sample@AdobeOrg"
      }
    ],
    "users": [
      {
        "key": "bob",
        "action": ["delete"],
        "userIDs": [
          {
            "namespace": "email",
            "value": "bob@adobe.com",
            "type": "standard"
          },
          {
            "namespace": "ECID",
            "type": "standard",
            "value":  "123451234512345123451234512345",
            "isDeletedClientSide": false
          }
        ]
      }
    ],
    "include": ["Identity"],
    "regulation": "gdpr"
}'
```

### UI の使用

UI でジョブリクエストを作成する場合は、必ず **[!UICONTROL ID]** under **[!UICONTROL 製品]** に保存されたデータのジョブを処理するために [!DNL Identity Service].

![identity-gdpr](./images/identity-gdpr.png)

## リクエスト処理の削除

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。個々の ID の削除は、提供された名前空間または ID 値に基づいておこなわれます。 さらに、特定の IMS 組織に関連付けられているすべてのサンドボックスに対して、削除がおこなわれます。

## 次の手順

このドキュメントでは、プライバシーリクエストの処理に関する重要な概念を紹介しました。 [!DNL Identity Service]. 他のユーザーのプライバシーリクエストの処理に関する情報 [!DNL Experience Cloud] アプリケーション、次のドキュメントを参照 [[!DNL Privacy Service] and [!DNL Experience Cloud] アプリ](../privacy-service/experience-cloud-apps.md).