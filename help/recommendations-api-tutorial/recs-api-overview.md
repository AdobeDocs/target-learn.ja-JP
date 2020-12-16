---
title: Adobe RecommendationsAPIの概要
keywords: recommendations;adobe recommendations;premium;api;apis
description: Adobe TargetRecommendationsには、レコメンデーション可能な商品やコンテンツのカタログを管理するためのAPIの専用セットが含まれています。レコメンデーションのアルゴリズムとキャンペーンを管理します。Web、モバイル、電子メール、IOTなどのチャネルに表示するJSON、HTMLまたはXMLオブジェクトでレコメンデーションを配信します。
kt: 3815
audience: developer
doc-type: tutorial
activity: use
feature: api
topics: recommendations;adobe recommendations;premium;api;apis
solution: Target
author: Judy Kim
translation-type: tm+mt
source-git-commit: c221f434ce9daec03dbb4d897343775b40b14462
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 1%

---


# Adobe RecommendationsAPIの概要

[!DNL Recommendations]に関連するAPIには[管理API](https://docs.adobe.com/content/help/en/target/using/apis/api-overview.html)が含まれ、以下のことができます。

* レコメンデーション可能な商品またはコンテンツのカタログの管理
* [!DNL Recommendations]アルゴリズムとアクティビティの管理

[!DNL Target] [配信API](https://docs.adobe.com/content/help/en/target/using/apis/api-overview.html)をRecommendationsと組み合わせて使用すると、次のこともできます。

* レコメンデーションをJSON、HTMLまたはXMLオブジェクトで取得し、Web、モバイル、電子メール、Internet of Things(IOT)などのチャネルで表示できるようにします。

## チュートリアルの説明

このチュートリアルでは、[!DNL Recommendations] APIを使用して[!DNL Recommendations]カタログとカスタム条件を設定および管理する実践、および配信APIを使用してrecommendationsコンテンツを取得する方法について、開発者に対する実習を紹介します。 このチュートリアルを終了すると、次のことが可能になります。

* RecommendationsAPIを使用したエンティティの設定と管理
* RecommendationsAPIを使用したカスタム条件の設定と管理
* 配信APIでRecommendationsを使用してrecommendationsを使用する方法を理解すると、HTML以外のデバイスに結果をもたらします。

## オーディエンス

このチュートリアルは、ターゲットAPIまたはRecommendationsAPIを初めて使用する開発者を対象としています。

## 前提条件

ターゲット管理APIを使用するには、[Adobe認証の設定](../apis/configure-io-target-integration.md)が必要です。 このチュートリアルを開始する前に、設定が完了していることを確認してください。

## リソース

このチュートリアルを理解し、問題なく実行するために必要な以下のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| 郵便配達人 | お使いのオペレーティングシステムの[Postmanアプリ](https://www.postman.com/downloads/)を取得します。 Postman Basicはアカウントの作成で自由です。 一般的にAdobe TargetAPIを使用するために必須ではありませんが、PostmanはAPIワークフローを容易にし、Adobe TargetはAPIの実行や動作の学習に役立ついくつかのポストマンコレクションを提供しています。 このチュートリアルの残りの部分は、Postmanの作業知識を前提としています。 援助が必要な場合は、[ポストマンのドキュメント](https://learning.getpostman.com/)を参照してください。 |
| リファレンス | このチュートリアルの残りの部分では、次のリソースに精通していることを前提としています。<UL><li>[Adobe I/Oギトブ](https://github.com/adobeio)</li><li>[ターゲットAdobe I/O文書](https://developers.adobetarget.com/api/#introduction)</li><li>[RecommendationsAPIドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[次の「Recommendationsカタログの管理」>](manage-catalog.md)
