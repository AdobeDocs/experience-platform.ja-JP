---
title: クエリテンプレート
description: クエリテンプレートは再利用可能な保存済みの SQL クエリで、他のユーザーが再利用して時間と労力を節約できます。 クエリエディターまたはクエリサービス API を使用して作成でき、すべての Experience Platform データセットで使用できます。
exl-id: e74d058f-bb89-45ed-83cc-2e3a33401270
source-git-commit: a085bac6b4ee825d534710ae91d6690fa076e873
workflow-type: ht
source-wordcount: '436'
ht-degree: 100%

---

# クエリテンプレート

Adobe Experience Platform クエリサービスを使用すると、SQL コードを保存して、クエリテンプレートの形式で再利用できます。 テンプレートを使用すると、よく実行されるタスクの繰り返しを避けることで、手間を省くことができます。 組織内でテンプレートを共有すると、基になる SQL にアクセスしたり理解したりする必要なく、クエリ値を簡単に変更できます。

このドキュメントでは、クエリサービスでクエリテンプレートを作成するために必要な情報を提供します。

## 前提条件

クエリエディターにアクセスし、Platform UI 内のクエリダッシュボードを表示するには、[!UICONTROL クエリの管理]権限が有効になっている必要があります。この権限は、Adobe [Admin Console](https://adminconsole.adobe.com/) を使用して有効にできます。この権限を有効にするための管理者権限がない場合は、組織の管理者に問い合わせてください。 [Admin Console を使用した権限の追加に関する完全な手順](../../access-control/home.md)については、アクセス制御に関するドキュメントを参照してください。

## クエリテンプレートの作成

クエリテンプレートを作成するには、Query Service API `query-templates` エンドポイントに対して POST リクエストを実行するか、クエリエディターでクエリを記述、命名、保存するという 2 つの方法があります。

### クエリエディターを使用して、クエリを作成し、テンプレートとして保存します。

クエリエディターを使用して[書き込み](./user-guide.md#query-authoring)および[クエリを保存](./user-guide.md#saving-queries)する方法については、ドキュメントを参照してください。クエリに名前を付けて保存すると、「[!UICONTROL テンプレート]」タブからクエリテンプレートとして再利用できます。

Platform UI のクエリワークスペースから、「**[!UICONTROL テンプレート]**」を選択して、使用可能な保存済みクエリのリストを表示します。

![「テンプレート」タブが強調表示されたクエリワークスペース。](../images/ui/query-templates/query-templates.png)

関連するテンプレート情報を見つけるには、使用可能なリストから任意のクエリテンプレートを選択して、詳細パネルを開きます。

![クエリ ID が強調表示されたクエリワークスペースの詳細パネル。](../images/ui/query-templates/details-panel.png)

### Query Service API を使用してテンプレートを作成する

Query Service API を使用した[クエリテンプレートの作成方法](../api/query-templates.md#create-a-query-template)の手順については、ドキュメントを参照してください。 新しく作成したクエリテンプレートの詳細は、応答本文に含まれています。

>[!NOTE]
>
>API を使用して作成したテンプレートは、「Platform UI クエリサービステンプレート」タブにも表示されます。

## 次の手順

このドキュメントでは、クエリサービスでクエリテンプレートを作成する方法をより深く理解しました。 クエリサービスの機能について詳しくは、[UI の概要](./overview.md)または [Query Service API ガイド](../api/getting-started.md)を参照してください。

API または UI 用[クエリエディターガイド](./user-guide.md#scheduled-queries)を使用して、クエリをスケジュールする方法について詳しくは、[スケジュールされたクエリエンドポイントガイド](../api/scheduled-queries.md)を参照してください。
