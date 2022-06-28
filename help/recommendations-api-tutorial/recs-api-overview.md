---
title: Adobe Recommendations API とは
description: このチュートリアルでは、Adobe Target Recommendations API を使用してRecommendationsカタログとカスタム条件の設定と管理を行い、Delivery API を使用してレコメンデーションコンテンツを取得する実践を、開発者に対して順を追って説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 10f80056-fb71-4362-86bc-d161f596cb91
source-git-commit: cee2618bb92284da1f82d108a0aff0d39340a15b
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 3%

---

# Adobe Recommendations API の概要

関連する API [!DNL Recommendations] 含める [管理 API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en) を使用して、次の操作を実行できます。

* レコメンデーション可能な製品またはコンテンツのカタログを管理
* を管理 [!DNL Recommendations] アルゴリズムとアクティビティ

の使用 [!DNL Target] [配信 API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en) Recommendationsでは、次のこともできます。

* JSON、HTML、または XML オブジェクトでレコメンデーションを取得して、Web、モバイル、E メール、Internet of Things(IOT)、その他のチャネルで表示できるようにします。

## チュートリアルの説明

このチュートリアルでは、 [!DNL Recommendations] 設定および管理する API [!DNL Recommendations] カタログとカスタム条件を作成し、delivery API を使用して recommendations コンテンツを取得する方法について説明します。 このチュートリアルを最後まで学習すると、次の内容を習得できます。

* Recommendations API を使用したエンティティの設定と管理
* Recommendations API を使用したカスタム条件の設定と管理
* 非HTMLデバイスでのレコメンデーション結果を使用するためにRecommendationsを Delivery API と共に使用する方法を理解する

## オーディエンス

このチュートリアルは、Target API またはRecommendations API を初めて使用する開発者を対象としています。

## 前提条件

Target 管理 API を使用するには、 [Adobe認証設定](https://developer.adobe.com/target/before-administer/configure-authentication/){target=&quot;_blank&quot;}。 このチュートリアルを開始する前に、この設定が完了していることを確認してください。

## リソース

このチュートリアルを理解し、それに従うために必要な、次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| Postman | を取得 [Postmanアプリ](https://www.postman.com/downloads/) ご使用のオペレーティングシステム用。 Postman basic は、アカウントの作成に自由に対応しています。 Adobe Target API を一般に使用するためには必須ではありませんが、Postmanでは API ワークフローが容易になります。Adobe Targetでは、API の実行とその動作方法の学習に役立つ、いくつかのPostmanコレクションが提供されています。 このチュートリアルの残りの部分は、Postmanの作業に関する知識を前提としています。 不明な点は、 [Postmanドキュメント](https://learning.getpostman.com/). |
| 参照 | このチュートリアルの残りの部分では、次のリソースに関する知識が必要となります。<UL><li>[Adobe I/OGithub](https://github.com/adobeio)</li><li>[TargetAdobe I/Oドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations API ドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[次：「Recommendationsカタログを管理」>](https://developer.adobe.com/target/before-administer/recs-api/manage-catalog/){target=&quot;_blank&quot;}
