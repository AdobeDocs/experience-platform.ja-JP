---
title: Reactor APIで の応答の並べ替え
description: Reactor API でリソースをリストする際に結果をフィルタリングする方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: ht
source-wordcount: '123'
ht-degree: 100%

---

# Reactor API での応答の並べ替え

Reactor API でエンドポイントをリストすると、指定した属性に基づいて返されたリソースを並べ替えることができます。リクエストパスに `sort` パラメーターを指定することで、応答の並べ替え順を設定できます。

## 昇順に並べ替え

並べ替える属性を指定し、先頭に `+` を付けると、リソースを属性の昇順で並べ替えることができます。


`GET /companies/:company_id/properties?sort=+name`

## 降順に並べ替え

並べ替える属性を指定し、先頭に `-` を付けると、リソースを属性の降順で並べ替えることができます。


`GET /companies/:company_id/properties?sort=-name`

## 複数の並べ替え

複数の値で並べ替えるには、並べ替えディレクティブをコンマ区切りのリストとして指定します。


`GET /companies/:company_id/properties?sort=+name,-org_id`
