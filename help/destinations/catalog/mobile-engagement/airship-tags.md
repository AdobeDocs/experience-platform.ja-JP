---
keywords: 飛行船タグ；飛行船の宛先
title: 飛行船タグの接続
description: Airship 内でターゲティングするために、Adobeのオーディエンスデータをオーディエンスタグとして Airship にシームレスに渡します。
exl-id: 84cf5504-f0b5-48d8-8da1-ff91ee1dc171
source-git-commit: c5d2427635d90f3a9551e2a395d01d664005e8bc
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 1%

---

# [!DNL Airship Tags] 接続 {#airship-tags-destination}

## 概要

[!DNL Airship] は、顧客ライフサイクルの各段階で、意味のあるパーソナライズされたオムニチャネルメッセージをユーザーに届けるのに役立つ、主要な顧客エンゲージメントプラットフォームです。

この統合により、Adobe Experience Platformのセグメントデータが [!DNL Airship] as [タグ](https://docs.airship.com/guides/audience/tags/) （ターゲティングまたはトリガーの場合）

詳しくは、以下を参照してください。 [!DNL Airship]を参照し、 [航空船ドキュメント](https://docs.airship.com).


>[!TIP]
>
>このドキュメントページは、 [!DNL Airship] チーム。 お問い合わせや更新のご依頼は、直接 [support.airship.com](https://support.airship.com/).

## 前提条件

にAdobe Experience Platformセグメントを送信する前に [!DNL Airship]を使用する場合は、次の操作を行う必要があります。

* タググループを [!DNL Airship] プロジェクト。
* 認証用の bearer トークンを生成します。

>[!TIP]
> 
>の作成 [!DNL Airship] 経由のアカウント [この登録リンク](https://go.airship.eu/accounts/register/plan/starter/) まだお持ちでない場合は、

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | Airship Tags の宛先で使用される識別子を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## タググループ

Adobe Experience Platform のセグメントの概念は、 [タグ](https://docs.airship.com/guides/audience/tags/) 飛行船では、実装のわずかな違いがあります。 この統合により、ユーザーの [Experience Platformセグメントのメンバーシップ](../../../xdm/field-groups/profile/segmentation.md) 存在または存在しない [!DNL Airship] タグを使用します。 例えば、Platform セグメントの場合、 `xdm:status` 変更 `realized`に値を指定しない場合、タグは [!DNL Airship] このプロファイルがマッピングされているチャネルまたは名前付きユーザー。 この `xdm:status` 変更 `exited`の場合、タグは削除されます。

この統合を有効にするには、 *タググループ* in [!DNL Airship] 名前付き `adobe-segments`.

>[!IMPORTANT]
>
>新しいタググループを作成する際 **チェックしない** 「[!DNL Allow these tags to be set only from your server]&quot;. これをおこなうと、Adobeタグの統合が失敗します。

詳しくは、 [タググループを管理](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups) タググループの作成手順を参照してください。

## bearer トークンを生成

に移動します。 **[!UICONTROL 設定]** &quot; **[!UICONTROL API と統合]** 内 [飛行船ダッシュボード](https://go.airship.com) を選択し、 **[!UICONTROL トークン]** をクリックします。

クリック **[!UICONTROL トークンを作成]**.

トークンのわかりやすい名前 ( 例：「Adobeタグの宛先」) を指定し、役割に「すべてのアクセス」を選択します。

クリック **[!UICONTROL トークンを作成]** 詳細を機密として保存します。

## ユースケース

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Airship Tags] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1

小売業者やエンターテイメントプラットフォームは、ロイヤルティ顧客のユーザープロファイルを作成し、それらのセグメントを [!DNL Airship] （モバイルキャンペーンでのメッセージターゲティング用）

### 使用例#2

トリガーがAdobe Experience Platform内の特定のセグメントにフォールアウトした場合に、1 対 1 のメッセージをリアルタイムで送信できます。

例えば、ある小売業者は、Platform でジーンズのブランド固有のセグメントを設定します。 あの小売業者は、誰かが特定のブランドにジーンズの好みを設定したらすぐに、モバイルメッセージをトリガーできるようになりました。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL Bearer トークン]**:から生成した bearer トークン [!DNL Airship] ダッシュボード。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**:米国または EU のデータセンターを選択します（どちらかに応じて選択します）。 [!DNL Airship] データセンターがこの宛先に適用されます。


## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

[!DNL Airship] タグは、デバイスインスタンス (iPhoneなど ) を表すチャネルに対して設定できます。また、すべてのユーザーのデバイスを、顧客 ID などの共通の識別子にマッピングする名前付きユーザーに対して設定することもできます。 スキーマにプレーンテキスト（ハッシュ化されていない）の電子メールアドレスがプライマリ ID である場合は、 **[!UICONTROL ソース属性]** と [!DNL Airship] の下の右側の列にユーザーという名前が付けられました **[!UICONTROL ターゲット ID]**、以下に示すように。

![特定ユーザーマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option-2.png)

チャネルにマッピングする必要がある識別子（デバイスなど）について、ソースに基づいて適切なチャネルにマッピングします。 以下の画像は、Google Advertising ID を [!DNL Airship] Android チャネル。

![飛行船タグに接続](../../assets/catalog/mobile-engagement/airship-tags/select-source-identity.png)
![飛行船タグに接続](../../assets/catalog/mobile-engagement/airship-tags/select-target-identity.png)
![チャネルマッピング](../../assets/catalog/mobile-engagement/airship-tags/mapping-option.png)

## データの使用とガバナンス {#data-usage-governance}

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] では、データガバナンスを強制します。詳しくは、 [データガバナンスの概要](../../../data-governance/home.md).
