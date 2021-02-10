---
keywords: facebook拡張子；facebook拡張子；facebook宛先；facebook;instagram;messenger;facebook messenger
title: Facebookの拡張機能
description: ハッシュ化された電子メールに基づいて、オーディエンスターゲティング、パーソナライズ機能および抑制のために Facebook キャンペーンのプロファイルをアクティブ化します。
translation-type: tm+mt
source-git-commit: e13a19640208697665b0a7e0106def33fd1e456d
workflow-type: tm+mt
source-wordcount: '951'
ht-degree: 23%

---


# [!DNL Facebook] 拡張機能

>[!IMPORTANT]
>
>新しい移行先バージョンへのお客様の移行は現在進行中です。 移行が完了するまで、この宛先で使用できるIDは[!UICONTROL EMAIL]と[!UICONTROL EMAIL_LC_SHA_256]のみです。

ハッシュされた電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーション、および抑制のための[!DNL Facebook]キャンペーンのプロファイルをアクティブにします。

[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]、[!DNL Messenger]など、[!DNL Custom Audiences]でサポートされるアプリのファミリーに対するオーディエンスターゲティングには、このリンク先を使用できます。 [!DNL Facebook’s]キャンペーンを実行するアプリの選択範囲が、[!DNL Facebook Ads Manager] の配置レベルで示されます。

![Adobe Experience PlatformUIでのFacebookのリンク先](../../assets/catalog/social/facebook/catalog.png)

## 使用例

[!DNL Facebook]の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの機能を使用して解決できる使用例を2つ示します。

### ユースケース 1

オンライン小売業者は、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと願っています。オンライン小売業者は、自社のCRMからAdobe Experience Platformに電子メールアドレスを取り込み、自社のオフラインデータからセグメントを作成し、[!DNL Facebook]ソーシャルプラットフォームにこれらのセグメントを送信して、広告費用を最適化できます。

### ユースケース 2

航空会社には異なる顧客階層（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じてパーソナライズされたオファーを各層に提供したいと考えています。ただし、航空会社のモバイルアプリを使用していない顧客や、会社のWebサイトにログオンしていない顧客もいます。 会社がこれらの顧客に関して持っている識別子は、メンバーシップ ID と電子メールアドレスのみです。

ソーシャルメディア全体で顧客をターゲットするには、CRMの顧客データをAdobe Experience Platformに乗せ、電子メールアドレスを識別子として使用します。

次に、関連するメンバーシップIDや顧客層を含むオフラインデータを使用して、[!DNL Facebook]宛先を通じてターゲットできる新しいオーディエンスセグメントを作成できます。

## 宛先の詳細{#destination-specs}

### [!DNL Facebook]宛先{#data-governance}のデータ・ガバナンス

>[!IMPORTANT]
>
>[!DNL Facebook]に送信されるデータには、ステッチIDを含めないでください。 お客様は、この義務を守る責任があります。アクティベーション用に選択したセグメントが、マージポリシーでステッチオプションを使用しないようにすることで、義務を守ることができます。 [マージポリシー](/help/profile/ui/merge-policies.md)の詳細を表示します。

### エクスポートの種類{#export-type}

**セグメントのエクスポート**  — セグメント(オーディエンス)のすべてのメンバーを、識別子（名前、電話番号など）と共にエクスポートします。Facebookのリンク先で使用されます。

### Facebookアカウントの前提条件{#facebook-account-prerequisites}

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

- [!DNL Facebook]ユーザーアカウントで、使用する広告アカウントに対して&#x200B;**[!DNL Manage campaigns]**&#x200B;権限を有効にする必要があります。
- **Adobe Experience Cloud**&#x200B;のビジネスアカウントは、貴社の[!DNL Facebook Ad Account]に広告パートナーとして追加する必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの[追加Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897)を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Experience Platform] 統合に必要です。
- [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

### IDの一致要件{#id-matching-requirements}

[!DNL Facebook] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL Facebook]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

Adobe Experience Platformに取り込むIDのタイプに応じて、対応する要件を満たす必要があります。

#### 電話番号のハッシュ要件{#phone-number-hashing-requirements}

[!DNL Facebook]で電話番号をアクティブにする方法は2つあります。

- **生の電話番号を取り込む**:生の電話番号を [!DNL E.164] 形式で取り込んで、アクティベーション時に自動的にハッシュ化 [!DNL Platform]されます。このオプションを選択する場合は、必ず生の電話番号を`Phone_E.164`名前空間に取り込むようにしてください。
- **ハッシュ化された電話番号を取り込む**:に取り込む前に電話番号を事前にハッシュ化でき [!DNL Platform]ます。このオプションを選択する場合は、ハッシュ化された電話番号を必ず`Phone_SHA256`名前空間に取り込むようにしてください。

>[!NOTE]
>
>`Phone`名前空間に取り込まれた電話番号は、[!DNL Facebook]では有効にできません。


#### Eメールハッシュ要件{#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュするか、Experience Platform内で電子メールアドレスを明確に扱ってアクティベーション上でアルゴリズムハッシュするかを選択できます。

Experience Platformでの電子メールアドレスの取り込みについては、[batch ingestion overview](/help/ingestion/batch-ingestion/overview.md)および[steaming ingestion overview](/help/ingestion/streaming-ingestion/overview.md)を参照してください。

電子メールアドレスを自分でハッシュする場合は、次の要件を満たしていることを確認してください。

- 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。例：`johndoe@example.com`（`<space>johndoe@example.com<space>` ではない）
- 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   - 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
- ハッシュ化された文字列がすべて小文字であることを確認します。
   - 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
- 文字列にソルトを使用しないでください。

非ハッシュ化された名前空間のデータは、アクティベーション時に[!DNL Platform]によって自動的にハッシュされます。

属性ソースデータは自動的にハッシュされません。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティベーション上のデータを自動的にハッシュ化します。
[!DNL Platform]![IDマッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

#### カスタム名前空間{#custom-namespaces}の使用

`Extern_ID`名前空間を使用して[!DNL Facebook]にデータを送信する前に、[!DNL Facebook Pixel]を使用して自分の識別子を同期させてください。 詳しくは、[公式ドキュメント](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers)を参照してください。

## 宛先に接続 {#connect-destination}

[!DNL Facebook]宛先に接続するには、[ソーシャルネットワーク宛先認証ワークフロー](./workflow.md)を参照してください。

## セグメントを[!DNL Facebook] {#activate-segments}にアクティブ化

[!DNL Facebook]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。

**[!UICONTROL セグメントスケジュール]**&#x200B;の手順では、[!DNL Facebook Custom Audiences]にセグメントを送信する際に、[!UICONTROL オーディエンス]の接触チャネルを指定する必要があります。

![Facebookでのオーディエンスの接触チャネル](../../assets/catalog/social/facebook/facebook-origin-audience.png)

## エクスポートされたデータ{#exported-data}

[!DNL Facebook]の場合、アクティベーションが成功したときは、[[!DNL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/)にプログラム的に[!DNL Facebook]カスタムオーディエンスが作成されます。 ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと[!DNL Facebook]の統合は、履歴オーディエンスのバックフィルをサポートします。 宛先に対してセグメントをアクティブ化すると、すべての過去のセグメント資格情報が[!DNL Facebook]に送信されます。