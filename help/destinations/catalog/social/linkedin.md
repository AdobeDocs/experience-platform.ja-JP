---
keywords: linkedin 接続；linkedin 接続；linkedin 宛先；linkedin;linkedin;
title: Linkedin Matched Audiences 接続
description: ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのLinkedInキャンペーンのプロファイルをアクティブ化します。
exl-id: 74c233e9-161a-4e4a-98ef-038a031feff0
source-git-commit: c5d2427635d90f3a9551e2a395d01d664005e8bc
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 11%

---

# [!DNL LinkedIn Matched Audiences] 接続

## 概要 {#overview}

のプロファイルをアクティブ化 [!DNL LinkedIn] ハッシュ化された電子メールとモバイル ID に基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのキャンペーンを作成します。

![Adobe Experience Platform UI でのlinkedInの宛先](../../assets/catalog/social/linkedin/catalog.png)

## ユースケース

を使用する方法とタイミングをより深く理解するために [!DNL LinkedIn Matched Audiences] の宛先に関しては、Adobe Experience Platformのお客様がこの機能を使用して解決できる使用例を以下に示します。

ソフトウェア会社が会議を開催し、参加者と連絡を取り合い、参加者の出席状況に基づいてパーソナライズされたオファーを表示したいと考えています。 会社は、独自の電子メールアドレスやモバイルデバイス ID を取り込むことができます [!DNL CRM] Adobe Experience Platformに その後、独自のオフラインデータからセグメントを作成し、それらのセグメントを [!DNL LinkedIn] ソーシャルプラットフォームを使用して、広告費用を最適化します。

## サポートされる ID {#supported-identities}

[!DNL LinkedIn Matched Audiences] では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「 [ID 一致要件](#id-matching-requirements-id-matching-requirements) を参照し、適切な名前空間を、それぞれプレーンテキストとハッシュ化された電子メールに使用してください。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。 |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーを、 [!DNL LinkedIn Matched Audiences] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## linkedInアカウントの前提条件 {#LinkedIn-account-prerequisites}

使用する前に [!UICONTROL linkedIn Matched Audience] 宛先、 [!DNL LinkedIn Campaign Manager] アカウントに [!DNL Creative Manager] 権限レベル以上。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

## ID 一致要件 {#id-matching-requirements}

[!DNL LinkedIn Matched Audiences] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL LinkedIn Matched Audiences] キーオフできる *ハッシュ* 識別子（電子メールアドレスやモバイルデバイス ID など）。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。

## 電子メールのハッシュ要件 {#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、電子メールアドレスをExperience Platformで明確に使用したり、 [!DNL Platform] 有効化時にハッシュ化します。

E メールアドレスの取り込みについて詳しくは、Experience Platform [バッチ取得の概要](/help/ingestion/batch-ingestion/overview.md) そして [ストリーミング取得の概要](/help/ingestion/streaming-ingestion/overview.md).

電子メールアドレスを自分でハッシュ化する場合は、次の要件に従ってください。

* 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。 例： `johndoe@example.com`ではない `<space>johndoe@example.com<space>`;
* 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   * 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
* ハッシュ化された文字列がすべて小文字であることを確認します。
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
* 文字列にソルトを使用しないでください。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます。 [!DNL Platform] 有効化時。
> 属性ソースデータは自動的にハッシュ化されません。
> 
> 期間 [ID マッピング](../../ui/activate-segment-streaming-destinations.md#mapping) ステップ、ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。
> 
> この **[!UICONTROL 変換を適用]** オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![ID マッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

次のビデオでは、 [!DNL LinkedIn Matched Audiences] の宛先に移動して、セグメントをアクティブ化します。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Experience Platformのユーザーインターフェイスは頻繁に更新され、このビデオの録画以降に変更された可能性があります。 最新の情報については、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先を将来認識するための名前。
* **[!UICONTROL 説明]**:この宛先を将来識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:あなたの [!DNL LinkedIn Campaign Manager Account ID]. この ID は、 [!DNL LinkedIn Campaign Manager] アカウント

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## 書き出されたデータ {#exported-data}

アクティベーションが成功した場合、 [!DNL LinkedIn] カスタムオーディエンスは、 [[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login). ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと [!DNL LinkedIn Matched Audiences] は、履歴オーディエンスのバックフィルをサポートします。 すべての過去のセグメント認定がに送信されます。 [!DNL LinkedIn] 宛先へのセグメントをアクティブ化した場合。
