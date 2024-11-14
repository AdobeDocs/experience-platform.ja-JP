---
title: ID 設定 UI
description: ID 設定ユーザーインターフェイスの使用方法を説明します。
exl-id: 738b7617-706d-46e1-8e61-a34855ab976e
source-git-commit: ee0f6d6dbbbdf55a1a0f10038b785e48f2b41474
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 1%

---

# ID 設定 UI

>[!AVAILABILITY]
>
>ID グラフリンクルールは現在、限定提供（LA）です。 開発用サンドボックスでこの機能にアクセスする方法については、Adobeアカウントチームにお問い合わせください。

ID 設定は、Adobe Experience Platform ID サービス UI の機能で、一意の名前空間を指定し、名前空間の優先度を設定するために使用できます。

UI で ID 設定を指定する方法については、このガイドを参照してください。

## 前提条件

ID 設定の使用を開始する前に、次のドキュメントをお読みください。

* [ID グラフリンクルール](./overview.md)
* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [実装ガイド](./implementation-guide.md)
* [グラフ設定の例](./example-configurations.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション](./graph-simulation.md)

## ID 設定の指定

ID 設定にアクセスするには、Adobe Experience Platform UI の ID サービスワークスペースに移動し、「**[!UICONTROL 設定]**」を選択します。

![ 選択された「ID 設定」ボタン ](../images/rules/identities-ui.png)

ID 設定ページは、[!UICONTROL  ユーザー名前空間 ] および [!UICONTROL  デバイスまたは cookie の名前空間 ] の 2 つのセクションに分かれています。 人物の名前空間は、1 人の個人の識別子です。 クロスデバイス ID、メールアドレスおよび電話番号から選択できます。 デバイスまたは cookie の名前空間は、デバイスおよび web ブラウザーの識別子であり、ユーザーの名前空間よりも優先することはできません。 また、デバイスまたは cookie の名前空間を一意の名前空間に指定することはできません。

### 名前空間の優先度の設定

名前空間の優先度を設定するには、ID 設定メニューで名前空間を選択し、その名前空間を好みの順序にドラッグ&amp;ドロップします。 リストの名前空間を上位に配置して優先度を高め、逆にリストの名前空間を下位に配置して優先度を低くします。 優先度が最も高い名前空間も、一意の名前空間として指定する必要があります。

![ ユーザー名前空間がハイライト表示された ID 設定ワークスペース。](../images/rules/namespace-priority.png)

### 一意の名前空間の指定

一意の名前空間を指定するには、その名前空間に対応する [!UICONTROL  グラフごとに一意 ] チェックボックスを選択します。 ID 設定設定に複数の一意の名前空間を選択できます。

![ 選択され、一意として定義された 2 つの名前空間。](../images/rules/unique-namespace.png)

一意の名前空間が確立されると、グラフは、一意の名前空間を含む複数の ID を持つことができなくなります。 例えば、CRMID を一意の名前空間として指定した場合、グラフは、CRMID 名前空間を持つ 1 つの ID のみを持つことができます。 詳しくは、[ID 最適化アルゴリズムの概要 ](./identity-optimization-algorithm.md#unique-namespace) を参照してください。

設定が完了したら、「**[!UICONTROL 次へ]**」を選択します。 確認メッセージが表示されます。この機会に、設定が正しいことを確認し、「**[!UICONTROL 完了]**」を選択します。

![ 「完了」がハイライト表示された検証ページ ](../images/rules/finish.png)

グラフが（設定を保存した後に **更新された場合にのみ既存のグラフがグラフアルゴリズムの影響を受けること、および名前空間の優先度が変更された後でもリアルタイム顧客プロファイル上のイベントフラグメントのプライマリ ID が更新されないことを示す警告メッセージが表示されます** さらに、新しい設定または更新された設定が有効になるまで、最大 **6 時間** かかることが通知されます。 確認するには、サンドボックス名を入力し、「**[!UICONTROL 確認]**」を選択します。

![ 設定が処理されるまで 6 時間の遅延に関する警告を表示する確認ウィンドウ。](../images/rules/confirm-settings.png)

## 次の手順

ID グラフリンクルールについて詳しくは、次のドキュメントを参照してください。

* [ID グラフリンクルールの概要](./overview.md)
* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [実装ガイド](./implementation-guide.md)
* [グラフ設定の例](./example-configurations.md)
* [トラブルシューティングと FAQ](./troubleshooting.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション UI](./graph-simulation.md)