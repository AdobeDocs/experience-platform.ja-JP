---
title: Facebookのリンク先
seo-title: Facebookのリンク先
description: ハッシュされた電子メールに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制に使用するFacebookキャンペーンのプロファイルをアクティブにします。
seo-description: ハッシュされた電子メールに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制に使用するFacebookキャンペーンのプロファイルをアクティブにします。
translation-type: tm+mt
source-git-commit: 79aecf4955507622ac7879c148cdcd23e893dd65
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 24%

---


# Facebookのリンク先

## 概要 {#overview}

ハッシュされた電子メールに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制に使用するFacebookキャンペーンのプロファイルをアクティブにします。

![Real-time CDP UI内のFacebookの宛先](/help/rtcdp/destinations/assets/facebook-destination.png)

## 使用例

Facebookの宛先を使用する方法とタイミングをより深く理解するために、Adobe Real-time Customer Data Platformのお客様がこの機能を使用して解決できる使用例を2つ示します。


### ユースケース 1


オンライン小売業者は、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと願っています。オンライン小売業者は、自社のCRMからAdobe Real-time CDPに電子メールアドレスを取り込み、自社のオフラインデータからセグメントを構築し、これらのセグメントをFacebookソーシャルプラットフォームに送信して、広告費用を最適化できます。


### ユースケース 2


航空会社には異なる顧客階層（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じてパーソナライズされたオファーを各層に提供したいと考えています。ただし、航空会社のモバイルアプリを使用していない顧客や、会社のWebサイトにログオンしていない顧客もいます。 会社がこれらの顧客に関して持っている識別子は、メンバーシップ ID と電子メールアドレスのみです。

ソーシャルメディアを介してターゲットするには、電子メールアドレスを識別子として使用し、CRMの顧客データをAdobe Real-time CDPにオンボードできます。

次に、関連するメンバーシップIDや顧客層を含むオフラインデータを使用して、Facebookの宛先でターゲットできる新しいオーディエンスセグメントを作成できます。

## 宛先の詳細 {#destination-specs}

### Facebookの宛先に対するデータガバナンス {#data-governance}

>[!IMPORTANT]
>
>Facebookに送信されるデータには、ステッチ済みIDは含めないでください。 お客様は、この義務を守る責任があります。アクティベーション用に選択したセグメントが、マージポリシーでステッチオプションを使用しないようにすることで、義務を守ることができます。 マー [ジポリシーの詳細](/help/profile/ui/merge-policies.md)。

### アクティベーションタイプ {#activation-type}

**セグメントエクスポート** — セグメント(オーディエンス)のすべてのメンバーを、識別子（名前、電話番号など）と共にエクスポートします。 Facebookのリンク先で使用されます。

### Facebookアカウントの前提条件 {#facebook-account-prerequisites}

Before you can send your audience segments to [!DNL Facebook], make sure you meet the following requirements:

1. Your [!DNL Facebook] user account must have the **[!DNL Manage campaigns]** permission enabled for the Ad account that you plan to use.
2. **Adobe Experience Cloud** ビジネスアカウントを [!DNL Facebook Ad Account] の広告パートナーとして追加します。`business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの「 [Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897) 」を参照してください。
   >[!IMPORTANT]
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Real-time CDP] 統合に必要です。
3. [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

### 電子メールハッシュ要件 {#email-hashing-requirements}

Facebookでは、個人を特定できる情報(PII)を明確に送信しないようにする必要があります。 したがって、Facebookにアクティブ化されたオーディエンスは、ハッシュ化された *電子メールアドレスを無効にする必要があります* 。 電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュするか、Experience Platformで電子メールアドレスを明確に扱って、アクティベーション上でアルゴリズムにハッシュするかを選択できます。

エクスペリエンスプラットフォームでの電子メールアドレスの取り込みについて詳しくは、 [バッチインジェストの概要](/help/ingestion/batch-ingestion/overview.md) 、ストリーミングインジェストの概要を参照してください [](/help/ingestion/streaming-ingestion/overview.md)。

電子メールアドレスを自分でハッシュする場合は、次の要件を満たしていることを確認してください。

* 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。例：`johndoe@example.com`（`<space>johndoe@example.com<space>` ではない）
* 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   * 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
* ハッシュ化された文字列がすべて小文字であることを確認します。
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
* 文字列にソルトを使用しないでください。


>[!IMPORTANT]
>
>電子メールアドレスをハッシュしない場合は、Facebookに対してセグメントをアクティブ化する際に、Adobe Real-time CDPが実行します。 [アクティベーションワークフロー](/help/rtcdp/destinations/activate-destinations.md#activate-data) （手順5を参照）で、 `Email` 生の電子メールアドレスの場合は次に示す *オプションを選択し、ハッシュされた電子メールアドレスの場合は次* の `Email_LC_SHA256`**&#x200B;オプションを選択します。


![アクティベーションのハッシュ](/help/rtcdp/destinations/assets/identity-mapping.png)

## 宛先に接続 {#connect-destination}

Facebookの宛先に接続するには、 [ソーシャルネットワークの宛先認証ワークフローを参照してください](/help/rtcdp/destinations/social-network-destinations-workflow.md)。


## Facebookへのセグメントのアクティブ化 {#activate-segments}

For instructions on how to activate segments to Facebook, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).