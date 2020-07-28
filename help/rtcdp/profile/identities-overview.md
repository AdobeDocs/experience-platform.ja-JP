---
title: ID と ID 名前空間
seo-title: Adobe Experience Platform ID サービス
description: 説明
seo-description: SEO 説明
translation-type: tm+mt
source-git-commit: b96286f6a06f0583b45343a513ee64f0025d79a7
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 62%

---


# リアルタイム CDP での ID

Adobe Experience Platform [!DNL Identity Service] helps you to gain a better view of your customers and their behavior by bridging together identities across devices and systems. 通常、顧客は複数のチャネルを通じてブランドとやり取りします。これには、Web サイトのオンライン閲覧、店頭での購入、ロイヤリティープログラムへの参加、サポートのためのヘルプデスクへの電話、他多数が含まれます。Across these multiple systems, there is an identity created for that customer, and [!DNL Identity Service] makes it possible to bring those identities together to see the complete picture.

5 つの異なるチャネル間でブランドとやりとりする 5 人の顧客が同一人物であることが分かれば、各インタラクションを通じて一貫性のあるパーソナライズされた関連性のあるエクスペリエンスを顧客に提供することができます。顧客に関する情報がさらに多くなると（例えば、Web サイトの匿名ブラウザーがアカウントにサインアップしてログインする）、情報が結合され、顧客の姿がより明確になります。

## ID 名前空間

Identity namespaces are a component of [!DNL Identity Service] and serve as indicators providing additional context to customer identities. 一般的に使用される ID 名前空間の例としては、「電子メール」が挙げられます。複数の Web サイトで同じ電子メールアドレスが使用されると、同じ顧客に属する複数の異なる（それぞれが一意の顧客 ID を持つ） ID を結合できます。[!DNL Experience Platform] では、ID 名前空間を使用して、ユーザーインターフェイス内の個々のプロファイルを検索できます。プロファイルの表示について詳しくは、「[プロファイルビューアの概要](/help/rtcdp/profile/profile-viewer.md)」を参照してください。ID 名前空間について詳しくは、「[ID 名前空間の概要](../../identity-service/namespaces.md)」を参照してください。

## ID グラフ

ID グラフは、異なる ID 名前空間間の関係のマップで、顧客が様々なチャネルを通じてブランドとどのようにやり取りするかを視覚的に示します。All customer identity graphs are collectively managed and updated by [!DNL Identity Service] in near real-time, in response to customer activity.

[!DNL Identity Service] 組織のみが表示し、自分のデータ（「個人用グラフ」と呼ばれる）に基づいて作成した身分証明書を管理します。 [!DNL Identity Service] 取り込まれたデータレコードに複数のIDが含まれる場合にプライベートグラフが増加し、見つかったID間の関係が追加されます。

## 次の手順

Identities, and the relationships between them, are defined and maintained by [!DNL Identity Service] and leveraged by [!DNL Real-time Customer Profile] to build a complete picture of each individual customer and their interactions. 詳しくは、[ID サービスのドキュメント](../../identity-service/home.md)を参照してください。