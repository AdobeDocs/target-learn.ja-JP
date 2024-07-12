---
title: Adobe Recommendations API とは
description: このチュートリアルでは、Adobe Target Recommendations API を使用してRecommendations カタログとカスタム条件を設定および管理したり、配信 API を使用してレコメンデーションコンテンツを取得したりする実践的な方法について、開発者に順を追って説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 10f80056-fb71-4362-86bc-d161f596cb91
source-git-commit: 542ff406fc24df54a2f1b007422492341ea46507
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 2%

---

# Adobe Recommendations API の概要

[!DNL Recommendations] に関連する API には、次の操作を可能にする [ 管理 API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en) が含まれます。

* お勧めの製品やコンテンツのカタログを管理する
* [!DNL Recommendations] のアルゴリズムとアクティビティの管理

Recommendationsで [!DNL Target] [ 配信 API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en) を使用すると、次のことも可能です。

* Web、モバイル、メール、Internet of Things （IOT）などのチャネルに表示できるように、レコメンデーションを JSON、HTML、XML オブジェクトで取得します。

## チュートリアルの説明

このチュートリアルでは、[!DNL Recommendations] API を使用して [!DNL Recommendations] カタログとカスタム条件を設定および管理したり、配信 API を使用してレコメンデーションコンテンツを取得したりするなど、開発者に対する実践的なプラクティスについて説明します。 このチュートリアルを終了すると、次の操作を実行できるようになります。

* Recommendations API を使用したエンティティの設定と管理
* Recommendations API を使用したカスタム条件の設定と管理
* Recommendationsと配信 API を使用して、レコメンデーションの結果をHTML以外のデバイスで使用する方法を説明します

## オーディエンス

このチュートリアルは、Target API またはRecommendations API を初めて使用する開発者向けです。

## 前提条件

Target 管理 API を使用するには、[Adobe認証のセットアップ ](https://experienceleague.adobe.com/docs/target-dev/developer/api/configure-authentication.html?lang=ja){target="_blank"} が必要です。 このチュートリアルを開始する前に、このが設定されていることを確認してください。

## リソース

このチュートリアルを理解し、正しく実行するために必要な、次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| Postman | お使いのオペレーティングシステム用の ](https://www.postman.com/downloads/)0}Postman アプリ } を入手します。 [Postman basic は無料でアカウントを作成できます。 Adobe Target API を一般的に使用するために必要なものではありませんが、Postmanを使用すると API ワークフローが容易になり、Adobe Targetには API の実行と操作方法の学習に役立つPostman コレクションが複数用意されています。 このチュートリアルの残りの部分では、Postmanに関する実務知識を前提としています。 サポートについては、[Postmanのドキュメント ](https://learning.getpostman.com/) を参照してください。 |
| 参照 | このチュートリアルの残りの期間、次のリソースに慣れていることを前提としています。<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[TargetAdobe I/Oドキュメント ](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations API ドキュメント ](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[ 次へ「Recommendations カタログの管理」 >](https://experienceleague.adobe.com/docs/target-dev/developer/api/recommendations-api/manage-catalog.html){target="_blank"}
