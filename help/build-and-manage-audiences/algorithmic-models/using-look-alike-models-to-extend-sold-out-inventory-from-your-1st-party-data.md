---
title: 類似モデルを使用して、ファーストパーティデータから売り切れ在庫を拡張します
description: このチュートリアルでは、類似モデルを設定および使用するために必要な手順を説明します。これにより、新しい類似オーディエンスを作成し、コンバージョンセグメントの拡張機能として販売できます。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# 類似モデルを使用して、ファーストパーティデータから売り切れ在庫を拡張します {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

このチュートリアルでは、類似オーディエンスを設定および使用するために必要な手順を説明します。これにより、新しい類似オーディ [!UICONTROL Models] ンスを作成し、コンバージョンセグメントの拡張機能として販売できます。

## ユースケースの詳細 {#use-case-details}

コンテンツパブリッシャーです。 サイトのコンバーターの在庫を既に売り切っている場合は、オポチュニティがそこで終わったと思うかもしれません。 AAMの類似 [!UICONTROL Models] を入力します。 この機能を使用すると、売り切れた在庫をさらに拡大したり、まだコンバージョンに至っていない可能性があるがコンバージョンに至った人物のように見える/行動する人物のオーディエンスを販売したりできます。 このオーディエンスセグメントは、通常、実際のコンバーターよりも販売が少なくなりますが、それでも、サイトに広告を掲載したい広告主に追加のオーディエンスオプションを提供することで、収益に追加できます。 このユースケースのもう 1 つの利点は、ファーストパーティデータでこのモデルを実行するのに何のコストもかからないことです。

このチュートリアルの手順は次のとおりです。

1. 理想的なユーザー（コンバージョン）特性またはセグメントの特定/作成
1. このコンバージョン特性/セグメントをベースアイテムとして使用して、モデルを作成します
1. モデル [!UICONTROL First party] データソースを選択し、モデルを実行します
1. モデル結果から [!UICONTROL Algorithmic Trait] を作成し、セグメントに特性を追加します。
1. コンバージョンセグメントの売上を伸ばすために、関心のある広告主にセグメントを提供

## 理想的なユーザー（コンバージョン）特性やセグメントの特定または作成 {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイトでユーザーに何を実行させるつもりですか？ コンバージョンイベントとは何ですか？ もちろん、この質問に対する回答は、サイトのタイプ/業種や組織の目標に応じて様々です。 いずれにせよ、AAMでは、これらの条件を満たした訪問者に対して特性を作成するのが一般的です。

このユースケースでは、コンバーターのユーザーの在庫が売り切れているので、既にこれを想定しています。 ただし、このチュートリアルの目的では、残りのユースケースの参照用として説明することをお勧めします。

また、特性を作成するためにイベントを使用する場合、必要以上のユーザーを特性に収集しないように、注意する必要がある重要な問題があります。 詳しくは、次のビデオをご覧ください。 :）

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上記のビデオでは、以下に示す例は、Adobe Analyticsを持っていることを前提としています。 明らかに、これは当てはまらない可能性があります。 Google Analytics（GA）がある場合、AAMにデータを送信するために使用できるモジュールがあります（[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja) を参照）。サイト上のコンバージョンアクティビティが GA によってAAMに送信される場合は、そこからコンバージョン特性を作成できます。 別の分析ソリューションがある（または分析ソリューションがない）場合でも、DIL コードや `submit` 関数などを介してAAMにデータを送信できます。 （[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=ja)）を参照してください。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいて、コンバージョン特性を作成します。

## ファーストパーティデータからの類似モデルの作成 {#creating-a-look-alike-model-from-first-party-data}

この手順では、[!UICONTROL First Party] しい類似モデルを作成します。 つまり、ベースとなる特性/セグメントにファーストパーティコンバージョン特性/セグメントを使用するだけでなく（これはほとんどのモデルでは正常です）、コンバータのように見える人のためにファーストパーティデータのプールを調べるだけです。 サードパーティのデータは表示されません。

このユースケースでは、これが重要です。なぜなら、私たちは、サイト上にコンバーターに似ていてもまだコンバージョンに至っていないユーザーのセグメントを作成しようとしているからです。そうすることで、興味のある広告主にこの類似セグメントを販売できます。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## アルゴリズム特性の作成 {#creating-an-algorithmic-trait}

次に、モデルの結果を使用できるように、[!UICONTROL Algorithmic Trait] を作成する必要があります。 特性を作成しないと、モデルは役に立ちません。 モデルを実行したら、必ず特性ダイアログに移動して [!UICONTROL Algorithmic Trait] を作成してください。 次のビデオでは、順を追って説明し、いくつかのヒントを示します。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 広告主に [!UICONTROL Algorithmic Segment] を提供する {#offering-the-algorithmic-segment-to-advertisers}

[!UICONTROL Algorithmic Trait][!UICONTROL Algorithmic Trait] ータを作成したら、新しいセグメントを作成して配置し、データをアクティブ化できます（特性をアクティブ化することはできませんが、特性を含んだ新しい単一特性セグメントを作成することで、セグメントをアクティブ化（使用）できます）。

類似モデルで高いスコアを付けたファーストパーティ訪問者（つまり、コンバーターのように見えても、まだコンバージョンに至っていない訪問者）のセグメントを作成したら、サイト上の実際のコンバーターの在庫をすべて売り切った後でも、このセグメントをサイトの広告主に提供できます。 これは、Audience Managerの類似 [!UICONTROL Models] を使用して、オーディエンスを拡張し、さらに売上高を増やす優れた方法です。
