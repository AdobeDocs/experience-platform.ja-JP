---
title: Reactor API での応答の並べ替え
description: Reactor API でリソースをリストする際に結果をフィルタリングする方法を説明します。
exl-id: 49dcf0b6-4ce8-41d9-9e3a-e44f5c0ff905
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 100%

---

# Reactor API での応答の並べ替え

Reactor API でエンドポイントをリストすると、指定した属性に基づいて返されたリソースを並べ替えることができます。リクエストパスに `sort` パラメーターを指定することで、応答の並べ替え順を設定できます。

## 昇順に並べ替え

リソースを属性の昇順で並べ替えるするには、並べ替えたい属性を指定し、その属性の前に `+` を付けます。

`GET /companies/:company_id/properties?sort=+name`

## 降順に並べ替え

リソースを属性別に降順で並べ替えるには、並べ替えたい属性を指定し、その属性の前に `-` を付けます。

`GET /companies/:company_id/properties?sort=-name`

## 複数の並べ替え

複数の値で並べ替えるには、並べ替えディレクティブをコンマ区切りリストで指定します。


`GET /companies/:company_id/properties?sort=+name,-org_id`
