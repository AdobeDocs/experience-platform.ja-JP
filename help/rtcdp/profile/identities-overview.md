---
keywords: ID rtcdp;rtcdp ID;Real-Time CDP ID
title: Real-time Customer Data Platform における ID
description: Adobe Experience Platform ID サービスは、すべてのデバイスやシステム間で ID を橋渡しすることで、顧客とその行動をより確実に把握できるようにします。
feature: Get Started, Identities
exl-id: 2b0d84de-9710-412e-ace7-56e3977245aa
source-git-commit: db57fa753a3980dca671d476521f9849147880f1
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 100%

---

# ID の概要

Adobe Experience Platform [!DNL Identity Service] を利用すると、すべてのデバイスやシステム間で ID を橋渡しできるので、顧客とその行動をより確実に把握できるようになります。通常、顧客は複数のチャネルを通じてブランドとやり取りします。例えば、web サイトのオンライン閲覧、店頭での購入、ロイヤルティプログラムへの参加、サポートを受けるためのヘルプデスクへの電話などがあります。これらの複数のシステムで、その顧客用に作成された ID がありますが、[!DNL Identity Service] を使用すると、それらの ID を統合して全体像を把握できるようになります。

5 つの異なるチャネル間でブランドとやりとりする 5 人の顧客が同一人物であることが分かれば、各インタラクションを通じて一貫性のあるパーソナライズされた関連性のあるエクスペリエンスを顧客に提供することができます。顧客に関する情報がさらに多くなると（例えば、Web サイトの匿名ブラウザーがアカウントにサインアップしてログインする）、情報が結合され、顧客の姿がより明確になります。

## ID 名前空間

ID 名前空間は [!DNL Identity Service] のコンポーネントで、顧客 ID に追加のコンテキストを提供するインジケーターとして機能します。よく使用される ID 名前空間の例としては、「メール」が挙げられます。複数の web サイトで同じメールアドレスが使用されると、実際には同じ顧客に属する複数の異なる ID（それぞれが一意の顧客 ID を持つ）をまとめることができます。[!DNL Experience Platform] では、ID 名前空間を使用して、ユーザーインターフェイス内で個々のプロファイルを検索できます。プロファイルの表示について詳しくは、[プロファイル参照の概要](profile-browse.md)を参照してください。ID 名前空間について詳しくは、「[ID 名前空間の概要](../../identity-service/namespaces.md)」を参照してください。

## ID グラフ

ID グラフは、異なる ID 名前空間間の関係のマップで、顧客が様々なチャネルを通じてブランドとどのようにやり取りするかを視覚的に示します。すべての顧客 ID グラフは、顧客の活動に応じて、[!DNL Identity Service]によって、ほぼリアルタイムでまとめて管理および更新されます。

[!DNL Identity Service]は、プライベートグラフと呼ばれる、組織のみが表示できる、データに基づいて構築された ID グラフを管理します。[!DNL Identity Service]は、取り込んだデータレコードに複数の ID が含まれている場合にプライベートグラフを拡張し、見つかった ID 間の関係を追加します。

## 次の手順

ID および ID 間の関係は [!DNL Identity Service] で定義および維持管理され、[!DNL Real-Time Customer Profile] で活用されて個々の顧客とそのインタラクションの全体像が作成されます。詳しくは、[ID サービスのドキュメント](../../identity-service/home.md)を参照してください。
