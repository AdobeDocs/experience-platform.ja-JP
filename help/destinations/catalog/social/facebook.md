---
keywords: facebook接続；facebook接続；facebookの宛先；facebook;instagram;messenger;facebook Messenger
title: Facebook接続
description: ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのFacebookキャンペーンのプロファイルをアクティブ化します。
exl-id: 51e8c8f0-5e79-45b9-afbc-110bae127f76
source-git-commit: 2faf27495c3785a27613db917c7416e1d7b08c4d
workflow-type: tm+mt
source-wordcount: '1521'
ht-degree: 12%

---

# [!DNL Facebook] 接続

## 概要 {#overview}

ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーション、抑制のための [!DNL Facebook] キャンペーンのプロファイルをアクティブ化します。

この宛先を、[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]、[!DNL Messenger] など、[!DNL Custom Audiences] でサポートされるアプリケーションファミリー全体でのオーディエンスのターゲティングに使用できます。 [!DNL Facebook’s]キャンペーンを実行するアプリの選択範囲が、[!DNL Facebook Ads Manager] の配置レベルで示されます。

![Adobe Experience Platform UI でのfacebookの宛先](../../assets/catalog/social/facebook/catalog.png)

## ユースケース

[!DNL Facebook] の宛先をいつどのように使用するかをより深く理解できるように、Adobe Experience Platformのお客様がこの機能を使用して解決できる、2 つの使用例を以下に示します。

### 使用例#1

オンライン小売業者は、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと願っています。オンライン小売業者は、独自の CRM からAdobe Experience Platformに電子メールアドレスを取り込み、独自のオフラインデータからセグメントを作成し、これらのセグメントを [!DNL Facebook] ソーシャルプラットフォームに送信して、広告費用を最適化できます。

### 使用例#2

航空会社には異なる顧客階層（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じてパーソナライズされたオファーを各層に提供したいと考えています。ただし、すべての顧客が航空会社のモバイルアプリを使用するわけではなく、会社の Web サイトにログオンしていない顧客もいます。 会社がこれらの顧客に関して持っている識別子は、メンバーシップ ID と電子メールアドレスのみです。

ソーシャルメディアをまたいでターゲットを設定するには、電子メールアドレスを識別子として使用して、顧客データを CRM からAdobe Experience Platformにオンボーディングできます。

次に、関連するメンバーシップ ID や顧客層を含むオフラインデータを使用して、[!DNL Facebook] 宛先を通じてターゲットに設定できる新しいオーディエンスセグメントを構築できます。

## サポートされる ID {#supported-identities}

[!DNL Facebook Custom Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md) の詳細をご覧ください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google 広告 ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストと SHA256 ハッシュ化された電話番号の両方が、Adobe Experience Platformでサポートされています。 「[ID 一致要件 ](#id-matching-requirements-id-matching-requirements)」の手順に従い、プレーンテキストとハッシュ化された電話番号にそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform] |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「[ID 一致要件 ](#id-matching-requirements-id-matching-requirements)」の手順に従い、プレーンテキストとハッシュ化された電子メールアドレスにそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform] |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

## 書き出しタイプ {#export-type}

**セグメントのエクスポート**  — セグメント（オーディエンス）のすべてのメンバーを、Facebookの宛先で使用されている識別子（名前、電話番号など）と共にエクスポートします。

## Facebookアカウントの前提条件 {#facebook-account-prerequisites}

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* お使いの [!DNL Facebook] ユーザーアカウントで、使用する予定の広告アカウントに対する **[!DNL Manage campaigns]** 権限が有効になっている必要があります。
* **Adobe Experience Cloud** ビジネスアカウントは、[!DNL Facebook Ad Account] に広告パートナーとして追加する必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの [Add Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897) を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。[!DNL Adobe Experience Platform] 統合には権限が必要です。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。それには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に移動します。ここで `accountID` は [!DNL Facebook Ad Account ID] です。
   >[!IMPORTANT]
   >
   >[!DNL Facebook Custom Audiences] 利用規約に署名する場合は、Facebook API での認証に使用したのと同じユーザーアカウントを使用してください。

## ID の一致要件 {#id-matching-requirements}

[!DNL Facebook] では、個人を特定できる情報 (PII) を明確に送信しないことが必要です。したがって、[!DNL Facebook] に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された* 識別子をキーオフにできます。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。

## 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

[!DNL Facebook] で電話番号をアクティブにする方法は 2 つあります。

* **生の電話番号の取り込み**:形式の生の電話番号をに取り込むこ [!DNL E.164] とができま [!DNL Platform]す。アクティベーション時に自動的にハッシュ化されます。 このオプションを選択する場合は、生の電話番号を必ず `Phone_E.164` 名前空間に取り込んでください。
* **ハッシュ化された電話番号の取り込み**:に取り込む前に電話番号を事前にハッシュ化できま [!DNL Platform]す。このオプションを選択する場合は、必ずハッシュ化された電話番号を `Phone_SHA256` 名前空間に取り込んでください。

>[!NOTE]
>
>`Phone` 名前空間に取り込まれた電話番号は、[!DNL Facebook] で有効化できません。


## 電子メールのハッシュ要件 {#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platform内で明確に電子メールアドレスを使用し、アクティブ化時に [!DNL Platform] ハッシュ化したりできます。

Experience Platformでの電子メールアドレスの取り込みについて詳しくは、「[ バッチ取り込みの概要 ](/help/ingestion/batch-ingestion/overview.md)」および「[ ストリーミング取り込みの概要 ](/help/ingestion/streaming-ingestion/overview.md)」を参照してください。

電子メールアドレスを自分でハッシュ化する場合は、次の要件に従ってください。

* 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。例：`johndoe@example.com`（`<space>johndoe@example.com<space>` ではない）
* 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   * 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
* ハッシュ化された文字列がすべて小文字であることを確認します。
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
* 文字列にソルトを使用しないでください。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、アクティベート時に [!DNL Platform] によって自動的にハッシュ化されます。
> 属性ソースのデータは自動的にハッシュ化されません。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform]
> **[!UICONTROL 変換を適用]** オプションは、ソースフィールドとして属性を選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![ID マッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## カスタム名前空間の使用 {#custom-namespaces}

`Extern_ID` 名前空間を使用して [!DNL Facebook] にデータを送信する前に、[!DNL Facebook Pixel] を使用して独自の識別子を同期してください。 詳しくは、[Facebook公式ドキュメント ](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers) を参照してください。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

次のビデオでも、[!DNL Facebook] の宛先を設定し、セグメントをアクティブ化する手順を示します。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Experience Platformのユーザーインターフェイスは頻繁に更新され、このビデオの録画以降に変更された可能性があります。 最新の情報については、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) を参照してください。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使 [!DNL Facebook Ad Account ID]いのこの ID は [!DNL Facebook Ads Manager] アカウントで確認できます。 この ID を入力する場合は、必ず `act_` というプレフィックスを付けます。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

**[!UICONTROL セグメントスケジュール]** の手順で、[!DNL Facebook Custom Audiences] にセグメントを送信する際に、[!UICONTROL  オーディエンスの接触チャネル ] を指定する必要があります。

![Facebook Origin of Audience](../../assets/catalog/social/facebook/facebook-origin-audience.png)

### マッピングの例：[!DNL Facebook Custom Audience] でのオーディエンスデータのアクティブ化 {#example-facebook}

[!DNL Facebook Custom Audience] でオーディエンスデータをアクティブ化する際の正しい ID マッピングの例を以下に示します。

ソースフィールドの選択：

* 使用している電子メールアドレスがハッシュ化されていない場合は、`Email` 名前空間をソース ID として選択します。
* [!DNL Facebook] [ 電子メールハッシュ要件 ](#email-hashing-requirements) に従って、データの取り込み時に顧客の電子メールアドレスを [!DNL Platform] にハッシュ化した場合は、`Email_LC_SHA256` 名前空間をソース ID として選択します。
* データがハッシュ化されていない電話番号で構成されている場合は、`PHONE_E.164` 名前空間をソース ID として選択します。 [!DNL Platform] は、要件に従って電話番号をハッシュ化 [!DNL Facebook] します。
* [!DNL Facebook] [ 電話番号のハッシュ要件 ](#phone-number-hashing-requirements) に従って [!DNL Platform] にデータを取り込む際に電話番号をハッシュ化した場合は、`Phone_SHA256` 名前空間をソース ID として選択します。
* データが [!DNL Apple] デバイス ID で構成されている場合は、`IDFA` 名前空間をソース ID として選択します。
* データが [!DNL Android] デバイス ID で構成されている場合は、`GAID` 名前空間をソース ID として選択します。
* データが他のタイプの識別子で構成されている場合は、`Custom` 名前空間をソース ID として選択します。

ターゲットフィールドの選択：

* ソース名前空間が `Email` または `Email_LC_SHA256` の場合、`Email_LC_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `PHONE_E.164` または `Phone_SHA256` の場合、`Phone_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `IDFA` または `GAID` の場合、`IDFA` または `GAID` 名前空間をターゲット ID として選択します。
* ソース名前空間がカスタムの名前空間の場合は、`Extern_ID` 名前空間をターゲット ID として選択します。

>[!IMPORTANT]
>
>ハッシュ化されていない名前空間のデータは、アクティベート時に [!DNL Platform] によって自動的にハッシュ化されます。
> 
>属性ソースのデータは自動的にハッシュ化されません。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform]

![ID マッピング](../../assets/ui/activate-segment-streaming-destinations/mapping-summary.png)

## エクスポートされたデータ {#exported-data}

[!DNL Facebook] の場合、アクティブ化が成功すると、[[!DNL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/) にプログラム的に [!DNL Facebook] カスタムオーディエンスが作成されます。 ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと [!DNL Facebook] の統合では、履歴オーディエンスのバックフィルがサポートされます。 宛先に対してセグメントをアクティブ化すると、すべての過去のセグメント認定が [!DNL Facebook] に送信されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定すると、次のエラーが発生する場合があります。

`{"message":"Facebook Error: Permission error","code":"400 BAD_REQUEST"}`

このエラーは、新しく作成されたアカウントを使用し、[!DNL Facebook] 権限がまだアクティブでない場合に発生します。

[Facebookアカウントの前提条件 ](#facebook-account-prerequisites) の手順に従った後に `400 Bad Request` エラーメッセージが表示された場合は、[!DNL Facebook] 権限を有効にするまで数日お待ちください。
