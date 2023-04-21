---
keywords: Experience Platform;ホーム;人気のトピック;トラブルシューティング;アクセス制御
solution: Experience Platform
title: アクセス制御トラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform のアクセス制御に関するよくある質問に対する回答を示します。
exl-id: c299c0c4-dbee-4e6d-8af4-2446444bed69
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: ht
source-wordcount: '407'
ht-degree: 100%

---

# アクセス制御トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform のアクセス制御に関するよくある質問に対する回答を示します。その他の [!DNL Platform] サービスに関する質問とトラブルシューティングについては、『[Experience Platform トラブルシューティングガイド](../landing/troubleshooting.md)』を参照してください。

[!DNL Experience Platform] は、[Adobe Admin Console](https://adminconsole.adobe.com) の製品プロファイルを活用して、役割ベースのアクセス制御を提供し、ユーザーを権限およびサンドボックスにリンクします。  詳しくは、「[アクセス制御の概要](home.md)」を参照してください。

## 現在のアクセス権限はどこで確認できますか。

組織のシステム管理者、製品管理者または製品プロファイル管理者は、割り当てられた製品プロファイルとそれによって提供される権限を Adobe Admin Console で確認できます。を使用して製品プロファイルの権限を表示する方法については、『[アクセス制御ユーザガイド](./ui/overview.md)』を参照してください[!DNL Admin Console]。

管理者以外の場合でも、アクセス制御 API の `/acl/effective-policies` エンドポイントにリクエストを送信することで、現在のアクセス権限を表示することができます。詳しくは、『[アクセス制御開発者ガイド](./api/effective-policies.md)』の「有効ポリシーの表示」の節を参照してください。

## [!DNL Platform] UI の一部の機能は使用できません。これらの機能へのアクセスは、権限によってどのように制御されますか。

特定の [!DNL Platform] 機能に対するアクセス権限を持っていない場合、その機能は [!DNL Experience Platform] UI で非表示または灰色で表示されます。例えば、「[!UICONTROL プロファイル]」タブを表示するには、「[!UICONTROL プロファイルの表示]」または「[!UICONTROL プロファイルの管理]」権限が必要です。[!DNL Experience Platform] 機能に対する追加の権限が必要な場合は、管理者に問い合わせてください。

## 権限はどのようにグループ化されていますか。使用したい権限を含むグループはどれですか。

権限は、適用対象の [!DNL Platform] 機能（[!DNL Data Management] や [!DNL Profile Management] など）別にグループ化され、分類されています。使用可能な権限とその属するグループの完全なリストについては、「アクセス制御の概要」の「[権限](home.md#permissions)」の節を参照してください。

役割ベースのアクセス制御の提供について詳しくは、「[アクセス制御の概要](home.md)」を参照してください。

## Adobe IO から Business ID に移行すると、権限はどうなりますか。

アクセス制御は、権限の付与にユーザー ID（ユーザーに割り当てられた内部の一意の ID）を使用します。組織を Adobe ID から Business ID に移行すると、ユーザー ID が変更され、新しく生成されたユーザー ID がアクセス制御で使用されるので、そのユーザーに設定されたすべての権限は失われます。組織が Business ID に移行されている場合、アドビ担当者に連絡して、Adobe IDから Business ID にユーザー ID を移行してください。
