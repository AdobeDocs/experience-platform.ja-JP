---
keywords: Experience Platform;ホーム;人気のトピック;ポリシーの適用;自動適用;API ベースの適用;データガバナンス
solution: Experience Platform
title: ポリシー適用の概要
description: Adobe Experience Platformでのデータ使用ポリシーの適用方法を説明します。
exl-id: d19d8060-85a1-405c-856d-f59041947a33
source-git-commit: 7b15166ae12d90cbcceb9f5a71730bf91d4560e6
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 48%

---

# ポリシー適用の概要

1 回 [データ使用ラベル](../labels/overview.md) 適用され [データ使用ポリシー](../policies/overview.md) が定義されている場合は、これらのポリシーを適用して、ポリシー違反を構成するデータ操作を防止できます。

>[!NOTE]
>
>このドキュメントでは、データ使用ポリシーの適用に焦点を当てます。 アクセス制御ポリシーの詳細については、 [属性ベースのアクセス制御](../../access-control/abac/overview.md).

Adobe Experience Platformでポリシーを適用する方法は 2 つあります。自動強制と API ベースの強制

## 自動適用

Experience Platform では、データ系列、データ分類、ポリシー管理機能を活用して、ポリシー違反を自動的に評価し、明らかにします。詳しくは、[自動ポリシー適用](./auto-enforcement.md)の概要を参照してください。

## API ベースの適用

[!DNL Policy Service] API は、マーケティングアクションをデータセットや任意のデータ使用ラベルの組み合わせに対してテストし、ポリシー違反が発生したかどうかを確認できるエンドポイントを提供します。その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データガバナンスポリシーのコンプライアンスを適切に実施できます。

API を使用してポリシーを評価する手順については、[API ベースの適用](./api-enforcement.md)に関するチュートリアルを参照してください。
