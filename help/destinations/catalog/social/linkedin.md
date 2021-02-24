---
keywords: linkedin接続；linkedin接続；linkedin接続；linkedinの宛先；linkedin;
title: Linkedinがオーディエンス接続に一致しました
description: ハッシュされた電子メールに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制に使用するLinkedInキャンペーンのプロファイルをアクティブにします。
translation-type: tm+mt
source-git-commit: db2e5d51a5ed07b91997df8a566272c86a7c1708
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 14%

---


# [!DNL LinkedIn Matched Audience] connection

ハッシュされた電子メールとモバイルIDに基づいて、オーディエンスのターゲット設定、パーソナライゼーション、および抑制のための[!DNL LinkedIn]キャンペーンのプロファイルをアクティブにします。

![Adobe Experience PlatformUIのLinkedInリンク先](../../assets/catalog/social/linkedin/catalog.png)

## 使用例

[!DNL LinkedIn Matched Audience]の宛先を使う方法と時期をよりよく理解するために、Adobe Experience Platformのお客様はこの機能を使って解決できる使用例を以下に示します。

ソフトウェア会社は会議を組織し、参加者と連絡を取り合い、会議の出席状況に基づいてパーソナライズされたオファーを表示したいと考えています。 会社は、自分の[!DNL CRM]からAdobe Experience Platformに電子メールアドレスやモバイルデバイスIDを取り込み、自分のオフラインデータからセグメントを作成し、[!DNL LinkedIn]ソーシャルプラットフォームに送信して、広告費用を最適化できます。

## 宛先の詳細{#destination-specs}

[!DNL LinkedIn Matched Audience] は、次のIDのアクティベーションをサポートします。ハッシュ化された電子メール、 [!DNL GAID]および [!DNL IDFA]。

### エクスポートの種類{#export-type}

**セグメントのエクスポート**  — セグメント(オーディエンス)のすべてのメンバーを、識別子（名前、電話番号など）と共にエクスポートします。[!DNL LinkedIn Matched Audience]宛先で使用されます。

### LinkedInアカウントの前提条件{#LinkedIn-account-prerequisites}

[!UICONTROL 一致したオーディエンス]の宛先にLinkedInを使用する前に、[!DNL LinkedIn Campaign Manager]アカウントに[!DNL Creative Manager]権限レベル以上があることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

### IDの一致要件{#id-matching-requirements}

[!DNL LinkedIn Matched Audience] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL LinkedIn Matched Audience]に対してアクティブ化されたオーディエンスは、電子メールアドレスやモバイルデバイスIDなど、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

Adobe Experience Platformに取り込むIDのタイプに応じて、対応する要件を満たす必要があります。

#### Eメールハッシュ要件{#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュするか、Experience Platform内で電子メールアドレスを明確に扱ってアクティベーション上でアルゴリズムハッシュするかを選択できます。

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

[!DNL LinkedIn Matched Audience]宛先に接続するには、[ソーシャルネットワーク宛先認証ワークフロー](./workflow.md)を参照してください。

## セグメントを[!DNL LinkedIn Matched Audience] {#activate-segments}にアクティブ化

[!DNL LinkedIn Matched Audience]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。

## エクスポートされたデータ{#exported-data}

アクティベーションが成功した場合は、[!DNL LinkedIn]カスタムオーディエンスが[[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login)にプログラム的に作成されます。 ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと[!DNL LinkedIn Matched Audience]の統合は、履歴オーディエンスのバックフィルをサポートします。 宛先に対してセグメントをアクティブ化すると、すべての過去のセグメント資格情報が[!DNL LinkedIn]に送信されます。