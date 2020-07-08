---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 0%

---


# リアルタイム顧客プロファイルでのプライバシーリクエストの処理

Adobe Experience Platform Privacy Serviceは、GDPR(General Data Protection Regulation)やCCPA(California Consumer Privacy Act)などのプライバシー規制に基づく、個人データのアクセス、販売、削除の要求を処理します。

このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストを処理する場合に必要な基本概念について説明します。

## はじめに

このガイドを読む前に、次のExperience Platformサービスに関する十分な理解を得ておくことをお勧めします。

* [Privacy Service](home.md): Adobe Experience Cloudアプリケーションで個人データにアクセス、オプトアウト、削除を行う際の顧客の要求を管理します。
* [IDサービス](../identity-service/home.md): デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## ID名前空間について {#namespaces}

Adobe Experience PlatformIDサービスは、システムとデバイス間で顧客IDデータを結合します。 アイデンティティサービスは **ID名前空間** を使用して、ID値を接触チャネルのシステムに関連付けることで、ID値のコンテキストを提供します。 名前空間は、電子メールアドレス（「電子メール」）などの汎用的な概念を表したり、IDを特定のAdvertising Cloud(AdCloud)やAdobe TargetID(「TNTID」)など)に関連付けたりできます。

アイデンティティサービスは、グローバルに定義された（標準）ID名前空間とユーザー定義の（カスタム）IDユーザーのストアを管理します。 標準名前空間は、すべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

Experience PlatformでのID名前空間について詳しくは、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。

## 要求の送信 {#submit}

>[!NOTE]
>
>この節では、プロファイルのデータストアに対するプライバシー要求を作成する方法について説明します。 リクエストペイロードで送信されたユーザーIDデータを適切にフォーマットする方法など、プライバシージョブの送信方法に関する完全な手順については、 [Privacy ServiceAPI](../privacy-service/api/getting-started.md)[(](../privacy-service/ui/overview.md) Privacy ServiceUI)のドキュメントを確認することを強くお勧めします。

次の節では、Privacy ServiceAPIまたはUIを使用してリアルタイムの顧客プロファイルとData Lakeに対してプライバシーリクエストを行う方法について概要を説明します。

### APIの使用

APIでジョブリクエストを作成する場合、提供され `userIDs` るジョブリクエストは、適用するデータストアに応じて、特定の `namespace` を使用する `type` 必要があります。 プロファイルストアのIDは、値に「標準」または「カスタム」を使用し、その値に対してIdentity Serviceで認識される有効な `type` ID名前空間 [を使用する必要があり](#namespaces)`namespace` ます。


さらに、要求ペイロードの `include` 配列には、要求が行われる別のデータストアの製品値を含める必要があります。 データレークに要求を行う場合、配列には「ProfileService」という値を含める必要があります。

次のリクエストは、標準の「電子メール」ID名前空間を使用して、リアルタイム顧客プロファイルの両方に新しいプライバシージョブを作成します。 また、 `include` アレイ内のプロファイルの製品値も含まれます。

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

UIでジョブリクエストを作成する場合、Data LakeまたはReal-time Customerプロファイルに保存されたデータのジョブを処理するには、「 **Products** (製品 **)」で「AEP Data Lake** (プロファイル)」または「 __ AEP Data Lake（ジョブ）」を必ず選択してください。

<img src="images/privacy/product-value.png" width="450"><br>

## 削除要求処理

Experience PlatformがPrivacy Serviceから削除要求を受け取ると、Platformは、要求が受信され、影響を受けたデータが削除のマークを付けられたことをPrivacy Serviceに確認するメッセージを送信します。 その後、7日以内にレコードがData Lakeまたはプロファイルストアから削除されます。 その7日間の期間は、データがソフト削除されるので、どのPlatformサービスからもアクセスできません。

今後のリリースでは、データが物理的に削除された後、PlatformはPrivacy Serviceに確認メッセージを送信する予定です。

## 次の手順

このドキュメントを読むことで、Experience Platformでのプライバシー要求の処理に関連する重要な概念について説明します。 IDデータの管理方法とプライバシージョブの作成方法についての理解を深めるために、このガイドに記載されているドキュメントを読み続けることをお勧めします。

プロファイルが使用しないPlatformリソースに対するプライバシー要求の処理について詳しくは、データレークの [プライバシー要求処理に関するドキュメントを参照してください](../catalog/privacy.md)。