---
title: Adobe Recommendations APIとは何ですか？
description: このチュートリアルでは、Adobe Target Recommendations APIを使用して、Recommendations カタログとカスタム条件を設定および管理し、配信APIを使用してRecommendations コンテンツを取得する実践的な方法について開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 10f80056-fb71-4362-86bc-d161f596cb91
TQID: https://experienceleague.adobe.com/NQpsNnhLA0MRP-pJQLS35ymJ2lnulZebvI-Yv4-xSxw
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: e0eb8757-182f-49f3-94a4-1587d16f5094id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: c0b4abf2d4ead4d58a8db6e8970857b7b50dbe5c
workflow-type: tm+mt
source-wordcount: 385
ht-degree: 5%

---

# Adobe Recommendations APIの概要

[!DNL Recommendations]に関連するAPIには、[管理者API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en)が含まれており、次のことが可能です。

* 商品カタログやコンテンツのレコメンデーションを管理
* [!DNL Recommendations]のアルゴリズムとアクティビティの管理

Recommendationsで[!DNL Target] [配信API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en)を使用すると、次の操作も実行できます。

* レコメンデーションをJSON、HTML、XML オブジェクトで取得し、web、モバイル、電子メール、IOT （モノのインターネット）などのチャネルで表示できます。

## チュートリアルの説明

このチュートリアルでは、[!DNL Recommendations] APIを使用して[!DNL Recommendations] カタログとカスタム条件を設定および管理し、配信APIを使用してレコメンデーションコンテンツを取得する実践的な方法を開発者に説明します。 このチュートリアルの終わりまでに、次のことが可能になります。

* Recommendations APIを使用したエンティティの設定と管理
* Recommendations APIを使用したカスタム条件の設定と管理
* HTML以外のデバイスでRecommendations APIを使用してRecommendationsの結果を使用する方法について説明します

## オーディエンス

このチュートリアルは、Target APIまたはRecommendations APIを初めて使用する開発者向けです。

## 前提条件

Target管理APIを使用するには、[Adobe認証の設定](https://experienceleague.adobe.com/docs/target-dev/developer/api/configure-authentication.html?lang=ja){target="_blank"}が必要です。 このチュートリアルを開始する前に、必ずこの設定を行ってください。

## リソース

このチュートリアルを理解し、それに従うために必要な次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| Postman | お使いのオペレーティング システム用の[Postman アプリ ](https://www.postman.com/downloads/)を入手します。 Postman basicはアカウント作成機能を無料で利用できます。 Adobe Target APIを一般的に使用する場合は不要ですが、PostmanではAPI ワークフローが簡単になり、Adobe TargetにはAPIの実行と動作の学習に役立つPostman コレクションがいくつか用意されています。 このチュートリアルの残りの部分では、Postmanに関する実務的な知識を前提としています。 サポートが必要な場合は、[Postman ドキュメント ](https://learning.getpostman.com/)を参照してください。 |
| 参照 | このチュートリアルの残りの部分では、次のリソースに精通していることを前提としています。<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Target Adobe I/O ドキュメント ](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations API ドキュメント ](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[次の「レコメンデーションカタログの管理」 >](https://experienceleague.adobe.com/docs/target-dev/developer/api/recommendations-api/manage-catalog.html){target="_blank"}
