---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
type: Documentation
description: Adobe Experience Platform Privacy Serviceは、多数のプライバシー規制に基づオプトアウトく説明に従って、個人データのアクセス、販売、削除を求める顧客の要求を処理します。 このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する重要な概念について説明します。
exl-id: fba21a2e-aaf7-4aae-bb3c-5bd024472214
translation-type: tm+mt
source-git-commit: 8d16a3030c663d40daed6c5105af07b2d2d5c7bf
workflow-type: tm+mt
source-wordcount: '1102'
ht-degree: 16%

---

# [!DNL Real-time Customer Profile]でのプライバシー要求の処理

Adobe Experience Platform[!DNL Privacy Service]は、一般的なデータ保護規則(GDPR)や[!DNL California Consumer Privacy Act] (CCPA)などのプライバシー規則に従って、個人データのアクセス、販売、または削除を求めるお客様の要求を処理します。

このドキュメントでは、[!DNL Real-time Customer Profile]のプライバシー要求の処理に関する基本的な概念について説明します。

## はじめに

このガイドを読む前に、次の[!DNL Experience Platform]サービスに関する作業を理解しておくことをお勧めします。

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Identity Service]](../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。
* [[!DNL Real-time Customer Profile]](home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform[!DNL Identity Service]は、システムとデバイス間で顧客のIDデータを結合します。 [!DNL Identity Service] は、 **identity** namespaceを使用して、identity値に対するコンテキストを提供します。このコンテキストは、接触チャネルのシステムに関連付けられます。名前空間は、電子メールアドレス（「電子メール」）などの汎用概念を表すことや、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform]のID名前空間について詳しくは、[ID名前空間の概要](../identity-service/namespaces.md)を参照してください。

## リクエストの送信 {#submit}

以下の節では、[!DNL Privacy Service] APIまたはUIを使用して[!DNL Real-time Customer Profile]のプライバシーリクエストを行う方法について説明します。 これらの節を読む前に、[Privacy ServiceAPI](../privacy-service/api/getting-started.md)または[Privacy ServiceUI](../privacy-service/ui/overview.md)のドキュメントを確認し、プライバシージョブの送信方法（リクエストペイロードで送信ユーザーIDデータを正しく書式設定する方法）を確認することを強くお勧めします。

>[!IMPORTANT]
>
>Privacy Serviceは、IDのステッチを実行しないマージポリシーを使用して[!DNL Profile]データのみを処理できます。 UIを使用してプライバシー要求が処理されているかどうかを確認する場合は、[!UICONTROL IDステッチ]タイプとして「[!DNL None]」を持つポリシーを使用していることを確認してください。 つまり、[!UICONTROL IDの切り替え]が&quot;[!UICONTROL プライベートグラフ]&quot;に設定されているマージポリシーは使用できません。
>
>![](./images/privacy/no-id-stitch.png)
>
>また、プライバシー要求が完了するまでに要する時間を保証できないことにも注意する必要があります。 リクエストの処理中に[!DNL Profile]データに変更が発生した場合、それらのレコードが処理されるかどうかも保証できません。

### API の使用

APIでジョブリクエストを作成する場合、`userIDs`内で提供されるIDは、特定の`namespace`と`type`を使用する必要があります。 [!DNL Identity Service]で認識される有効な[ID名前空間](#namespaces)は`namespace`値に指定する必要がありますが、`type`は`standard`または`unregistered`(標準名前空間とカスタムそれぞれ)にする必要があります。

>[!NOTE]
>
>IDグラフや、プロファイルフラグメントがプラットフォームデータセット内でどのように分散されるかに応じて、各顧客に対して複数のIDを指定する必要がある場合があります。 詳しくは、次の[プロファイルフラグメント](#fragments)を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。[!DNL Data Lake]に要求を行う場合、配列には値&quot;ProfileService&quot;を含める必要があります。

次のリクエストは、[!DNL Profile]ストア内の1人の顧客のデータに対して新しいプライバシージョブを作成します。 `userIDs`アレイの顧客に対して2つのID値が提供されます。1つは標準の`Email` ID名前空間を使用し、もう1つはカスタムの`Customer_ID`名前空間を使用しています。 また、`include`配列の[!DNL Profile] (`ProfileService`)の製品値も含みます。

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

UIでジョブリクエストを作成する場合、[!DNL Data Lake]または[!DNL Real-time Customer Profile]に保存されたデータのジョブを処理するには、**[!UICONTROL Products]**&#x200B;の下で&#x200B;**[!UICONTROL AEP Data Lake]**&#x200B;または&#x200B;**[!UICONTROL プロファイル]**&#x200B;を必ず選択してください。

<img src="images/privacy/product-value.png" width="450"><br>

## プライバシー要求{#fragments}のプロファイルフラグメント

[!DNL Profile]データストアでは、個々の顧客の個人データが複数のプロファイルフラグメントで構成される場合が多く、これらはIDグラフを介して個人に関連付けられます。 [!DNL Profile]ストアにプライバシーリクエストを送信する場合、リクエストはプロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意する必要があります。

例えば、顧客属性データを3つの異なるデータセットに格納する場合、異なる識別子を使用して個々の顧客に関連付けます。

| データセット名 | プライマリIDフィールド | 保存された属性 |
| --- | --- | --- |
| データセット1 | `customer_id` | `address` |
| データセット2 | `email_id` | `firstName`、`lastName` |
| データセット3 | `email_id` | `mlScore` |

データセットの1つは主識別子として`customer_id`を使用し、他の2つは`email_id`を使用します。 ユーザーIDの値として`email_id`のみを使用してプライバシーリクエスト（アクセスまたは削除）を送信する場合、`firstName`、`lastName`、`mlScore`の属性のみが処理され、`address`は影響を受けません。

プライバシー要求ですべての関連する顧客属性を確実に処理するには、該当するすべてのデータセットに対し、それらの属性を保存できるプライマリID値を指定する必要があります（1人の顧客につき最大9個のID）。 一般的にIDとしてマークされるフィールドの詳細については、「スキーマ構成の基本[」の「IDフィールド」の節を参照してください。](../xdm/schema/composition.md#identity)

>[!NOTE]
>
>異なる[サンドボックス](../sandboxes/home.md)を使用して[!DNL Profile]データを保存する場合は、`x-sandbox-name`ヘッダーに適切なサンドボックス名を示す、各サンドボックスに対して個別のプライバシーリクエストを行う必要があります。

## リクエスト処理の削除

[!DNL Experience Platform]が[!DNL Privacy Service]から削除要求を受け取ると、[!DNL Platform]は、要求が受信され、影響を受けたデータが削除のマークが付けられたことを[!DNL Privacy Service]に確認します。 プライバシージョブが完了すると、レコードは[!DNL Data Lake]または[!DNL Profile]ストアから削除されます。 削除ジョブが処理中の間は、データはソフト削除されるので、どの[!DNL Platform]サービスからもアクセスできません。 ジョブのステータスの追跡の詳細については、[[!DNL Privacy Service] ドキュメント](../privacy-service/home.md#monitor)を参照してください。

>[!IMPORTANT]
>
>削除が成功すると、顧客（または顧客のセット）の収集された属性データが削除されますが、このリクエストでは、IDグラフに設定された関連付けは削除されません。
>
>例えば、顧客の`email_id`と`customer_id`を使用する削除リクエストでは、これらのIDに格納されているすべての属性データが削除されます。 しかし、その後同じ`customer_id`の下で取り込まれるデータは、関連付けが存在するので、適切な`email_id`に関連付けられます。

今後のリリースでは、[!DNL Platform]は、データが物理的に削除された後、[!DNL Privacy Service]に確認を送ります。

## 次の手順

このドキュメントを読むと、[!DNL Experience Platform]でのプライバシー要求の処理に関する重要な概念が紹介されます。 ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

[!DNL Profile]が使用しない[!DNL Platform]リソースのプライバシー要求の処理について詳しくは、データレーク](../catalog/privacy.md)の[プライバシー要求処理のドキュメントを参照してください。
