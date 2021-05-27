---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
type: Documentation
description: Adobe Experience Platform Privacy Serviceは、多数のプライバシー規制に基づき、個人データに対するアクセス、販売のオプトアウト、削除のお客様のリクエストを処理します。 このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する重要な概念について説明します。
exl-id: fba21a2e-aaf7-4aae-bb3c-5bd024472214
source-git-commit: e94482532e0c5698cfe5e51ba260f89c67fa64f0
workflow-type: tm+mt
source-wordcount: '1173'
ht-degree: 28%

---

# [!DNL Real-time Customer Profile]でのプライバシーリクエストの処理

Adobe Experience Platform [!DNL Privacy Service]は、EU一般データ保護規則(GDPR)や[!DNL California Consumer Privacy Act](CCPA)などのプライバシー規制に基づき、自分の個人データに対するアクセス、販売のオプトアウト、削除の要求を処理します。

このドキュメントでは、Adobe Experience Platform内での[!DNL Real-time Customer Profile]のプライバシーリクエストの処理に関する基本的な概念について説明します。

>[!NOTE]
>
>このガイドでは、Experience Platformのプロファイルデータストアに対してプライバシーリクエストを実行する方法についてのみ説明します。 Platformデータレイクに対してもプライバシーリクエストをおこなう予定がある場合は、このチュートリアルに加えて、データレイク](../catalog/privacy.md)の[プライバシーリクエストの処理に関するガイドを参照してください。
>
>他のAdobe Experience Cloudアプリケーションにプライバシーリクエストを送信する手順については、[Privacy Serviceのドキュメント](../privacy-service/experience-cloud-apps.md)を参照してください。

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

以下の節では、[!DNL Privacy Service] APIまたはUIを使用して[!DNL Real-time Customer Profile]のプライバシーリクエストをおこなう方法について説明します。 これらの節を読む前に、リクエストペイロードで送信されたPrivacy ServiceIDデータを適切に書式設定する方法など、プライバシーPrivacy Serviceの送信手順の詳細について、 [ユーザーAPI](../privacy-service/api/getting-started.md)または[ユーザーUI](../privacy-service/ui/overview.md)のドキュメントを確認することを強くお勧めします。

>[!IMPORTANT]
>
>Privacy Serviceは、IDステッチを実行しない結合ポリシーを使用して[!DNL Profile]データのみを処理できます。 UIを使用してプライバシーリクエストが処理されているかどうかを確認する場合は、[!UICONTROL IDステッチ]タイプが「[!DNL None]」のポリシーを使用していることを確認します。 つまり、[!UICONTROL IDステッチ]が「[!UICONTROL プライベートグラフ]」に設定されている結合ポリシーは使用できません。
>
>![](./images/privacy/no-id-stitch.png)
>
>また、プライバシーリクエストが完了するまでにかかる時間は保証できないことに注意する必要があります。 リクエストの処理中に[!DNL Profile]データに変更が発生した場合は、それらのレコードが処理されるかどうかも保証できません。

### API の使用

APIでジョブリクエストを作成する場合、`userIDs`内で提供されるIDは、特定の`namespace`と`type`を使用する必要があります。 [!DNL Identity Service]の値には[ID名前空間](#namespaces)を指定し、`type`は`standard`または`unregistered`（標準名前空間とカスタム名前空間のそれぞれ）を指定する必要があります。`namespace`

>[!NOTE]
>
>IDグラフと、プロファイルフラグメントをPlatformデータセットで配布する方法に応じて、各顧客に複数のIDを提供する必要が生じる場合があります。 詳しくは、次の[プロファイルフラグメント](#fragments)を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。[!DNL Data Lake]にリクエストを送信する場合、配列には「ProfileService」という値を含める必要があります。

次のリクエストは、[!DNL Profile]ストア内の1人の顧客のデータに対して新しいプライバシージョブを作成します。 顧客に対して`userIDs`配列で2つのID値が提供されます。1つは標準の`Email` id名前空間を使用し、もう1つはカスタムの`Customer_ID`名前空間を使用します。 [!DNL Profile](`ProfileService`)の製品値も`include`配列に含まれます。

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
    "regulation": "ccpa"
}'
```

### UI の使用

UI でジョブリクエストを作成する場合は、「**[!UICONTROL 製品]**」の下の「**[!UICONTROL AEP Data Lake]**」または「**[!UICONTROL プロファイル]**」を必ず選択し、[!DNL Data Lake]または[!DNL Real-time Customer Profile]に保存されたデータのジョブを処理します。

<img src="images/privacy/product-value.png" width="450"><br>

## プライバシーリクエストのプロファイルフラグメント {#fragments}

[!DNL Profile]データストアでは、個々の顧客の個人データは、多くの場合、IDグラフを通じて個人に関連付けられた複数のプロファイルフラグメントで構成されます。 [!DNL Profile]ストアにプライバシーリクエストを送信する場合、リクエストはプロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意する必要があります。

例えば、顧客属性データを3つの異なるデータセットに保存し、異なる識別子を使用して個々の顧客に関連付ける場合を考えてみましょう。

| データセット名 | プライマリIDフィールド | 保存済み属性 |
| --- | --- | --- |
| データセット1 | `customer_id` | `address` |
| データセット2 | `email_id` | `firstName`、`lastName` |
| データセット3 | `email_id` | `mlScore` |

データセットの1つは主識別子として`customer_id`を使用し、他の2つは`email_id`を使用します。 ユーザーID値に`email_id`のみを使用してプライバシーリクエスト（アクセスまたは削除）を送信する場合、`firstName`、`lastName`および`mlScore`属性のみが処理され、`address`は影響を受けません。

プライバシーリクエストですべての関連する顧客属性を確実に処理するには、該当するすべてのデータセットのプライマリID値を指定する必要があります（1人の顧客につき最大9つのID）。 IDとして一般的にマークされるフィールドについて詳しくは、「[スキーマ構成の基本](../xdm/schema/composition.md#identity)」のIDフィールドに関する節を参照してください。

>[!NOTE]
>
>別の[サンドボックス](../sandboxes/home.md)を使用して[!DNL Profile]データを保存する場合は、`x-sandbox-name`ヘッダー内の適切なサンドボックス名を示す、各サンドボックスに対して個別のプライバシーリクエストを作成する必要があります。

## リクエスト処理の削除

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。プライバシージョブが完了すると、レコードは[!DNL Data Lake]または[!DNL Profile]ストアから削除されます。 削除ジョブが処理中の間、データはソフト削除されるので、[!DNL Platform]サービスからはアクセスできません。 ジョブステータスの追跡について詳しくは、[[!DNL Privacy Service] ドキュメント](../privacy-service/home.md#monitor)を参照してください。

>[!IMPORTANT]
>
>削除リクエストが成功すると、顧客（または顧客のセット）の収集された属性データが削除されますが、このリクエストでは、IDグラフで確立された関連付けは削除されません。
>
>例えば、顧客の`email_id`と`customer_id`を使用する削除リクエストでは、これらのIDの下に保存されているすべての属性データが削除されます。 ただし、同じ`customer_id`の下で取り込まれたデータは、関連付けがまだ存在するので、適切な`email_id`に関連付けられます。

今後のリリースでは、データが物理的に削除された後、[!DNL Platform] は [!DNL Privacy Service] へと確認を送信します。

## 次の手順

このドキュメントでは、[!DNL Experience Platform]でのプライバシーリクエストの処理に関する重要な概念について説明しました。 ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

[!DNL Profile]で使用されない[!DNL Platform]リソースのプライバシーリクエストの処理について詳しくは、データレイク](../catalog/privacy.md)の[プライバシーリクエストの処理に関するドキュメントを参照してください。
