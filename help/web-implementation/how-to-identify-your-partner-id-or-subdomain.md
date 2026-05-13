---
title: パートナーIDまたはサブドメインの特定方法
description: Experience Cloudの一部の機能を実装する際にパートナーIDまたはサブドメインを特定する方法と、Audience Manager UIでこのIDを取得できる2つの場所について説明します。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
TQID: https://experienceleague.adobe.com/ZObq0VjU8IEiaQSL4emTTRXdTgBvjiDfzztHFc7rO-g
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2:
  - id: a8b0238e-1d43-4679-a3b4-5ba1bad83baa
subfeature_v2:
  - id: f0bb1502-9f96-4d5e-a596-06876fe34ea0
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 316
ht-degree: 0%

---

# Audience Manager サブドメインの特定方法 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Experience Cloudの一部の機能を実装する場合は、Audience Manager `Subdomain`が何であるかを把握する必要があります（`client ID`または`Partner ID`とも呼ばれます）。 このビデオでは、Audience Manager UIでこの情報を取得できる2つの場所について説明します。

## 結末を捨てて・・・。 {#giving-away-the-ending}

この短いビデオを見ずに飛び込んで見つけたい場合は、UIの2つの場所で`Partner Subdomain`を見つけることができます。

1. 既に[!UICONTROL rule-based]特性を作成している場合は、をクリックします **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL]は、そのフォルダー内の特性のリストの特性の横にあり、URLはURLにサブドメインを含みます。
1. **[!UICONTROL Tools]** > **[!UICONTROL Tags]** インターフェイスに移動し、コンテナの&#x200B;**[!UICONTROL Get code]**&#x200B;をクリックした場合、サブドメインはAkamai行の末尾にあります

これらのクイックリファレンスですぐに見つけることができない場合、ビデオは短時間のコミットメントです。 :)

>[!VIDEO](https://video.tv.adobe.com/v/40889/?captions=jpn&quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloudの各顧客に割り当てられた数値IDがあり、これは「PID」またはパートナーIDと呼ばれることがよくあります。 これは、この記事とビデオで説明しているIDではありません。 代わりに、パートナーIDと呼ばれることもある「パートナーサブドメイン」は、通常、クライアント名のバージョンであり、データが送信されるサーバーのサブドメインです。 例えば、会社が「Bob&#39;s Knobs」（もちろんdoorknobsなど）の場合、パートナーサブドメインは「bobsknobs」、「PID」は「12345」のようになります。 通常、PIDを知る必要はありませんが、Audience Managerの実装を設定するために、サブドメインを知っておくことが重要です。
>
