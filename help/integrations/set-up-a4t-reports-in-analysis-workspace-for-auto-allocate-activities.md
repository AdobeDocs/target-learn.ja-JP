---
title: ' [!DNL Analysis Workspace]  での[!UICONTROL 自動配分]アクティビティ用 A4T レポートの設定方法'
description: 設定方法 [!UICONTROL Analytics for Target] (A4T) レポート： [!DNL Adobe] [!DNL Analysis Workspace] 実行時 [!UICONTROL 自動配分] アクティビティ。
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 7d53adce-cc05-4754-9369-9cc1763a9450
source-git-commit: 352f334e2ca8c1d0be3ff0f89482b97500685174
workflow-type: tm+mt
source-wordcount: '1545'
ht-degree: 3%

---

# [!DNL Analysis Workspace] での [!DNL Auto-Allocate] アクティビティ用 A4T レポートの設定

An [!UICONTROL 自動配分] アクティビティ [!DNL Adobe Target] は、2 つ以上のエクスペリエンスの中から勝者を特定し、テストによる学習を続ける間、訪問者のトラフィックを自動的にその勝者に配分します。 The [!UICONTROL Analytics for Target] (A4T) 統合： [!UICONTROL 自動配分] でレポートデータを表示できます。 [!DNL Adobe Analytics]を使用して、カスタムイベントや指標に対してを最適化することができます。 [!DNL Analytics].

豊富な分析機能は [!DNL Adobe Analytics] [!DNL Analysis Workspace]（デフォルトに対するいくつかの変更） [!UICONTROL Analytics for Target] パネルが正しく解釈するために必要になる場合があります [!UICONTROL 自動配分] アクティビティ。 これらの変更は、 [最適化指標の基準](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#supported){target=_blank}.

各タイプの最適化指標では、次のように A4T で異なるレポート設定が必要です。

* の使用 [!DNL Analytics] 指標

   * [!UICONTROL 訪問者あたりの指標値を最大化]
   * [!UICONTROL 個別訪問者コンバージョン率を最大化する]

* の使用 [!DNL Target] — 定義されたコンバージョン指標

このチュートリアルでは、A4T の全体的なガイダンスと、条件に固有のレポート設定手順について説明します。

## 「」を使用した Analytics 指標[!UICONTROL 訪問者あたりの指標値の最大化]&#39;&#39;最適化基準

**定義**: （指標全体の値） / （訪問者数）

レポートを設定するには、A4T レポートで次の変更をおこないます。

| 変更が必要です | [!DNL Target] — トリガーされたレポート | A4T パネルレポート |
| --- | --- | --- |
| の指標値を最大化 [!DNL Analytics] 指標 | <ul><li>[!UICONTROL 信頼性] 指標を削除する必要があります。</li><li>[!UICONTROL 上昇率（低）] および [!UICONTROL 上昇率（高）] を削除する必要があります。</li><li>次の割合のプレゼンテーションをオフにします： [!UICONTROL コンバージョン率] 列を使用して混乱を避けます。 詳しくは、 [全体的なガイダンス](#guidance) 下</li><li>コンバージョン率指標は、「指標/訪問者」に名前を変更する必要があります。</li></ul> | <ul><li>[!UICONTROL 信頼性] 指標を削除する必要があります。</li><li>[!UICONTROL 上昇率（低）] および [!UICONTROL 上昇率（高）] を削除する必要があります。</li><li>次の割合のプレゼンテーションをオフにします： [!UICONTROL コンバージョン率] 列を使用して混乱を避けます。 詳しくは、 [全体的なガイダンス](#guidance) 下</li><li>コンバージョン率指標は、「指標/訪問者」に名前を変更する必要があります。</li><li>日付と時間範囲が、 [!DNL Target] レポート。 詳しくは、 [全体的なガイダンス](#guidance) 下</li></ul> |

![売上高の指標値を最大化](/help/integrations/assets/maximize-metric-value-revenue.png)

## [!DNL Analytics] 「」を含む指標[!UICONTROL 個別訪問者コンバージョン率]&#39;&#39;最適化基準

**定義**: （指標の正の値を持つ個別訪問者の数） / （個別訪問者の合計数）

例：最適化指標が [!UICONTROL 売上高]. アクティビティには 5 人の個別訪問者があり、そのうち 3 人が購入します。 この例では、値は（次のユーザーの 3 人の訪問者）になります。 [!UICONTROL 売上高] が正の場合 ) / （5 人の合計ユニーク訪問者） = 0.6 = 60%

>[!NOTE]
>
>ここで参照しているコンバージョン率は、クリック数、インプレッション数など、注文以外のアクションを指す場合があります。 この場合、条件は引き続き、クリックまたはページを表示した訪問者の数を最大化することになります。

レポートを設定するには、A4T レポートで次の変更をおこないます。

| 変更が必要です | ターゲットトリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| のコンバージョンを最大化します。 [!DNL Analytics] 指標 | <ul><li>[!UICONTROL 信頼性] 指標を削除する必要があります。</li><li>すべて [!UICONTROL 上昇率] 指標を削除する必要があります。</li><li>次の割合のプレゼンテーションをオフにします： [!UICONTROL コンバージョン率] 列を使用して混乱を避けます。 詳しくは、 [全体的なガイダンス](#guidance) 下</li></ul> | <ul><li>[!UICONTROL 信頼性] 指標を削除する必要があります。</li><li>すべて [!UICONTROL 上昇率] 指標を削除する必要があります。</li><li>セグメントを作成して、分析されたアクティビティを表示した正の指標値で訪問者をフィルタリングします。 詳しくは、 [セグメントの作成](#segment) 下</li><li>自動入力を置き換える [!UICONTROL コンバージョン率] ～間の除算になるように指標 [!UICONTROL 実訪問者数] と共に使用します。 詳しくは、 [コンバージョン率指標の更新](#update-conversion-metric) 下</li><li>次の割合のプレゼンテーションをオフにします： [!UICONTROL コンバージョン率] 列を使用して混乱を避けます。 詳しくは、 [全体的なガイダンス](#guidance) 下</li><li>日付と時間範囲が、 [!DNL Target] レポート。 詳しくは、 [全体的なガイダンス](#guidance) 下</li></ul> |

### デフォルトの A4T パネルレポート — 追加のガイダンス

以下の節では、デフォルトの A4T パネルレポートを設定する際の追加のガイダンスについて詳しく説明します。

#### セグメントの作成 {#segment}

1. 次をクリック： **&quot;+&quot;記号** 次の **[!UICONTROL セグメント]** をクリックします。

   ![左パネルのセグメントの横にあるプラス記号。](/help/integrations/assets/plus-sign.png)

1. セグメントに「正の指標値を持つ訪問者」というタイトルを付けます。
1. の下 **[!UICONTROL 定義]**（横） **[!UICONTROL 次を含む]**&#x200B;を選択します。 **[!UICONTROL 訪問者]**.
1. の下 **[!UICONTROL 定義]**」で、アクティビティの最適化指標を選択します。

   この例では、次のように仮定します。 [!UICONTROL 売上高] を最適化指標として使用します。

1. 「[!UICONTROL が次の値より大きい]&quot;演算子を使用して、&quot;0&quot;を指定します。

   これらの設定では、正の指標値を持つすべての訪問者がフィルタリングされます。

1. 「**[!UICONTROL 保存]**」をクリックします。

   ![正の指標値](/help/integrations/assets/positive-metric-value.png)

1. A4T パネルに、新しく作成した「Visitors with positive metric value」という名前のセグメントを追加します。
1. 次をドラッグ&amp;ドロップ： [!UICONTROL 実訪問者数] 指標が「正の指標値を持つ訪問者」と同じ列に表示されます。

   この設定により、指標値が正の数を示すすべての個別訪問者のセグメントが作成されます。 この例では、売上高が 0 より大きいすべての個別訪問者数が表示されます。

#### を更新します。 [!UICONTROL コンバージョン率] 指標 {#update-conversion-metric}

1. まだ削除していない場合は、既存の [!UICONTROL コンバージョン率] 」列を使用して選択できます。
1. 指標を追加するには、 **[!UICONTROL 指標]** セクションをクリックします。
1. 指標に「コンバージョン率」という名前を付け、「([!UICONTROL 実訪問者数] （正の指標値を含む）」を「個別訪問者数」で割った値です（下図を参照）。

   「正の指標値を持つ訪問者」の新しく作成したセグメント（以下で定義する手順）、除算演算子、分子内の「個別訪問者」指標、分母として「個別訪問者」を追加します。

   ![A4T パネルのコンバージョン率。](/help/integrations/assets/conversion-rate.png)

1. 「**[!UICONTROL 保存]**」をクリックします。

1. 新しく作成した「コンバージョン率」指標を既存のパネルにドラッグ&amp;ドロップします。
1. 歯車アイコンをクリックし、「 **[!UICONTROL 割合]** 」チェックボックスにチェックマークを付けます。この値は混乱を招く可能性があります。

   レポートの正しい設定では、次の図のような結果が得られます。

   ![A4T パネルレポートの個別訪問コンバージョン率](/help/integrations/assets/a4t-aa-maximize-metric-value-revenue.png)

## [!DNL Target] — 定義されたコンバージョン率

レポートを設定するには、A4T レポートで次の変更をおこないます。

| 変更が必要です | ターゲットトリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| [!DNL Analytics] レポート， [!DNL Target] コンバージョン指標 | <ul><li>[!UICONTROL 信頼性] 指標を削除する必要があります。</li><li>[!UICONTROL 上昇率（低）] および [!UICONTROL 上昇率（高）] を削除する必要があります。</li><li>次の割合のプレゼンテーションをオフにします： [!UICONTROL コンバージョン率] 列を使用して混乱を避けます。 詳しくは、 [全体的なガイダンス](#guidance) 下</li></ul> | <ul><li>[!UICONTROL 信頼性] 指標を削除する必要があります。</li><li>[!UICONTROL 上昇率（低）] および [!UICONTROL 上昇率（高）] を削除する必要があります。</li><li>次の割合のプレゼンテーションをオフにします： [!UICONTROL コンバージョン率] 列を使用して混乱を避けます。 詳しくは、 [全体的なガイダンス](#guidance) 下</li><li>日付と時間範囲が、 [!DNL Target] レポート。 詳しくは、 [全体的なガイダンス](#guidance) 下</li></ul> |

レポートの正しい設定では、次の図のような結果が得られます。

![アクティビティコンバージョン](/help/integrations/assets/optimized-table.png)

## の全体的なガイダンス [!UICONTROL Analytics for Target] (A4T) {#guidance}

事前定義済みの [!UICONTROL Analytics for Target] パネルで [!UICONTROL Adobe Target] ( これは、このガイドで後述します。[!DNL Target]-triggered report」) を参照してください。 または、 [!DNL Analytics] （詳しくは、この節を参照）。

次の節では、選択した方法に応じて、必要な設定を指定します。 ただし、以下の手順は全体的なガイダンスとして機能します。

* パネルの作成方法に関係なく、信頼性指標を A4T パネルから削除する必要があります（両方について詳しくは、以下を参照）。 代わりに、[!DNL Target] レポートでこれらの値を参照してください。さらに、アクティビティの勝者は、 [!DNL Target] レポート。 アクティビティの勝者の特定について詳しくは、 [アクティビティの勝者の特定](#winner) 」の節を参照してください。
>>
* 混乱を避けるには、「[!UICONTROL 割合]」の表示 [!UICONTROL コンバージョン率] 指標。 詳しくは、 [割合を [!UICONTROL コンバージョン率] 列](#hide-percentage) 下
>>
* A4T パネルを作成する場合は、日付と時間の範囲が [!DNL Target] レポート。 詳しくは、 [A4T パネルでの日時の整列](#aligning-date-and-time) 下

### 割合を [!UICONTROL コンバージョン率] 列 {#hide-percentage}

1. 次をクリック： **ギア** アイコン [!UICONTROL コンバージョン率] 列。

   ![コンバージョン率列の歯車アイコン](/help/integrations/assets/coversion-rate-gear-icon.png)

   The [!UICONTROL 列] [ 設定 ] ダイアログボックスには、次の項目が表示されます。

   ![[ 列設定 ] ダイアログボックス](/help/integrations/assets/column-settings-dialog-box.png){width="200"}

1. 選択を解除すると、 **[!UICONTROL 割合]** チェックボックス。

   A4T パネルで、コンバージョン率と一致する割合が表示されなくなりました。 [!DNL Target]、以下に示すように。

   ![コンバージョン率列に割合が表示されない](/help/integrations/assets/no-percentages.png)

### A4T パネルでの日時の整列 {#aligning-date-and-time}

1. 各パネルの下で、パネルが参照する日付範囲が [!DNL Target] レポート。

   ![A4T パネルの日付範囲](/help/integrations/assets/date-range.png)

1. In [!DNL Analytics]の場合、時間範囲を午前 12:00～午後 11:59 に設定します。

### アクティビティの勝者の特定 {#winner}

[!DNL Auto-Allocate] アクティビティの勝者は、信頼性の値が 95%以上の勝者コンバージョン率がある場合に選択されます。 これらの値は、 [!DNL Target] 信頼性の計算は、より保守的な方法を反映しているので、レポート [!DNL Target] が次の値を推奨： [!UICONTROL 自動配分] アクティビティ。 詳しくは、 [自動配分の統計的保証](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/determine-winner.html#section_7AF3B93E90BA4B80BC9FC4783B6A389C){target=_blank} （内） *[!UICONTROL Adobe Target Business Practioner ガイド]*.

>[!NOTE]
>
「まだ勝者がありません」と「勝者」のバッジは、A4T パネル ( [!DNL Analysis Workspace]. また、 [!DNL Target] レポート： [!UICONTROL 自動配分] アクティビティは無視する必要があります。 詳しくは、 [自動配分](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=en#aa){target=_blank} in *自動配分と自動ターゲットアクティビティに対する A4T のサポート* （内） *[!UICONTROL Adobe Target Business Practioner ガイド]*.

### 用の A4T の作成 [!UICONTROL 自動配分] パネル内 [!DNL Analysis Workspace]

1. 用の A4T パネルを作成するには、以下を実行します。 [!UICONTROL 自動配分] アクティビティレポートを開始するには、 [!UICONTROL Analytics for Target] パネル内 [!DNL Analysis Workspace]、以下に示すように。

   ![Analytics for Target — 自動配分レポート](/help/integrations/assets/a4t-auto-allocate-report.png)

1. 次の選択を行います。

   * **[!UICONTROL コントロールエクスペリエンス]**：任意のエクスペリエンスを選択します。
   * **[!UICONTROL 指標の標準化]**：を選択します。 **[!UICONTROL 訪問者]** （デフォルトで A4T パネルに含まれています）。 [!UICONTROL 自動配分] は常に個別訪問者ごとにコンバージョン率を標準化します。
   * **成功指標**：アクティビティの作成時に使用したのと同じ（最適化）指標を選択します。 これが [!DNL Target] — 定義済みのコンバージョン指標、「 **[!UICONTROL アクティビティコンバージョン]**. それ以外の場合は、使用した [!DNL Adobe Analytics] 指標を選択します。









