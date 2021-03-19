---
keywords: linkedin接続；linkedin接続；linkedin接続；linkedinの宛先；linkedin;
title: Linkedinがオーディエンス接続に一致しました
description: ハッシュされた電子メールに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制に使用するLinkedInキャンペーンのプロファイルをアクティブにします。
translation-type: tm+mt
source-git-commit: fd95357f3e3533fe6b7b9752798dd99eb1cc0eb5
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 12%

---


# [!DNL LinkedIn Matched Audiences] connection

ハッシュされた電子メールとモバイルIDに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制のための[!DNL LinkedIn]キャンペーンのプロファイルをアクティブにします。

![Adobe Experience PlatformUIのLinkedInリンク先](../../assets/catalog/social/linkedin/catalog.png)

## 使用例

[!DNL LinkedIn Matched Audiences]の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの機能を使って解決できる使用例を以下に示します。

ソフトウェア会社は会議を組織し、参加者と連絡を取り合い、会議の出席状況に基づいてパーソナライズされたオファーを表示したいと考えています。 会社は、自分の[!DNL CRM]から電子メールアドレスやモバイルデバイスIDをAdobe Experience Platformに取り込むことができます。 その後、独自のオフラインデータからセグメントを作成し、これらのセグメントを[!DNL LinkedIn]ソーシャルプラットフォームに送信して、広告費用を最適化できます。

## 宛先の詳細{#destination-specs}

[!DNL LinkedIn Matched Audiences] は、次のIDのアクティベーションをサポートします。ハッシュ化された電子メール、 [!DNL GAID]および [!DNL IDFA]。

### サポートされるID{#supported-identities}

[!DNL LinkedIn Matched Audiences] は、次の表に示すIDのアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を表示します。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | Google広告ID | ソースIDがGAID名前空間の場合は、このターゲットIDを選択します。 |
| IDFA | Apple の広告主 ID | ソースIDがIDFA名前空間の場合は、このターゲットIDを選択します。 |
| email_lc_sha256 | SHA256アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストとSHA256ハッシュの電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「[ID matching requirements](#id-matching-requirements-id-matching-requirements)」の説明に従い、プレーンテキストとハッシュ電子メールにそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティベーション上のデータを自動的にハッシュ化します。[!DNL Platform] |


### エクスポートの種類{#export-type}

**セグメントエクスポート**  — セグメント(オーディエンス)のすべてのメンバーを、 [!DNL LinkedIn Matched Audiences] 宛先で使用されている識別子（名前、電話番号など）と共にエクスポートします。

### LinkedInアカウントの前提条件{#LinkedIn-account-prerequisites}

[!UICONTROL 一致したオーディエンス]の宛先にLinkedInを使用する前に、[!DNL LinkedIn Campaign Manager]アカウントに[!DNL Creative Manager]権限レベル以上があることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

### IDの一致要件{#id-matching-requirements}

[!DNL LinkedIn Matched Audiences] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL LinkedIn Matched Audiences]に対してアクティブ化されたオーディエンスは、電子メールアドレスやモバイルデバイスIDなど、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

Adobe Experience Platformに取り込むIDのタイプに応じて、対応する要件に従う必要があります。

#### Eメールハッシュ要件{#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platform内で明確な電子メールアドレスを使用して、アクティベーション上で[!DNL Platform]ハッシュ化したりできます。

Experience Platformでの電子メールアドレスの取り込みについて詳しくは、[バッチインジェストの概要](/help/ingestion/batch-ingestion/overview.md)および[ストリーミングインジェストの概要](/help/ingestion/streaming-ingestion/overview.md)を参照してください。

電子メールアドレスを自分でハッシュする場合は、次の要件を満たしていることを確認してください。

- 電子メール文字列の先頭と末尾の空白文字をすべて削除します。 次に例を示します。`johndoe@example.com`、`<space>johndoe@example.com<space>`ではありません；
- 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   - 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
- ハッシュ化された文字列がすべて小文字であることを確認します。
   - 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
- 文字列にソルトを使用しないでください。

>[!NOTE]
>
>非ハッシュ化された名前空間のデータは、アクティベーション時に[!DNL Platform]によって自動的にハッシュされます。
> 属性ソースデータは自動的にハッシュされません。
> 
> [IDマッピング](../../ui/activate-destinations.md#identity-mapping)の手順中に、ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL 変換]**&#x200B;を適用オプションをオンにして、アクティベーション上のデータを自動的にハッシュします。[!DNL Platform]
> 
> 「**[!UICONTROL 変換を適用]**」オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![IDマッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## 宛先に接続 {#connect-destination}

[!DNL LinkedIn Matched Audiences]宛先に接続するには、[ソーシャルネットワーク宛先認証ワークフロー](./workflow.md)を参照してください。

## セグメントを[!DNL LinkedIn Matched Audiences] {#activate-segments}にアクティブ化

[!DNL LinkedIn Matched Audiences]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。

## エクスポートされたデータ{#exported-data}

アクティベーションが成功した場合は、[!DNL LinkedIn]カスタムオーディエンスが[[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login)にプログラム的に作成されます。 ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと[!DNL LinkedIn Matched Audiences]の統合は、履歴オーディエンスのバックフィルをサポートします。 宛先に対してセグメントをアクティブ化すると、すべての過去のセグメント資格情報が[!DNL LinkedIn]に送信されます。