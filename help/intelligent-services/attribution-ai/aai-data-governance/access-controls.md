---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;adobe admin console
solution: Experience Platform
feature: Attribution AI
title: Attribution AIのアクセス制御
description: このドキュメントでは、Attribution AIの属性ベースのアクセス制御に関する情報を提供します。
source-git-commit: d82fd8dd5efbe314c09d32905f8ab964640cc11a
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 43%

---


# アクセス制御

Attribution AIのアクセス制御は、 [Adobe Admin Console](https://adminconsole.adobe.com/). この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。

アクセス制御について詳しくは、[アクセス制御の概要](../../../access-control/home.md)を参照してください。

## 属性ベースのアクセス制御

>[!IMPORTANT]
>
>属性ベースのアクセス制御は、現在、限られたリリースでのみ使用できます。

[属性ベースのアクセス制御は、管理者が属性に基づいて特定のオブジェクトや機能へのアクセスを制御できるようにする Adobe Experience Platform の機能です。](../../../access-control/abac/overview.md)属性は、スキーマフィールドやセグメントに追加されるラベルなど、オブジェクトに追加されるメタデータであることがあります。 管理者は、ユーザーアクセス権限を管理する属性を含めた、アクセスポリシーを定義します。

この機能を使用すると、エクスペリエンスデータモデル（XDM）スキーマフィールドに、組織またはデータの使用範囲を定義するラベルを付けることができます。同時に、管理者は、ユーザーと役割の管理インターフェイスを使用して、XDM スキーマフィールドに関するアクセスポリシーを定義し、ユーザーまたはユーザーのグループ（内部、外部、またはサードパーティのユーザー）に与えるアクセスをうまく管理できます。また、属性ベースのアクセス制御により、管理者は特定のセグメントへのアクセスを管理できます。

管理者は、属性ベースのアクセス制御を通じて、すべての Platform ワークフローとリソースにわたって、機密性の高い個人データ (SPD) と個人情報 (PII) の両方に対するユーザーのアクセスを制御できます。 管理者は、特定のフィールドおよびそれらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

属性ベースのアクセス制御により、一部のフィールドおよび機能は、アクセスが制限され、特定のAttribution AIサービスモデルで使用できない場合があります。 例として、「ID」、「スコア定義」、「クローン」があります。

Attribution AIワークスペースの上部 **インサイトページ**&#x200B;の場合、サイドバーに表示される詳細はアクセスが制限されています。

![制限されたAttribution AIフィールドが強調表示されたスキーマワークスペース。](../images/user-guide/access-restricted.png)

で制限されたスキーマのデータセットを選択した場合 **[!UICONTROL モデルワークフローを作成]** ページの場合、データセット名の横に次のメッセージの警告記号が表示されます。 [!UICONTROL 制限された情報は除外されます].

![制限されたAttribution AIセットフィールドが強調表示されたデータワークスペース。](../images/user-guide/restricted-info-excluded.png)

制限されたスキーマを持つデータセットをでプレビューする場合 **[!UICONTROL モデルワークフローを作成]** ページに表示される場合、次のことを知らせる警告が表示されます。 [!UICONTROL アクセス制限により、一部の情報はデータセットのプレビューに表示されません。]

![制限付きのプレビュー済みスキーマフィールドの結果がハイライト表示されたAttribution AIワークスペース。](../images/user-guide/restricted-dataset-preview.png)

制限された情報を持つモデルを作成し、 **[!UICONTROL 目標を定義]** 手順の場合、上部に警告が表示されます。 [!UICONTROL アクセス制限により、特定の情報は設定に表示されません。]

![モデル結果の制限されたフィールドを含むAttribution AIワークスペースがハイライト表示されます。](../images/user-guide/information-not-displayed-save-and-exit.png)

## 次の手順

このガイドを読むことで、[!DNL Experience Platform] のアクセス制御の主な原則を学びました。[!DNL Admin Console] を使用して製品プロファイルを作成して [!DNL Platform] の権限を割り当てる方法に関する詳細な手順について、引き続き『[アクセス制御ユーザーガイド](../overview.md)』をご覧ください。
