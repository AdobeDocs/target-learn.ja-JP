---
title: Android用Adobe Mobile Services SDK v4 のAdobe Target
description: Android用Adobe Mobile Services SDK v4 を使用したAdobe Targetは、既にAndroid Mobile Services SDK v4 を使用していて、Adobe Targetでアプリエクスペリエンスのパーソナライズを開始したいAdobe開発者にとって最適な出発点になります。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile, Overview
doc-type: tutorial
kt: 3040
exl-id: 20f8ed4f-a86d-4c5e-9296-71a93724caa3
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 2%

---

# Android用Adobe Mobile Services SDK v4 を使用したAdobe Target – 概要

_Android用Adobe Mobile Services SDK v4 を使用したAdobe Targetは_ 既にAndroid Mobile Services SDK v4 を使用していて、Adobe Targetでアプリエクスペリエンスのパーソナライズを開始したいAdobe開発者にとって最適な出発点になります。

レッスンを完了するためのデモ用Android アプリが提供されています。 このチュートリアルを完了すると、独自のAndroid アプリで [!DNL Target] の実装を開始する準備が整います。

このチュートリアルでは、以下の内容について学習します。

* [Adobe Mobile Services SDK](https://experienceleague.adobe.com/docs/mobile-services/android/getting-started-android/requirements.html?lang=ja) 設定の検証
* 次のタイプの [!DNL Target] リクエストを実装します。
   * [!DNL Target] コンテンツのプリフェッチ
   * 1 回のリクエストで複数の [!DNL Target] ロケーション（mbox）をバッチ処理する
   * リクエストのブロック （アプリを表示する前に実行）
   * ノンブロックリクエスト（バックグラウンドで実行）
   * リアルタイム（非キャッシュ）
   * キャッシュ無効化の再取得
* 拡張パーソナライゼーションのリクエストへのパラメーターの追加
* オーディエンスとオファーの作成
* レイアウトのパーソナライズ
* 機能フラグ付きの新機能のロールアウト

## 前提条件

これらのレッスンでは、次のことを前提としています。

* Adobe ID と承認者レベルでAdobe Target インターフェイスにアクセスできる（以下の検証手順を参照）
* 自分のアカウントにリクエストを送信できるように、Adobe Targetのクライアントコードを把握します。 クライアントコードは、のAdobe Target インターフェイスに表示されます   設定/実装/ at.js 設定を編集画面
* [Mobile Services ユーザーインターフェイス &#x200B;](https://mobilemarketing.adobe.com/) にアクセスし、精通している
* Android モバイルアプリ開発用の IDE がある。 このチュートリアルでは、[Android Studio](https://developer.android.com/studio/install) を様々な手順とスクリーンショットで紹介します

Experience Cloud ソリューションへの必要なアクセス権がない場合は、Experience Cloud管理者にお問い合わせください。

また、Java でのAndroid開発に精通していることを前提としています。 このレッスンを完了するために Java の専門家である必要はありませんが、コードを快適に読んで理解できれば、より多くの知識を得ることができます。

### Adobe Targetへのアクセスの確認

このレッスンでは、Adobe Targetにアクセスする必要があります。 次の手順に進む前に、次の操作を実行してAdobe Targetにアクセスできることを確認します。

1. [Adobe Experience Cloud](https://experience.adobe.com/) にログインします
1. Experience Cloudのホーム画面で、[!DNL Target] をクリックします。
   ![Experience Cloud ホーム画面 &#x200B;](assets/aec_homeScreen_clickTarget.png)
1. 以下の図に示すように、Adobe Targetのアクティビティリストに移動すると、ユーザーが承認者レベルのアクセス権を持っていることが確認できます。 [!DNL Target] にアクセスできない場合や、承認者レベルのアクセスを確認できない場合は、会社のExperience Cloud管理者の 1 人に問い合わせて、このアクセスをリクエストし、付与されたら、このチュートリアルを再開してください。

   ![Adobe UI](assets/targetUI_approver.png)

## レッスンについて

これらのレッスンでは、Adobe Targetを、独自のAdobe Target アカウントを使用した「We.Travel」というデモ用の旅行アプリに実装します。 このチュートリアルを終了すると、アプリの使用状況に基づいて、パーソナライズされたメッセージをユーザーに配信できるようになります。 最終的なパーソナライゼーションエクスペリエンスは次のようになります。

![We.Travel アプリの最終版 &#x200B;](assets/overview_final_result.jpg)

We.Travel アプリ内での実装を確認した後、独自のモバイルアプリで [!DNL Target] を使用できるようになります。

それでは、始めましょう。

**[次へ：「サンプルアプリをダウンロードして更新する」 >](download-and-update-the-sample-app.md)**
