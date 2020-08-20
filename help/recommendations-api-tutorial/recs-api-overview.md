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
solution: Adobe Target
author: Judy Kim
translation-type: tm+mt
source-git-commit: b0e36ff68732f79c61797181da781ec7401f3f84
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 1%

---


# Adobe RecommendationsAPIの概要

関連するAPIに [!DNL Recommendations] は [管理者APIが含まれ](https://docs.adobe.com/content/help/en/target/using/apis/api-overview.html) 、次のことができます。

* レコメンデーション可能な商品またはコンテンツのカタログの管理
* アルゴリズムと [!DNL Recommendations] アクティビティの管理

Recommendationsで [!DNL Target] 配信API [](https://docs.adobe.com/content/help/en/target/using/apis/api-overview.html) を使用すると、次のこともできます。

* レコメンデーションをJSON、HTMLまたはXMLオブジェクトで取得し、Web、モバイル、電子メール、Internet of Things(IOT)などのチャネルで表示できるようにします。

## チュートリアルの説明

このチュートリアルでは、APIを使用したカタログとカスタム条件の設定と管理、および配信APIを使用したレコメンデーションコンテンツの取得に関する実践、開発者向けの [!DNL Recommendations][!DNL Recommendations] 練習について説明します。 このチュートリアルを終了すると、次のことが可能になります。

* RecommendationsAPIを使用したエンティティの設定と管理
* RecommendationsAPIを使用したカスタム条件の設定と管理
* 配信APIでRecommendationsを使用してrecommendationsを使用する方法を理解すると、HTML以外のデバイスに結果をもたらします。

## オーディエンス

このチュートリアルは、ターゲットAPIまたはRecommendationsAPIを初めて使用する開発者を対象としています。

## 前提条件

ターゲット管理APIを使用するには、 [Adobe認証の設定が必要です](../apis/configure-io-target-integration.md)。 このチュートリアルを開始する前に、設定が完了していることを確認してください。

## リソース

このチュートリアルを理解し、問題なく実行するために必要な以下のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| 郵便配達人 | ご使用のオペレーティングシステムの [Postmanアプリを取得します](https://www.postman.com/downloads/) 。 Postman Basicはアカウントの作成で自由です。 一般的にAdobe TargetAPIを使用するために必須ではありませんが、PostmanはAPIワークフローを容易にし、Adobe TargetはAPIの実行や動作の学習に役立ついくつかのポストマンコレクションを提供しています。 このチュートリアルの残りの部分では、Postmanの作業知識を前提としています。 援助が必要な場合は、 [ポストマンのドキュメントを参照してください](https://learning.getpostman.com/)。 |
| リファレンス | このチュートリアルの残りの部分では、次のリソースに精通していることを前提としています。<UL><li>[AdobeI/O Github](https://github.com/adobeio)</li><li>[ターゲットAdobeI/Oドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[RecommendationsAPIドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[次の「Recommendationsカタログの管理」>](manage-catalog.md)
