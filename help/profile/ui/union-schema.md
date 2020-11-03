---
keywords: Experience Platform;profile;real-time customer profile;;unified profile;Unified Profile;unified;Profile;rtcp;enable profile;Enable profile;Union schema;UNION PROFILE;union profile
title: リアルタイム顧客プロファイルUIガイド
topic: guide
description: Adobe Experience Platformユーザーインターフェイス(UI)では、組織内の任意の和集合スキーマを簡単に表示し、特定のクラスのフィールド、ID、関係および貢献しているスキーマをプレビューできます。 このガイドでは、Platform UIを使用して和集合スキーマを表示し、調査する方法について詳しく説明します。
translation-type: tm+mt
source-git-commit: e1fc20a5f791f7628a19c5440db3741c8be3743a
workflow-type: tm+mt
source-wordcount: '1023'
ht-degree: 0%

---


# [!UICONTROL 和集合スキーマ] UIガイド

Adobe Experience Platformユーザーインターフェイス(UI)では、組織内の任意の和集合スキーマを簡単に表示し、特定のクラスのフィールド、ID、関係および貢献しているスキーマをプレビューできます。 このガイドでは、Platform UIを使用して和集合スキーマを表示し、調査する方法について詳しく説明します。

## はじめに

This UI guide requires an understanding of the various [!DNL Experience Platform] services involved with managing Real-time Customer Profile data. このガイドを読む前、またはUIで作業する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Identity Service]](../../identity-service/home.md):異なるデータソース [!DNL Real-time Customer Profile] に取り込まれる際に、アイデンティティを別々のデータソースからブリッジすることで有効に [!DNL Platform]します。
* [[!DNL Experience Data Model] (XDM)](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

## 和集合スキーマについて

リアルタイムの顧客プロファイルにより、Adobe Experience Platformと統合されたシステム全体で顧客属性とタイムスタンプのある各イベントを含む、堅牢で一元化されたプロファイルを作成できます。 このデータの形式と構造はExperience Data Model(XDM)スキーマによって提供され、各スキーマはXDMクラスに基づき、そのクラスと互換性のあるフィールドを含んでいます。

スキーマは、同じクラスを参照し、使用に特有のフィールドを含む、複数の使用例に対して作成できます。 スキーマがプロファイル可能になると、和集合スキーマの一部になります。 つまり、和集合スキーマは、同じクラスを共有し、プロファイルが有効になっている複数のスキーマで構成されます。 和集合スキーマを使用すると、同じクラスを共有するスキーマ内に含まれるすべてのフィールドの結合を確認できます。 リアルタイム顧客プロファイルは、和集合スキーマを使用して個々の顧客の全体的な表示を作成します。

和集合スキーマを扱う作業には、XDMスキーマに関する深い理解が必要です。 詳しくは、スキーマ組成の [基本を読んでください](../../xdm/schema/composition.md)。

## 表示和集合スキーマ

Platform UI内の和集合スキーマに移動するには、左側のナビゲーションから **[!UICONTROL プロファイル]** を選択し、「 **[!UICONTROL 和集合スキーマ]** 」タブを選択します。 「 [!UICONTROL 和集合スキーマ] 」タブが開き、現在選択されているクラスの和集合スキーマが表示されます。

![](../images/union-schema/union-schema-landing.png)

## クラスを選択

特定のXDMクラスの和集合スキーマを表示するには、「 **[!UICONTROL Class]** 」ドロップダウンからクラスを選択します。 すべてのクラスに和集合スキーマがあるわけではないので、和集合スキーマを持つクラス(プロファイルが有効になっているスキーマを持つクラスを意味)のみがドロップダウンで使用できます。

クラスを選択すると、表示されるスキーマが更新され、選択したクラスの和集合スキーマが反映されます。 例えば、「 **** XDM Individual」プロファイルを選択して、そのクラスの和集合スキーマを表示できます。

![](../images/union-schema/union-schema-class.png)

## 和集合スキーマの調査

和集合スキーマを調べるには、表示構造全体を上下にスクロールしてスキーマし、右山括弧(`>`)を選択してネストされたフィールドを展開します。

![](../images/union-schema/union-schema-explore.png)

表示名、データタイプ、説明、パス、作成日、最終変更日など、詳細を表示するフィールドを選択します。 また、選択したフィールドを含む貢献スキーマのリストを表示することもできます。

![](../images/union-schema/union-schema-explore-field.png)

貢献しているスキーマの名前を選択すると、そのスキーマに関連するデータセットの名前が表示され、選択したフィールドにデータが取り込まれています。 各データセット名はリンクとして表示されます。 データセット名を選択すると、そのデータセットの「アクティビティ」タブが新しいウィンドウに開きます。

データセットのアクティビティの表示、UIでのデータセットデータのプレビューなど、データセットについて詳しくは、『 [datasets UI』ガイドを参照してください](../../catalog/datasets/user-guide.md)。

![](../images/union-schema/union-schema-field-datasets.png)

## 表示貢献スキーマ

また、スキーマのリストを拡大するために「 **[!UICONTROL すべての貢献スキーマ]** 」を選択して、和集合のスキーマにどの特定のスキーマが貢献しているかを表示することもできます。 選択したクラスと組織がPlatform内で作成したスキーマの数によっては、単一のリストを含む短いリスト、または多数のスキーマを含む長いスキーマの場合があります。

![](../images/union-schema/union-schema-contributing-schemas.png)

特定のスキーマの名前を選択すると、選択したスキーマの一部である和集合スキーマ内のフィールドが強調表示されます。 スキーマを選択すると、和集合スキーマが灰色表示になり、黒いバーが表示され、貢献しているスキーマの一部であるフィールドを示します。

![](../images/union-schema/union-schema-select-schema.png)

## 表示ID

UIを使用して、「 **[!UICONTROL Identitys]** 」を選択してリストを展開すると、和集合スキーマに含まれるIDのリストを表示できます。

![](../images/union-schema/union-schema-identities.png)

リストから個々のIDを選択すると、表示されたスキーマが必要に応じて自動的に更新され、IDフィールドが表示されます。 IDフィールドが入れ子になっている場合は、複数のフィールドを展開することもできます。

和集合スキーマ内で「ID」フィールドがハイライト表示され、IDの詳細が画面の右側に表示されます。 詳細には、IDフィールドを含む貢献スキーマのリストが含まれ、詳細を表示して、選択したIDフィールドにデータを取り込むスキーマに関連するデータセットへのリンクを探すことができます。

![](../images/union-schema/union-schema-select-identity.png)

## 表示関係

また、和集合スキーマUIを使用すると、選択したスキーマクラスに基づいてスキーマに対して定義された関係を表示できます。 関係の定義は、異なるクラスに属する2つのスキーマを結び付け、顧客データに対するより複雑な洞察を得る方法です。

選択したクラスに対してリレーションシップが確立されている場合は、[ **[!UICONTROL リレーションシップ]** ]を選択すると、リレーションシップの作成に使用するフィールドのリストが表示されます。 すべてのスキーマが関係を使用したり、必要としない場合があるので、関係セクションにフィールドを含めないことが一般的です。

UIを使用してスキーマの関係を定義する方法など、スキーマの関係について詳しくは、 [このドキュメントを参照してください](../../xdm/tutorials/relationship-ui.md)。

![](../images/union-schema/union-schema-relationships.png)

リストから関係フィールドを選択すると、ハイライト表示された関係フィールドを表示するために、必要に応じてスキーマが更新されます。 関係フィールドが入れ子になっている場合は、複数のフィールドを展開することもできます。

![](../images/union-schema/union-schema-select-relationship.png)

## 次の手順

このガイドを読むと、 [!DNL Experience Platform] UIを使用した和集合スキーマの表示方法とナビゲート方法がわかります。 スキーマの使用方法など、プラットフォーム全体での使用方法について詳しくは、 [XDMシステムの概要を読んでください](../../xdm/home.md)。
