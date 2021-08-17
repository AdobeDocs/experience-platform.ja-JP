---
keywords: プロファイルの宛先のアクティブ化、宛先のアクティブ化、データのアクティブ化電子メールマーケティングの宛先をアクティブ化する。クラウドストレージの宛先のアクティブ化
title: ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化
type: Tutorial
seo-title: ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化
description: ストリーミングプロファイルベースの宛先にセグメントを送信して、Adobe Experience Platformで保有するオーディエンスデータをアクティブ化する方法について説明します。
seo-description: ストリーミングプロファイルベースの宛先にセグメントを送信して、Adobe Experience Platformで保有するオーディエンスデータをアクティブ化する方法について説明します。
source-git-commit: 02c22453470d55236d4235c479742997e8407ef3
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 14%

---


# ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化

## 概要 {#overview}

この記事では、Amazon Kinesisなど、Adobe Experience Platformストリーミングプロファイルベースの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、宛先](./connect-destination.md)に[接続している必要があります。 まだの場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照して、使用する宛先を設定します。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL 接続/宛先]**&#x200B;に移動し、「**[!UICONTROL 参照]**」タブを選択します。

   ![「宛先の参照」タブ](../assets/ui/activate-streaming-profile-destinations/browse-tab.png)

1. 次の図に示すように、セグメントをアクティブ化する宛先に対応する「**[!UICONTROL Add segments]**」ボタンを選択します。

   ![ボタンの有効化](../assets/ui/activate-streaming-profile-destinations/activate-buttons-browse.png)

1. 次のセクションに移動して[セグメントを選択します](#select-segments)。

## セグメントの選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

![セグメントの選択](../assets/ui/activate-streaming-profile-destinations/select-segments.png)

## プロファイル属性の選択 {#select-attributes}

ターゲットの宛先に送信するプロファイル属性を選択します。

>[!NOTE]
>
> Adobe Experience Platformは、スキーマの4つの推奨される、一般的に使用される属性を使用して、選択内容を事前入力します。`person.name.firstName`、`person.name.lastName`、`personalEmail.address`、`segmentMembership.status`。

ファイルのエクスポート方法は、`segmentMembership.status`が選択されているかどうかによって異なります。
* 「`segmentMembership.status`」フィールドを選択した場合、エクスポートされるファイルには、最初のフルスナップショットの「**[!UICONTROL アクティブ]**」メンバーと、後続の増分エクスポートの「**[!UICONTROL アクティブ]**」および「**[!UICONTROL 期限切れ]**」メンバーが含まれます。
* 「 `segmentMembership.status` 」フィールドが選択されていない場合、エクスポートされるファイルには、最初の完全なスナップショットとそれ以降の増分エクスポートで、**[!UICONTROL アクティブ]**&#x200B;メンバーのみが含まれます。

![推奨属性](../assets/ui/activate-streaming-profile-destinations/attributes-default.png)

1. **[!UICONTROL 属性を選択]**&#x200B;ページで、「**[!UICONTROL 新しいフィールドを追加]**」を選択します。

   ![新しいマッピングの追加](../assets/ui/activate-streaming-profile-destinations/add-new-field.png)

1. 「**[!UICONTROL スキーマフィールド]**」エントリの右側にある矢印を選択します。

   ![ソースフィールドの選択](../assets/ui/activate-streaming-profile-destinations/select-schema-field.png)

1. **[!UICONTROL フィールド]**&#x200B;を選択ページで、宛先に送信するXDM属性を選択し、「**[!UICONTROL 選択]**」を選択します。

   ![ソースフィールドページを選択](../assets/ui/activate-streaming-profile-destinations/target-field-page.png)


1. マッピングをさらに追加するには、手順1 ～ 3を繰り返し、「**[!UICONTROL 次へ]**」を選択します。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 次に、ポリシーに違反する例を示します。 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法について詳しくは、データガバナンスのドキュメントの節の「[ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement)」を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確認し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-streaming-profile-destinations/review.png)

## セグメントのアクティベーションの検証 {#verify}


電子メールマーケティングの宛先とクラウドストレージの宛先の場合、Adobe Experience Platformはストレージの指定した場所にタブ区切りの`.csv`ファイルを作成します。 新しいファイルはストレージの場所に毎日作成されます。デフォルトのファイル形式は次のとおりです。
`<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv`

3 日連続で受け取るファイルは次のようになります。

```console
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。書き出されたファイルの構造を理解するには、サンプルの.csvファイル](../assets/common/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv)を[ダウンロードします。 このサンプルファイルには、プロファイル属性`person.firstname`、`person.lastname`、`person.gender`、`person.birthyear`、`personalEmail.address`が含まれます。
