---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
topic: overview
translation-type: tm+mt
source-git-commit: 5f0e0deb4a2fae636ac4d2313a6541c25309c294

---


# リアルタイム顧客プロファイルでのプライバシーリクエストの処理

Adobe Experience Platform Privacy Serviceは、General Data Protection Regulation(GDPR)やCalifornia Consumer Privacy Act(CCPA)などのプライバシー規制に基づいて定義された個人データのアクセス、販売、または削除を求める顧客の要求を処理します。

このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する重要な概念について説明します。

## はじめに

このガイドを読む前に、次のExperience Platformサービスに関する十分な理解を得ることをお勧めします。

* [プライバシーサービス](home.md):Adobe Experience Cloudアプリケーションで個人データにアクセス、オプトアウト、または削除する際の顧客の要求を管理します。
* [IDサービス](../identity-service/home.md):デバイスやシステム間でIDをブリッジングすることで顧客体験データの断片化に伴う基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

## ID名前空間 {#namespaces}

Adobe Experience Platform Identity Serviceは、顧客のIDデータをシステムやデバイス間で橋渡しします。 アイデンティティ **名前空間** は、アイデンティティのシステムに関連付けることで、アイデンティティの値に対するコンテキストを提供するために使用されます。接触チャネル 名前空間は、電子メールアドレス（「電子メール」）などの汎用概念を表すことや、IDを特定のアプリケーション(Adobe Advertising Cloud ID(「AdCloud」)やAdobeターゲットID(「TNTID」)など)に関連付けることができます。

IDサービスは、グローバルに定義された（標準）IDおよびユーザー定義の（カスタム）ID名前空間を保持します。 標準名前空間は、すべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

Experience PlatformのID名前空間について詳しくは、ID名前空間の概要を参照し [てください](../identity-service/namespaces.md)。

## 要求の送信 {#submit}

>[!NOTE] この節では、Data Lakeのプライバシー要求の形式を設定する方法について説明します。 リクエストペイロードで送信されたユーザーIDデータを適切にフォーマットする方法など、プライバシージョブの送信方法に関する完全な手順については、 [Privacy Service API](../privacy-service/api/getting-started.md) （プライバシーサービスAPI）または [Privacy Service UI](../privacy-service/ui/overview.md) （プライバシーサービスUI）のドキュメントを確認することを強くお勧めします。

次の節では、プライバシーサービスAPIまたはUIを使用して、リアルタイム顧客プロファイルとData Lakeに対してプライバシーリクエストを行う方法について説明します。

### APIの使用

APIでジョブリクエストを作成する場合、提供され `userIDs` るすべてのユーザーは、適用するデータスト `namespace` アに応じ `type` て、特定のを使用する必要があります。 プロファイルストアのIDは、値に「標準」または「カスタム」を使用し、値に対してIdentity Serviceで認識される有効な `type` ID [名前空間を使用する必要があり](#namespaces)`namespace` ます。


さらに、要求ペイロ `include` ードの配列には、要求が行われる別のデータストアの製品値を含める必要があります。 Data Lakeにリクエストを行う場合、配列には「ProfileService」という値を含める必要があります。

次のリクエストは、標準の「電子メール」ID名前空間を使用して、リアルタイム顧客プロファイルの両方に新しいプライバシージョブを作成します。 また、アレイ内のプロファイルの製品値も含まれ `include` ます。

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

UIでジョブリクエストを作成する場合は、「 **Products** 」の下の「 **AEP Data Lake** 」または「 __ プロファイル」を必ず選択し、「Data Lake」または「リアルタイム顧客プロファイル」に保存されたデータのジョブを処理します。

<img src="images/privacy/product-value.png" width="450"><br>

## リクエスト処理の削除

Experience Platformがプライバシーサービスから削除リクエストを受信すると、プラットフォームは、リクエストが受信され、影響を受けるデータが削除用にマークされたことをプライバシーサービスに確認するメッセージを送信します。 その後、7日以内にレコードがData Lakeまたはプロファイルストアから削除されます。 この7日間の期間中は、データはソフト削除されるので、どのプラットフォームサービスからもアクセスできません。

今後のリリースでは、データが物理的に削除された後、プラットフォームはプライバシーサービスに確認を送信します。

## 次の手順

このドキュメントを読むことで、Experience Platformでのプライバシーリクエストの処理に関わる重要な概念について説明します。 IDデータの管理方法とプライバシージョブの作成方法についての理解を深めるために、このガイド全体に記載されているドキュメントを読み続けることをお勧めします。

プロファイルが使用しないプラットフォームリソースのプライバシー要求の処理に関するドキュメントについては、Data Lakeのプラ [イバシー要求の処理に関する情報を参照してください](../catalog/privacy.md)。