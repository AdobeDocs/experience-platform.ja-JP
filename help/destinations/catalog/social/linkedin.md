---
keywords: linkedin接続；linkedin接続；linkedinの宛先；linkedin;
title: Linkedinが一致したオーディエンス接続
description: ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのLinkedInキャンペーンのプロファイルをアクティブ化します。
exl-id: 74c233e9-161a-4e4a-98ef-038a031feff0
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 10%

---

# [!DNL LinkedIn Matched Audiences] 接続

## 概要 {#overview}

ハッシュ化された電子メールとモバイルIDに基づいて、オーディエンスのターゲティング、パーソナライゼーション、抑制のための[!DNL LinkedIn]キャンペーンのプロファイルをアクティブ化します。

![Adobe Experience Platform UIでのlinkedInの宛先](../../assets/catalog/social/linkedin/catalog.png)

## ユースケース

[!DNL LinkedIn Matched Audiences]の宛先を使用する方法とタイミングをより深く理解できるように、Adobe Experience Platformのお客様がこの機能を使用して解決できる使用例を以下に示します。

ソフトウェア会社が会議を開催し、参加者と連絡を取り合い、参加者の出席状況に基づいてパーソナライズされたオファーを表示したいと考えています。 会社は、電子メールアドレスやモバイルデバイスIDを自分の[!DNL CRM]からAdobe Experience Platformに取り込むことができます。 その後、独自のオフラインデータからセグメントを作成し、これらのセグメントを[!DNL LinkedIn]ソーシャルプラットフォームに送信して、広告費用を最適化できます。

## サポートされるID {#supported-identities}

[!DNL LinkedIn Matched Audiences] では、以下の表で説明するIDのアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を説明します。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | Google広告ID | ソースIDがGAID名前空間の場合は、このターゲットIDを選択します。 |
| IDFA | Apple の広告主 ID | ソースIDがIDFA名前空間の場合は、このターゲットIDを選択します。 |
| email_lc_sha256 | SHA256アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストとSHA256ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「[ID一致要件](#id-matching-requirements-id-matching-requirements)」の手順に従い、プレーンテキストとハッシュ化された電子メールに対して、それぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に[!DNL Platform]でデータを自動的にハッシュ化します。 |


## 書き出しタイプ {#export-type}

**セグメントの書き出し**  — 宛先で使用されている識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出 [!DNL LinkedIn Matched Audiences] します。

## linkedInアカウントの前提条件 {#LinkedIn-account-prerequisites}

[!UICONTROL LinkedInの一致するオーディエンス]の宛先を使用する前に、[!DNL LinkedIn Campaign Manager]アカウントに[!DNL Creative Manager]権限レベル以上があることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

## ID一致の要件 {#id-matching-requirements}

[!DNL LinkedIn Matched Audiences] では、個人を特定できる情報(PII)を明確に送信しないことが求められます。したがって、[!DNL LinkedIn Matched Audiences]に対してアクティブ化されたオーディエンスは、電子メールアドレスやモバイルデバイスIDなど、*ハッシュ化された*&#x200B;識別子をキーにすることができます。

Adobe Experience Platformに取り込むIDのタイプに応じて、対応する要件を満たす必要があります。

## 電子メールのハッシュ要件 {#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platform内で明確に電子メールアドレスを使用し、アクティブ化時に[!DNL Platform]ハッシュ化したりできます。

Experience PlatformでのEメールアドレスの取り込みについて詳しくは、「[バッチ取り込みの概要](/help/ingestion/batch-ingestion/overview.md)」および「[ストリーミング取り込みの概要](/help/ingestion/streaming-ingestion/overview.md)」を参照してください。

自分で電子メールアドレスをハッシュ化する場合は、次の要件に従ってください。

* 電子メール文字列の先頭と末尾の空白文字をすべてトリミングします。 例：`johndoe@example.com`（`<space>johndoe@example.com<space>`ではない）;
* 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   * 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
* ハッシュ化された文字列がすべて小文字であることを確認します。
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
* 文字列にソルトを使用しないでください。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、アクティブ化時に[!DNL Platform]によって自動的にハッシュ化されます。
> 属性ソースのデータは自動的にハッシュ化されません。
> 
> [IDマッピング](../../ui/activate-segment-streaming-destinations.md#mapping)の手順で、ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に[!DNL Platform]でデータを自動的にハッシュ化します。
> 
> 「**[!UICONTROL 変換を適用]**」オプションは、ソースフィールドとして属性を選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![IDマッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

次のビデオでも、[!DNL LinkedIn Matched Audiences]の宛先を設定し、セグメントをアクティブ化する手順を示します。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Experience Platformのユーザーインターフェイスは頻繁に更新され、このビデオの録画以降に変更された可能性があります。 最新の情報については、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)を参照してください。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:この宛先を将来識別するのに役立つ説明。
* **[!UICONTROL アカウントID]**:お使 [!DNL LinkedIn Campaign Manager Account ID]いのこのIDは[!DNL LinkedIn Campaign Manager]アカウントで確認できます。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## エクスポートされたデータ {#exported-data}

アクティブ化が成功した場合、[!DNL LinkedIn]カスタムオーディエンスが[[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login)にプログラムで作成されます。 ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと[!DNL LinkedIn Matched Audiences]の統合では、履歴オーディエンスのバックフィルがサポートされます。 宛先に対してセグメントをアクティブ化すると、すべての過去のセグメント認定が[!DNL LinkedIn]に送信されます。
