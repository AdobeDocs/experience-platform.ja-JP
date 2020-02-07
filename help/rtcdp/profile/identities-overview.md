---
title: IDとID名前空間
seo-title: Adobe Experience Platform ID サービス
description: description
seo-description: seo説明
translation-type: tm+mt
source-git-commit: 5cba5a1e8139dd85f23250d42a1cd8d2318eb916

---


# リアルタイムCDPのアイデンティティ

Adobe Experience Platform Identity Serviceは、デバイスとシステム間でIDを結合することで、顧客とその行動をより良く把握できるようにします。 通常、顧客は複数のチャネルを通じてブランドとやり取りします。これには、Webサイトのオンライン閲覧、店頭での購入、忠誠度プログラムへの参加、サポートのためのヘルプデスクへの電話などが含まれます。 複数のシステムにわたって、その顧客に対してIDが作成され、IDサービスを使用すると、それらのIDを統合して全体像を確認できます。

5つの異なるチャネル間でブランドとやりとりする5人の顧客の代わりに、同じ顧客であることが分かり、各インタラクションを通じて一貫性のあるパーソナライズされた関連性のあるエクスペリエンスを受け取ることができます。 顧客に関する情報がさらに多くなると（例えば、Webサイトの匿名ブラウザーがアカウントにサインアップしてログインすることを決定する）、情報が関連付けられ、顧客の姿がより明確になります。

## ID名前空間

ID名前空間はIDサービスのコンポーネントで、顧客IDに追加のコンテキストを提供するインジケーターとして機能します。 一般的に使用されるID名前空間の例としては、「電子メール」が挙げられます。複数のWebサイトで同じ電子メールアドレスを使用すると、同じ顧客に属する一意の顧客IDを持つ複数の異なるIDを結合できます。 Experience Platformでは、ID名前空間を使用して、ユーザーインターフェイス内の個々のプロファイルを検索できます。 プロファイルの表示について詳しくは、プロファイルビューアの概要 [を参照してください](/help/rtcdp/profile/profile-viewer.md)。 ID名前空間について詳しくは、Adobe I/OのID名 [前空間の概要を参照してください](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_namespace_overview/identity_namespace_overview.md)。

## IDグラフ

IDグラフは、異なるID名前空間間の関係のマップで、顧客が様々なチャネルを通じてブランドとどのように相互作用するかを視覚的に示します。 すべての顧客IDグラフは、顧客の行動に応じて、ほぼリアルタイムでIDサービスによって管理および更新されます。

アイデンティティーサービスは、組織のみが表示し、データに基づいて構築されたアイデンティティーグラフ（「プライベートグラフ」と呼ばれる）を管理します。 取り込んだデータレコードに複数のIDが含まれている場合、アイデンティティサービスはプライベートグラフを増補し、見つかったID間の関係を追加します。

## 次の手順

IDとID間の関係は、IDサービスによって定義および維持され、リアルタイム顧客プロファイルによって使用され、個々の顧客とそのインタラクションの全体像を作成します。 詳しくは、Adobe I/Oに関する [IDサービスのドキュメントを参照してください](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/identity_services_architectural_overview/identity_services_architectural_overview.md)。