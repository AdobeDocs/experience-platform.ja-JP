---
keywords: facebook extensions;facebook extension;facebook destinations;facebook;instagram;messenger;facebook messenger
title: Facebook の宛先
seo-title: Facebook の宛先
description: ハッシュ化された電子メールに基づいて、オーディエンスターゲティング、パーソナライズ機能および抑制のために Facebook キャンペーンのプロファイルをアクティブ化します。
seo-description: ハッシュ化された電子メールに基づいて、オーディエンスターゲティング、パーソナライズ機能および抑制のために Facebook キャンペーンのプロファイルをアクティブ化します。
translation-type: tm+mt
source-git-commit: 85e6a65e1407ca60e7b63681c045fadaaa24aef9
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 32%

---


# [!DNL Facebook] 宛先

## 概要 {#overview}

Activate profiles for your [!DNL Facebook] campaigns for audience targeting, personalization and suppression based on hashed emails.

このリンク先を、、、およびなど、によってサポートされる [!DNL Facebook’s] ファミリーアプリでのオーディエンスのターゲティングに使用でき [!DNL Custom Audiences]ます [!DNL Facebook][!DNL Instagram][!DNL Audience Network][!DNL Messenger]。 キャンペーンを実行するアプリの選択範囲が、[!DNL Facebook Ads Manager] の配置レベルで示されます。

![Real-time CDP UI内のFacebookの宛先](../../assets/catalog/social/facebook/catalog.png)

## 使用例

To help you better understand how and when you should use the [!DNL Facebook] destination, here are two sample use cases that Real-time Customer Data Platform customers can solve by using this feature.

### ユースケース 1

オンライン小売業者は、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと願っています。オンライン小売業者は、自社のCRMからReal-time CDPにEメールアドレスを取り込み、自社のオフラインデータからセグメントを構築し、これらのセグメントを [!DNL Facebook] ソーシャルプラットフォームに送信して、広告費用を最適化できます。

### ユースケース 2

航空会社には異なる顧客階層（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じてパーソナライズされたオファーを各層に提供したいと考えています。ただし、航空会社のモバイルアプリを使用していない顧客や、会社のWebサイトにログオンしていない顧客もいます。 会社がこれらの顧客に関して持っている識別子は、メンバーシップ ID と電子メールアドレスのみです。

ソーシャルメディアを通じてターゲットするために、CRMの顧客データをReal-time CDPにオンボードできます。CDPは、電子メールアドレスを識別子として使用します。

次に、関連するメンバーシップIDや顧客層を含むオフラインデータを使用して、新しいオーディエンスセグメントを作成し、 [!DNL Facebook] 宛先を通じてターゲットできるようにします。

## 宛先の詳細 {#destination-specs}

### 宛先のデータ・ガバナンス [!DNL Facebook] {#data-governance}

>[!IMPORTANT]
>
>宛先に送信されるデータには、ステッチIDが含ま [!DNL Facebook] れてはなりません。 お客様は、この義務を守る責任があります。アクティベーション用に選択したセグメントが、マージポリシーでステッチオプションを使用しないようにすることで、義務を守ることができます。 マー [ジポリシーの詳細](/help/profile/ui/merge-policies.md)。

### 書き出しタイプ {#export-type}

**セグメントエクスポート** — セグメント(オーディエンス)のすべてのメンバーを、識別子（名前、電話番号など）と共にエクスポートします。 Facebookのリンク先で使用されます。

### Facebookアカウントの前提条件 {#facebook-account-prerequisites}

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

- Your [!DNL Facebook] user account must have the **[!DNL Manage campaigns]** permission enabled for the Ad account that you plan to use.
- **Adobe Experience Cloud** ・ビジネス・アカウントは、貴社の広告パートナーとして追加する必要があり [!DNL Facebook Ad Account]ます。 `business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの「 [Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897) 」を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Real-time CDP] 統合に必要です。
- [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

### 電子メールハッシュ要件 {#email-hashing-requirements}

[!DNL Facebook] 個人識別情報(PII)を明確に送信しないようにする必要があります。 したがって、にアクティブ化するオーディエンスは、ハッシュ化された [!DNL Facebook] 電子メールアドレスからキーオフにする ** 必要があります。 電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュするか、Experience Platform内で電子メールアドレスを明確に扱ってアクティベーション上でアルゴリズムハッシュするかを選択できます。

電子メールアドレスのExperience Platformでの取り込みについては、 [バッチインジェストの概要](/help/ingestion/batch-ingestion/overview.md) 、ストリーミングインジェストの概要を参照してください [](/help/ingestion/streaming-ingestion/overview.md)。

電子メールアドレスを自分でハッシュする場合は、次の要件を満たしていることを確認してください。

- 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。例：`johndoe@example.com`（`<space>johndoe@example.com<space>` ではない）
- 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   - 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
- ハッシュ化された文字列がすべて小文字であることを確認します。
   - 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
- 文字列にソルトを使用しないでください。


>[!IMPORTANT]
>
>Eメールアドレスをハッシュしない場合は、にセグメントをアクティブ化する際に、Real-time CDPがこれを行い [!DNL Facebook]ます。 [アクティベーションワークフロー](../../ui/activate-destinations.md#activate-data) （手順5を参照）で、 `Email` 生の電子メールアドレスの場合は次に示す *オプションを選択し、ハッシュされた電子メールアドレスの場合は次* の `Email_LC_SHA256`**&#x200B;オプションを選択します。

![アクティベーションのハッシュ](../../assets/common/identity-mapping.png)

## 宛先に接続 {#connect-destination}

To connect to the [!DNL Facebook] destination, see [Social network destinations authentication workflow](./workflow.md).

## Activate segments to [!DNL Facebook] {#activate-segments}

For instructions on how to activate segments to [!DNL Facebook], see [Activate Data to Destinations](../../ui/activate-destinations.md).

## 書き出されたデータ {#exported-data}

For [!DNL Facebook], a successful activation means that a [!DNL Facebook] custom audience would be created programmatically in [[!DNL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>The integration between Real-time CDP and [!DNL Facebook] supports historical audience backfills. 宛先に対してセグメントをアクティブ化する [!DNL Facebook] と、すべての過去のセグメント資格情報がに送信されます。