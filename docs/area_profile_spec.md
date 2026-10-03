# Area Profile Specification

Version: 0.1

## 1. 目的

Area Profileは、不動産推薦に利用するために、駅・地域などのエリアが持つ特徴を
共通の特徴空間で表現することを目的とする。

Area Profileは、以下の3種類の特徴から構成する。

1. Functional Features
2. Affective Features
3. Character Tags

Area Profileは「どのような人に向いているか」を直接評価するものではない。

人のHousing NeedsとArea Profileとの対応付け、および推薦スコアの算出は、
推薦側（real-estate-value-ai）の責務とする。


## 2. Areaの単位

### 2.1 基本単位

Ver.0.1では、鉄道駅を中心とした生活圏・駅勢圏を基本的なArea単位とする。

駅そのものの施設や駅構内だけを評価するのではなく、
その駅を利用して生活する場合に一般的に認識される周辺地域を評価対象とする。


### 2.2 鉄道会社が異なる同名駅

鉄道会社が異なる場合は、同じ駅名であっても原則として別Areaとして扱う。

例：

- 阪急 御影
- 阪神 御影
- 阪急 春日野道
- 阪神 春日野道

これは、同名駅であっても駅勢圏や地域特性が異なる場合があるためである。


### 2.3 複数路線が乗り入れる同一駅

同一鉄道会社内で複数路線が乗り入れる同一駅は、
原則として1つのAreaとして扱い、複数路線への所属情報を持たせる。

例：

阪急 淡路
- 京都本線
- 千里線


### 2.4 将来のArea単位

将来的には以下のArea Typeを扱うことを想定する。

- station：駅勢圏
- named_area：一般的に認識される地域名称
- administrative_area：行政区域
- railway_corridor：沿線

Ver.0.1ではstationを中心に扱う。


## 3. Area Profileの構造

Area Profileは以下の3層から構成する。

### 3.1 Functional Features

エリアに「何があるか」「どの程度利用しやすいか」など、
比較的客観的・機能的な地域特性を表す。

Ver.0.1では24項目とする。

Functional Featuresの正式な項目定義は
`data/source/Definition.xlsx`
を正とする。


### 3.2 Affective Features

エリアから受ける感性的・経験的な印象を表す。

Ver.0.1では以下の8軸を使用する。

| ID | 軸 | 1 | 5 |
|---|---|---|---|
| A01 | 都市性 | ローカル | 都会的 |
| A02 | 街のスタイル | 庶民・生活文化型 | 洗練・上質型 |
| A03 | 活動性 | 静的・穏やか | 活気・刺激的 |
| A04 | 歴史・成熟性 | 新しい・新興 | 歴史・成熟 |
| A05 | 開放性 | 密集・路地的 | 開放的 |
| A06 | 文化性 | 実生活中心 | 文化的・知的 |
| A07 | 独自性 | 標準的 | 個性的 |
| A08 | 親密性 | 匿名的 | 親しみ・人情 |

各軸を1〜5で評価する。

3を中間とし、1と5は方向性を表す。

この尺度は優劣を表すものではない。


### 3.3 Character Tags

地域固有の歴史、文化、社会的背景、街のアイデンティティを表す。

Character Tagsは排他的分類ではなく、
1つのAreaに複数のタグを付与できる。

Ver.0.1では以下を基本語彙とする。

| ID | Character Tag |
|---|---|
| C01 | 邸宅・高級住宅地 |
| C02 | 文教 |
| C03 | 学生街 |
| C04 | 商店街 |
| C05 | 下町・庶民文化 |
| C06 | 歴史街・旧市街 |
| C07 | 門前町・宗教文化 |
| C08 | 芸術・文化拠点 |
| C09 | 地域産業・ものづくり文化 |
| C10 | スポーツ文化 |
| C11 | 港町・水辺文化 |
| C12 | 観光・非日常文化 |

Character TagsはControlled Vocabularyとして管理する。

既存タグでは表現できない地域特性が継続的に出現した場合には、
新しいタグの追加を検討する。

一時的・局所的な特徴のためにタグを無制限に増やさない。


## 4. Functional Featuresの評価

Functional Featuresの項目および基本的な評価尺度は、
`Definition.xlsx` の定義に従う。

Ver.0.1では基本的に以下の値を使用する。

- 2：高い、良好、多い、便利など
- 1：普通
- -1：低い、悪い、少ない、不便など
- 0：未評価・判断不能

各項目固有の意味はDefinition.xlsxを正とする。

Functional Featuresは可能な限り、
客観的な公開情報または確認可能な情報に基づいて評価する。


## 5. Affective Featuresの評価

Affective Featuresは、Functional Featuresだけでは表現できない
街の雰囲気や経験的特徴を表現する。

評価にあたっては、単一施設や一つの口コミだけではなく、
複数の情報から地域全体として認識される傾向を判断する。

評価値は1〜5とする。

Affective Featuresは主観的評価を含むため、
絶対的な事実ではなく、Area間の相対的な特徴を表すものとして扱う。


## 6. Character Tagsの付与

Character Tagは、地域の性格を説明するうえで意味のある
歴史的・文化的・社会的背景が存在する場合に付与する。

単に施設が存在するだけでは付与しない。

例えば大学が存在するだけでは「学生街」とせず、
学生の存在が地域の商業・文化・街の雰囲気に
一定の影響を与えている場合に「学生街」を付与する。

同様に、商店が存在するだけでは「商店街」とせず、
商店街や市場、個人商店などが地域の特徴として認識できる場合に付与する。

必要に応じて、タグに加えて具体的な内容を
`character_detail`
として保持する。

例：

- C09 地域産業・ものづくり文化
  - character_detail: 灘五郷・酒造文化

- C08 芸術・文化拠点
  - character_detail: 宝塚歌劇

- C10 スポーツ文化
  - character_detail: 阪神甲子園球場・高校野球


## 7. 情報源と根拠

Area Profile生成では、可能な限り複数の情報源を使用する。

情報源の優先順位は概ね以下とする。

1. 国・自治体等の公的情報
2. 鉄道会社等の公式情報
3. 公的・準公的な観光・地域情報
4. 地域に関する信頼できる公開情報
5. その他の補助的情報

単一の情報源のみから地域全体の性格を断定しない。

将来的にはArea単位またはFeature単位で、
以下を保持できる構造を検討する。

- source
- evidence
- evaluation_date
- confidence
- model/version


## 8. 未評価・不明情報の扱い

情報が不足している場合は推測によって値を埋めない。

特に以下のような項目は、
適切なデータソースなしに駅名や地域イメージだけから評価しない。

- ハザードリスク
- 防災体制
- 福祉環境
- 空気環境

Ver.0.1では判断できないFunctional Featureを
0（未評価）として扱う。

将来的には「中立」と「Unknown」を明確に分離することを検討する。


## 9. Area Master

生成したArea ProfileはArea Masterとして保存する。

Ver.0.1のArea Masterは、

`data/master/Area_Master_Ver0.1_阪急阪神.xlsx`

とする。

Area Masterには最低限以下を保持する。

- area_id
- company
- line
- station
- Functional Features
- Affective Features
- Character Tags
- character_detail
- evaluation_status
- confidence


## 10. Area Profile生成と更新

Area Profileは、一度生成した値を固定的な真実として扱わない。

以下の場合には再評価を行う。

- Feature定義が変更された
- 新しい情報源が利用可能になった
- 大規模開発等によって地域特性が変化した
- 評価ロジックが変更された
- 人によるレビューで修正が必要と判断された

Area Masterを更新する場合は、
既存バージョンを上書きするのではなく、
原則としてバージョンを更新する。


## 11. 責務の分離

`real-estate-profile-ai` は不動産・エリア側の特徴を記述する。

ここでは以下を行わない。

- 特定の人に向いているかの判定
- Housing Needsとの対応付け
- ユーザーの価値観とのマッチング
- 推薦順位の決定

これらは `real-estate-value-ai` 側で行う。

基本的な処理関係は以下とする。

Area側：

Definition
→ Area Profile生成
→ Area Master

推薦側：

Personal Values
→ Lifestyle Values
→ Housing Needs
→ Required Area Features
→ Area MasterとのMatching
→ Recommendation


## 12. Ver.0.1の位置づけ

Ver.0.1はPoCのための初期仕様である。

現時点では完全な客観性や全国対応を目標とせず、

「Area Profileという表現によって、
価値観・Housing Needsに基づく不動産推薦が成立するか」

を検証することを優先する。

阪急・阪神の対象駅を最初の検証データセットとし、
推薦結果を確認しながらFeature定義・評価方法を改善する。