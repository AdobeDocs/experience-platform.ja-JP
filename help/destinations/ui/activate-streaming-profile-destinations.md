---
keywords: プロファイルの宛先のアクティブ化、宛先のアクティブ化、データのアクティブ化、電子メールのマーケティング対象をアクティブにします。クラウドの保存先のアクティブ化
title: ストリーミングプロファイルの書き出し先に対する対象ユーザーデータのアクティブ化
type: Tutorial
seo-title: Activate audience data to streaming profile export destinations
description: Adobe エクスペリエンスプラットフォームにある対象ユーザーデータをアクティブにする方法については、ストリーミングプロファイルに基づいた宛先にセグメントを送信します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by sending segments to streaming profile-based destinations.
exl-id: bc0f781e-60de-44a5-93cb-06b4a3148591
source-git-commit: 2b1cde9fc913be4d3bea71e7d56e0e5fe265a6be
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 7%

---

# ストリーミングプロファイルの書き出し先に対する対象ユーザーデータのアクティブ化

## 概要 {#overview}

この記事では、Amazon Kinesis など、Adobe エクスペリエンスプラットフォームのストリーミングプロファイルに基づいた宛先において、対象ユーザーデータをアクティブにするために必要なワークフローについて説明しています。

## 前提条件 {#prerequisites}

データを転送先にアクティブにするには、宛先に正しく接続されている必要があり [ ](./connect-destination.md) ます。 まだ作成していない場合は、宛先カタログに移動し [ ](../catalog/overview.md) て、サポートされている出力先を参照し、使用する宛先を設定します。

## 宛先を選択します。 {#select-destination}

1. 「接続 > 宛先」に移動し、「 **** **[!UICONTROL カタログ」タブを選択し]** ます。

   ![「送信先カタログ」タブ](../assets/ui/activate-streaming-profile-destinations/catalog-tab.png)

1. **** 次の図に示すように、セグメントのライセンス認証を行う宛先に対応するカードの「セグメントをアクティブにする」を選択します。

   ![「セグメントをアクティブ化」ボタン](../assets/ui/activate-streaming-profile-destinations/activate-segments-button.png)

1. 使用する接続先を選択し、「次へ」を選択し **** ます。

   ![移動先を選択](../assets/ui/activate-streaming-profile-destinations/select-destination.png)

1. 次のセクションに移動し [ て、セグメントを選択し ](#select-segments) ます。

## セグメントを選択します。 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先にアクティブ化するセグメントを選択し、「次へ」を選択し **** ます。

![セグメントの選択](../assets/ui/activate-streaming-profile-destinations/select-segments.png)

## プロファイル属性の選択 {#select-attributes}

目的の宛先に送信するプロファイル属性を選択します。

>[!NOTE]
>
> アドビシステムズ社では、選択したアイテムに、スキーマでよく使用される4つの属性を設定します。、、、、の各属性について学習 `person.name.firstName` `person.name.lastName` `personalEmail.address` `segmentMembership.status` します。

ファイルの書き出し方法は、「」が選択されているかどうかによって異なり `segmentMembership.status` ます。
* この `segmentMembership.status` フィールドが選択されている場合、 **[!UICONTROL 最初のフルスナップショットにはアクティブなメンバーが、以降の増分書きについてはアクティブなメンバーと期限切れのメンバーが書き出さ]** **** **** れます。
* この `segmentMembership.status` フィールドが選択されていない場合、 **[!UICONTROL 最初の]** フルスナップショットとその後の増分エクスポートにはアクティブなメンバーのみが書き出されます。

![推奨属性](../assets/ui/activate-streaming-profile-destinations/attributes-default.png)

1. 「 **[!UICONTROL 属性を選択」]** ページで、「新規フィールドを追加」を選択し **** ます。

   ![新しいマッピングの追加](../assets/ui/activate-streaming-profile-destinations/add-new-field.png)

1. スキーマフィールドエントリの右側にある矢印をクリックし **** ます。

   ![ソースフィールドを選択](../assets/ui/activate-streaming-profile-destinations/select-schema-field.png)

1. 「 **[!UICONTROL フィールドを選択」]** ページで、宛先に送信する XDM 属性を選択し、「選択」を選択し **** ます。

   ![ソースフィールドを選択ページ](../assets/ui/activate-streaming-profile-destinations/target-field-page.png)


1. マッピングをさらに追加するには、手順 1 ~ 3 を繰り返してから、「次へ」を選択し **** ます。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platform がデータ使用ポリシー違反を確認します。 次の例は、ポリシーに違反する場合を示しています。 この違反が解決されるまで、セグメントアクティブ化ワークフローを完了することはできません。 ポリシー違反を解決する方法については、「 [ ](../../rtcdp/privacy/data-governance-overview.md#enforcement) データガバナンスドキュメント」セクションの「ポリシーの適用」を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「完了」をクリックして **** 選択内容を確認し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-streaming-profile-destinations/review.png)

## セグメントのライセンス認証の検証 {#verify}

エクスポートさ [!DNL Experience Platform] れたデータは、JSON 形式のターゲットの出力先に格納されます。 例えば、次のイベントには、特定のセグメントを対象としていて、別のセグメントを終了した対象ユーザーの電子メールアドレスプロファイル属性が含まれています。 このようなお客様の id は、d と email になります。

```json
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
