---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
topic: overview
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---


# プライバシーリクエストの処理( [!DNL Real-time Customer Profile]

Adobe Experience Platform [!DNL Privacy Service] は、一般的なデータ保護規則(GDPR)や [!DNL California Consumer Privacy Act] (CCPA)などのプライバシー規則に従って、個人データのアクセス、販売、削除に関するお客様の要求を処理します。

このドキュメントでは、のプライバシー要求の処理に関する重要な概念について説明 [!DNL Real-time Customer Profile]します。

## はじめに

このガイドを読む前に、次の [!DNL Experience Platform] サービスに関する作業上の理解を得ておくことをお勧めします。

* [!DNL Privacy Service](home.md): Adobe Experience Cloudアプリケーションで個人データにアクセス、オプトアウト、削除を行う場合の顧客の要求を管理します。
* [!DNL Identity Service](../identity-service/home.md): デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [!DNL Real-time Customer Profile](../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## ID名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイス間で顧客のIDデータを結合します。 [!DNL Identity Service] は、 **identity名前空間** を使用して、identity値に対するコンテキストを、その接触チャネルのシステムに関連付けることで提供します。 名前空間は、電子メールアドレス（「電子メール」）などの汎用的な概念を表したり、IDを特定のAdvertising Cloud(AdCloud)やAdobe TargetID(「TNTID」)など)に関連付けたりできます。

アイデンティティサービスは、グローバルに定義された（標準）ID名前空間とユーザー定義の（カスタム）IDユーザーのストアを管理します。 標準名前空間は、すべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

のID名前空間について詳し [!DNL Experience Platform]くは、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。

## 要求の送信 {#submit}

>[!NOTE]
>
>この節では、 [!DNL Profile] データストアのプライバシーリクエストを作成する方法について説明します。 リクエストペイロードで送信されたユーザーIDデータを適切にフォーマットする方法など、プライバシージョブの送信方法に関する完全な手順については、 [Privacy ServiceAPI](../privacy-service/api/getting-started.md)[(](../privacy-service/ui/overview.md) Privacy ServiceUI)のドキュメントを確認することを強くお勧めします。

次の節では、APIまたはUIを使用して、およびにプライバシーリクエストを行う方法 [!DNL Real-time Customer Profile] の概要を説明し [!DNL Data Lake][!DNL Privacy Service] ます。

### APIの使用

APIでジョブリクエストを作成する場合、提供され `userIDs` るジョブリクエストは、適用するデータストアに応じて、特定の `namespace` を使用する `type` 必要があります。 ストアのIDは、その値に「standard」または「custom」を使用し、その値に対して [!DNL Profile] 「 `type` standard」または「custom」が認識される有効な [ID名前空間](#namespaces) を使用する必要があり [!DNL Identity Service]`namespace` ます。


さらに、要求ペイロードの `include` 配列には、要求が行われる別のデータストアの製品値を含める必要があります。 に対して要求を行う場合 [!DNL Data Lake]、配列には「ProfileService」という値を含める必要があります。

次の要求は、標準の「電子メール」ID名前空間を使用して、両方に対して新しいプライバシージョブ [!DNL Real-time Customer Profile]を作成します。 また、次の [!DNL Profile]`include` 配列のの製品値も含まれます。

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

### UIの使用

UIでジョブリクエストを作成する場合、Productsの下で **[!UICONTROL AEP Data Lake]** または **[!UICONTROL プロファイル]** （またはその両方）を選択して、それぞれ _[!UICONTROL に格納されているジョブを]_[!DNL Data Lake][!DNL Real-time Customer Profile]UIで処理します。

<img src="images/privacy/product-value.png" width="450"><br>

## 削除要求処理

から削除リクエスト [!DNL Experience Platform] を受け取ると、リクエストが受信され、影響を受けるデータが削除のマーク [!DNL Privacy Service]が付けられたこと [!DNL Platform] を [!DNL Privacy Service] 確認するメッセージがに送信されます。 その後、7日以内にレコードがまたは [!DNL Data Lake][!DNL Profile] ストアから削除されます。 その7日間の期間は、データはソフトデリートされるので、どの [!DNL Platform] サービスからもアクセスできません。

今後のリリースでは、データ [!DNL Platform] が物理的に削除された [!DNL Privacy Service] 後に、に確認メッセージが送信されます。

## 次の手順

このドキュメントを読むことで、のプライバシー要求の処理に関連する重要な概念について説明し [!DNL Experience Platform]ます。 IDデータの管理方法とプライバシージョブの作成方法についての理解を深めるために、このガイドに記載されているドキュメントを読み続けることをお勧めします。

が使用しない [!DNL Platform] リソースに対するプライバシー要求の処理について詳し [!DNL Profile]くは、Data Lakeの [プライバシー要求処理に関するドキュメントを参照してください](../catalog/privacy.md)。