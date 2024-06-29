---
title: ID 設定 UI
description: ID 設定ユーザーインターフェイスの使用方法を説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 605aa5ed2db2bfd7f787f2dff9fa00cee2afbce6
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# ID 設定 UI

>[!AVAILABILITY]
>
>この機能はまだ使用できません。ID グラフリンクルールのベータ版プログラムは、開発用サンドボックスで 7 月に開始される予定です。 パーティシペーションの条件については、Adobeアカウントチームにお問い合わせください。

ID 設定は、Adobe Experience Platform ID サービス UI の機能で、一意の名前空間を指定し、名前空間の優先度を設定するために使用できます。

ID 設定ツールの使用方法については、このガイドを参照してください。

## 前提条件

ID 設定の使用を開始する前に、次のドキュメントをお読みください。

* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション](./graph-simulation.md)

## ID 設定の指定

ID 設定にアクセスするには、Adobe Experience Platform UI の ID サービスワークスペースに移動し、以下を選択します。 **[!UICONTROL 設定]**.

![「ID 設定」ボタンが選択されている。](../images/rules/identity-ui.png)

ID 設定ページが表示され、実稼動サンドボックスで設定を完了する前に、まず開発用サンドボックスで ID 設定をテストおよび検証するように促す確認メッセージが表示されます。

![ID 設定ページ。](../images/rules/identity-settings.png)

ID 設定ページは 2 つのセクションに分かれています。 [!UICONTROL 人物の名前空間] および [!UICONTROL デバイスまたは cookie の名前空間]. 人物の名前空間は、1 人の個人の識別子です。 クロスデバイス ID、メールアドレスおよび電話番号から選択できます。 デバイスまたは cookie の名前空間は、デバイスおよび web ブラウザーの識別子であり、ユーザーの名前空間よりも優先することはできません。 また、デバイスまたは cookie の名前空間を一意の名前空間に指定することはできません。

### 一意の名前空間の指定

一意の名前空間を指定するには、 [!UICONTROL グラフごとのユニーク数] その名前空間に対応するチェックボックス。 ID 設定設定に複数の一意の名前空間を選択できます。

![2 つの一意の名前空間が選択されました。](../images/rules/unique-namespaces.png)

一意の名前空間が確立されると、グラフは、一意の名前空間を含む複数の ID を持つことができなくなります。 例えば、一意の名前空間として Analytics カスタム ID を指定した場合、Analytics カスタム ID 名前空間を使用してグラフに含める ID は 1 つだけです。 詳しくは、 [id 最適化アルゴリズムの概要](./identity-optimization-algorithm.md#unique-namespace).

### 名前空間の優先度の設定

名前空間の優先度を設定するには、ID 設定メニューで名前空間を選択し、その名前空間を好みの順序にドラッグ&amp;ドロップします。 リストの名前空間を上位に配置すると優先度が高くなり、逆にリストの名前空間を下位に配置すると優先度が低くなります。 優先度が最も高い名前空間も、一意の名前空間として指定する必要があります。

設定が完了したら、次を選択します。 **[!UICONTROL 次]**. 確認メッセージが表示されます。この機会に、設定が正しいことを確認し、を選択します。 **[!UICONTROL 終了]**.

![検証ページ。](../images/rules/validate.png)

新しい設定が、既に取り込まれた ID グラフ内の既存のリンクやエクスペリエンスイベントプロファイルフラグメントには影響しないことを示す警告が表示されます。 確定するには、サンドボックス名を入力し、次のいずれかを選択します **[!UICONTROL 確認]**.

![確認ウィンドウ。](../images/rules/confirm.png)