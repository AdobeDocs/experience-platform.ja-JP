---
title: キャピラリーストリーミングイベントの概要
description: キャピラリからExperience Platformにデータをストリーミングする方法を説明します。
hide: true
hidefromtoc: true
badge: ベータ版
exl-id: 3b8eb2f6-3b4a-4b91-89d4-b6d9027c6ab4
source-git-commit: b3b1542f7e297f4ca872a155ac3801266bc1e6a6
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 7%

---

# [!DNL Capillary Streaming Events]

>[!AVAILABILITY]
>
>[!DNL Capillary Streaming Events] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、ソースの概要の [ 利用条件 ](../../home.md#terms-and-conditions) を参照してください。

[!DNL Capillary Technologies] は、世界中の 300 以上のブランドから信頼されている、主要なロイヤルティおよびエンゲージメントプラットフォームです。 [!DNL Capillary Streaming Events] ソースを使用すると、ビジネスが顧客プロファイル、ロイヤルティデータ、トランザクションイベントを、[!DNL Capillary] からAdobe Experience Platformへとシームレスにストリーミングできるようになります。 [!DNL Capillary] ソースを接続すると、リアルタイムのパーソナライゼーション、高度なオーディエンスのセグメント化、オムニチャネルキャンペーンオーケストレーションが可能になります。

[!DNL Capillary] をExperience Platformと統合すると、次のことが可能になります。

* **ロイヤルティポイント、階層および報酬** をリアルタイムで同期します。
* **トランザクションデータ** をExperience Platformに送信して、分析とアクティベーションを行います。
* Real-Time CDP、Experience Platform、Adobe Journey Optimizerを活用して、セグメント化、Journey Orchestration、パーソナライゼーションを行います。

## 前提条件

[!DNL Capillary] をAdobe Experience Platformに接続する前に、次のことを確認してください。

* 有効な **Adobe組織 ID** と、有効なExperience Platform サンドボックスへのアクセス。
* ソ **[!DNL Capillary]ス資格情報** （クライアント ID およびクライアント秘密鍵）。
* ソースとデータフローを作成するために必要なAdobe Admin Console内の権限。

### 必要な資格情報の収集

[!DNL Capillary] アカウントをExperience Platformに接続するには、次の資格情報の値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| クライアント ID | [!DNL Capillary] ソースのクライアント識別子。 | `321c8a8fee0d4a06838d46f9d3109e8a` |
| クライアント秘密鍵 | クライアント ID で発行されたクライアント秘密鍵 | `xxxxxxxxxxxxxxxxxx` |
| 組織 ID | Adobe組織 ID | `0A7D42FC5DB9D3360A495FD3@AdobeOrg` |

アクセストークンの生成について詳しくは、[Adobe認証ガイド ](https://developer.adobe.com/developer-console/docs/guides/authentication/) を参照してください。

### アクセストークンの生成

次に、クライアント ID とクライアント秘密鍵を使用して、Adobeからアクセストークンを生成します。

**リクエスト**

```shell
curl -X POST 'https://ims-na1.adobelogin.com/ims/token' \
  -d 'client_id={CLIENT_ID}' \
  -d 'client_secret={CLIENT_SECRET}' \
  -d 'grant_type=client_credentials' \
  -d 'scope=openid AdobeID read_organizations additional_info.projectedProductContext session'
```

**応答**

```json
{
  "access_token": "eyJhbGciOi...",
  "token_type": "bearer",
  "expires_in": 86399994
}
```

## 次の手順

[!DNL Capillary] の前提条件の設定が完了したら、次のドキュメントを参照して、アカウントを接続し、[!DNL Capillary] からExperience Platformへのストリーミングデータのストリーミングを開始する方法を確認します。

* [API [!DNL Capillary Streaming Events]  使用したExperience Platformへの接続](../../tutorials/api/create/loyalty/capillary.md)
* [UI [!DNL Capillary Streaming Events]  使用したExperience Platformへの接続](../../tutorials/ui/create/loyalty/capillary.md)
