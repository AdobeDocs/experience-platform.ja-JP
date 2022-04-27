---
title: ドキュメントのセルフサービステンプレート//を宛先の名前に置き換えます。
description: このテンプレートを使用して、Adobe Experience Platformカタログの宛先に関する公開ドキュメントを作成します。//概要セクションの段落に置き換えます
exl-id: 99700474-8bf6-4176-acc1-38814e17c995
source-git-commit: 729c0724c7af88bb69c9d68a45d58c3575c90828
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 6%

---

# 宛先接続 {#your-destination}

*このテンプレートの操作中に、斜体のすべての段落を置き換えるか削除します（この段落から始まります）。*

*まず、ページ上部のメタデータ（タイトルと説明）を更新します。 このページの UICONTROL のすべてのインスタンスを無視してください。 これは、機械翻訳プロセスがページをサポートする複数の言語に正しく翻訳するのに役立つタグです。 提出後、ドキュメントにタグを追加します。*

>[!IMPORTANT]
>
>* このテンプレートのすべてのセクションを、テンプレートで説明されている順序で入力します。
>* このテンプレートは、パートナーからのフィードバックに基づいて、まれに更新されます。 宛先のオーサリングドキュメントを開始する前に、 [テンプレートの最新バージョン](/help/destinations/destination-sdk/docs-framework/assets/yourdestination-template.zip).


## 概要 {#overview}

*顧客に提供する価値を含め、会社の概要を短く示します。 詳しくは、製品ドキュメントのホームページへのリンクを含めてください。*

>[!IMPORTANT]
>
>このドキュメントページは、 *宛先* チーム。 お問い合わせや更新のご依頼は、直接 *更新用にアクセスできるリンクや電子メールアドレスを挿入します。例： `support@YourDestination.com`.*

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 *宛先* の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1 {#use-case-1}

*モバイルメッセージプラットフォームの場合：*

*レンタルや販売用プラットフォームは、顧客の Android やiOSのデバイスにモバイル通知をプッシュして、以前にレンタルを検索した地域に更新済みの 100 件のリストがあることを知らせたいと考えています。*

### 使用例#2 {#use-case-2}

*ソーシャルネットワークプラットフォームの場合：*

*スポーツアパレルブランドは、ソーシャルメディアアカウントを通じて既存の顧客にリーチしたいと考えています。 アパレルブランドは、独自の CRM からAdobe Experience Platformに電子メールアドレスを取り込み、独自のオフラインデータからセグメントを作成し、これらのセグメントを YourDestination に送信して、顧客のソーシャルメディアフィードに広告を表示できます。*

## 前提条件 {#prerequisites}

*Adobe Experience Platformユーザーインターフェイスで宛先の設定を開始する前に顧客が認識しておく必要がある事項に関する情報を、この節に追加します。 次のような場合が考えられます。*

* *許可リスト*
* *電子メールハッシュの要件*
* *お客様側のアカウントの詳細*
* *プラットフォームに接続するための API キーの取得方法*

*お客様に役立つ場合は、関連するドキュメントにリンクアウトできます。*

## サポートされる ID {#supported-identities}

*宛先でサポートされている ID に関する情報をこの節に追加します。 テーブルには、いくつかの標準値が事前入力されています。 宛先に適用しない値と、事前入力されていない値を削除します。*

*宛先* では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、次のエイリアスからも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 次のドキュメントを参照してください： [ECID](/help/identity-service/ecid.md) を参照してください。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストと SHA256 ハッシュ化された電話番号の両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

*テーブルでは、目的の宛先に対応する行のみを残します。 [ 書き出し ] タイプに 1 行、[ 書き出し頻度 ] に 1 行が必要です。 宛先に適用しない値を削除します。*

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーを、 *宛先* 宛先。 |
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

*宛先の認証時に入力する必要があるフィールドを追加します。 これらのフィールドは宛先固有で、Destination SDKの設定に応じて異なります。 宛先のフィールドが次に示すフィールドと異なる場合があります。 また、以下のサンプルのスクリーンショットに似たスクリーンショットを含めてください。*

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

![宛先への認証方法を示すサンプルスクリーンショット](/help/destinations/destination-sdk/docs-framework/assets/authenticate-destination.png)

* **[!UICONTROL Bearer トークン]**:宛先への認証をおこなうために bearer トークンを入力します。

### 宛先の詳細を入力 {#destination-details}

*新しい宛先を設定する際に顧客が入力する必要があるフィールドを追加します。 これらのフィールドは宛先固有で、Destination SDKの設定に応じて異なります。 宛先のフィールドが次に示すフィールドと異なる場合があります。 また、以下のサンプルのスクリーンショットに似たスクリーンショットを含めてください。*

宛先の詳細を設定するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 次へ]**.

![宛先の詳細を入力する方法を示すサンプルスクリーンショット](/help/destinations/destination-sdk/docs-framework/assets/configure-destination-details.png)

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使いの *宛先* アカウント ID。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en) を参照してください。

## エクスポートされたデータ/データエクスポートの検証 {#exported-data}

*宛先へのデータの書き出し方法に関する段落を追加します。 これにより、顧客が宛先と正しく統合されていることを確認できます。 例えば、以下のような JSON のサンプルを指定できます。 または、宛先プラットフォームでのセグメントの生成を顧客が期待する方法を示すスクリーンショットと情報を、宛先のインターフェイスから提供できます。*

```
{
  "person": {
    "email": "yourstruly@adobe.com"
  },
  "segmentMembership": {
    "ups": {
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93": {
        "lastQualificationTime": "2020-05-25T21:24:39Z",
        "status": "exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae": {
        "lastQualificationTime": "2020-05-25T23:37:33Z",
        "status": "existing"
      }
    }
  },
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

*お客様が成功するために重要と考える製品ドキュメントやその他のリソースへのリンクを提供できます。*