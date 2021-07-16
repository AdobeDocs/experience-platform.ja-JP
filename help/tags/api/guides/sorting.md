---
title: Reactor APIでの応答の並べ替え
description: Reactor APIでリソースをリストする際に結果をフィルタリングする方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 0%

---

# Reactor APIでの応答の並べ替え

Reactor APIのエンドポイントをリストすると、返されたリソースを指定した属性に基づいて並べ替えることができます。 リクエストパスに`sort`パラメーターを指定することで、応答の並べ替え順を設定できます。

## 昇順に並べ替え

リソースは、
属性を使用して並べ替え、先頭に`+`を付けます。

`GET /companies/:company_id/properties?sort=+name`

## 降順に並べ替え

リソースは、
属性を使用して並べ替え、先頭に`-`を付けます。

`GET /companies/:company_id/properties?sort=-name`

## 複数の並べ替え

複数の値で並べ替えるには、並べ替えディレクティブをコンマ区切りで指定します
リスト：

`GET /companies/:company_id/properties?sort=+name,-org_id`
