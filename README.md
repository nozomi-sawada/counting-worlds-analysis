# Counting Worlds: Text Mining Analysis of Early Nigerian Newspapers

## 概要 / Overview

本プロジェクトは、1880年代のナイジェリア（ラゴス）で発行された初期新聞のテキストマイニング分析を行うPythonコードです。植民地時代における地理的表象、集団的アイデンティティ、言説パターンの変遷を計量的に分析します。

This project provides Python code for text mining analysis of early Nigerian newspapers published in Lagos during the 1880s. It quantitatively analyzes geographical representations, collective identity, and discourse pattern changes during the colonial period.

## 研究背景 / Research Background

### 分析対象新聞 / Target Newspapers
- **Lagos Observer (LO)** - 1882-1888
  - 社説 (Editorials)
  - 読者投稿欄 (Correspondences)
- **Lagos Weekly Record (LWR)** - 1891-1930s (本分析では初期データを使用)
  - 社説 (Editorials)

これらの新聞は、西アフリカにおける最初期の現地発行新聞であり、植民地公共圏の形成過程を理解する上で重要な史料です。

## 主な分析手法 / Analysis Methods

### 1. 基礎統計分析 / Basic Statistical Analysis
- 記事数の時系列推移
- 月別・年代別の記事分布
- 語彙の多様性指標

### 2. テキストマイニング / Text Mining
- **TF-IDF分析**: 重要語彙の抽出と時代別比較
- **トピックモデリング (LDA)**: 潜在的トピックの発見（8-10トピック）
- **ワードクラウド**: 視覚的な頻出語表現

### 3. 地理的表象分析 / Geographic Representation Analysis
- 地域カテゴリ別の言及頻度
  - Local (Lagos, Yoruba regions)
  - Regional (West Africa)
  - Continental (Africa)
  - Global (Europe, America, Asia)
- 地理的カテゴリの共起パターン分析

### 4. 代名詞使用分析 / Pronoun Usage Analysis
- "We/Our" vs "They/Their" の使用パターン
- 集団的アイデンティティの形成過程
- 代名詞と地理的カテゴリの共起関係

### 5. 植民地用語分析 / Colonial Terminology Analysis
- "Native" 用語の使用頻度と文脈
- 時代による用語使用の変化

### 6. 感情分析 / Sentiment Analysis
- TextBlobによる記事の感情傾向分析
- トピック別・時期別の感情変化

## 必要環境 / Requirements

```bash
pip install -r requirements.txt
```

主要ライブラリ:
- pandas, numpy (データ処理)
- matplotlib, seaborn (可視化)
- scikit-learn (機械学習)
- gensim (トピックモデリング)
- spacy, NLTK (自然言語処理)
- wordcloud (ワードクラウド生成)
- pyLDAvis (LDA可視化)

## 使用方法 / Usage

1. データファイルを `./data/` フォルダに配置
2. Jupyter Notebookを起動
```bash
jupyter notebook Counting_Worlds_cleaned.ipynb
```
3. セルを順番に実行

## クイックスタート / Quick Start

サンプルデータですぐに動作確認できます：

```bash
# デモノートブックを実行（言語を選択）
jupyter notebook demo_en.ipynb  # English version
jupyter notebook demo_ja.ipynb  # 日本語版
```

**Language Versions / 言語版について:**
- `demo_en.ipynb` - English version with detailed explanations of each cell's functionality
- `demo_ja.ipynb` - 日本語版、各セルの機能について詳しい説明付き
- Both notebooks contain identical functionality, only the language differs
- どちらのノートブックも同じ機能を持ち、言語のみが異なります

サンプルデータは `sample_data/` フォルダに含まれています：
- `sample_editorial.csv` - 社説データのサンプル（5件）
- `sample_correspondence.csv` - 読者投稿データのサンプル（5件）

## プロジェクト構造 / Project Structure

```
counting-worlds-analysis/
├── Counting_Worlds_cleaned.ipynb    # メイン分析ノートブック / Main analysis notebook
├── demo_en.ipynb                    # デモノートブック（英語版） / Demo (English)
├── demo_ja.ipynb                    # デモノートブック（日本語版） / Demo (Japanese)
├── README.md                        # プロジェクト説明 / Project documentation
├── requirements.txt                 # 必要パッケージ / Required packages
├── sample_data/                     # サンプルデータフォルダ / Sample data folder
│   ├── sample_editorial.csv         # 社説サンプル（5件） / Editorial samples (5)
│   ├── sample_correspondence.csv    # 読者投稿サンプル（5件） / Correspondence samples (5)
│   └── README.md                    # サンプルデータ説明 / Sample data documentation
└── images/                          # 分析結果画像 / Analysis result images
    ├── 1_article_counts_by_year.png
    ├── 2_lagos_observer_wordcloud.png
    ├── 3_datasets_comparison_wordcloud.png
    └── 4_lagos_weekly_record_timeline.png
```

## 統一データ読み込み関数 / Unified Data Loader

本プロジェクトは異なる新聞形式に対応する統一データ読み込み関数を提供します：

```python
# Counting_Worlds_cleaned.ipynb、demo_en.ipynb、またはdemo_ja.ipynb内で定義済みの関数を使用

# 自動判定
df = load_newspaper_data('your_data.csv')

# 手動指定
df = load_newspaper_data('your_data.csv',
                        data_type='editorial',
                        data_source='Your Newspaper')
```

### 対応データ形式 / Supported Data Formats

| データタイプ | カラム構造 | 説明 |
|-------------|-----------|------|
| **Editorial (社説)** | id, text, Publication Date, Year, Years | LOE/LWRE形式 |
| **Correspondence (読者投稿)** | no, id_1, text, year, date | LOC形式 |

### 自動変換機能 / Automatic Conversion

関数は以下の処理を自動実行します：
- ファイル名からデータタイプを自動判定
- カラム名の統一（Publication Date → date等）
- LOC特有の処理：`no` → `id`、`id_1` → `composite_id`
- メタデータの追加（data_source, article_type）

## データファイル / Data Files

### 必要なCSVファイル形式
以下は本研究で使用したファイル例です（著作権の関係で本リポジトリには含まれません）：
- LOE形式の例: `LOE_150_20250422.csv` - Lagos Observer Editorials
- LOC形式の例: `LOC1882-88_original_divide_20250322_Individual_id.csv` - Lagos Observer Correspondences  
- LWRE形式の例: `LWRE_1328_20250321.csv` - Lagos Weekly Record Editorials

**注意**: ファイル名は任意です。同じCSV構造（text, date/year列を含む）であれば使用可能です。
- 本コードはナイジェリア新聞（Lagos Observer, Lagos Weekly Record）用に開発されました
- ファイル名にLOE/LOC/LWRが含まれる場合、これらの新聞形式として自動認識されます
- 他の新聞データを使用する場合は、data_typeパラメータで記事種別を明示的に指定してください
  例：load_newspaper_data('your_data.csv', data_type='editorial')

データ形式:
- テキストカラム
- 日付情報（年、月、日）
- 記事ID

## 出力結果 / Outputs

分析結果は以下のフォルダに保存されます:
- `basic_stats/` - 基礎統計
- `text_mining/` - テキストマイニング結果
- `geographic_analysis/` - 地理的分析
- `pronoun_analysis/` - 代名詞分析
- `visualizations/` - 各種グラフ・図表

### サンプル結果 / Sample Results

#### 1. 記事数の年次推移
![Article counts by year](images/1_article_counts_by_year.png)

Lagos Observer社説の年次記事数推移（1882-1888）

#### 2. Lagos Observer 社説のワードクラウド
![Lagos Observer Wordcloud](images/2_lagos_observer_wordcloud.png)

頻出語彙の視覚的表現。"government", "lagos", "people", "native"などの重要キーワードが確認できます。

#### 3. データセット間の語彙比較
![Datasets comparison](images/3_datasets_comparison_wordcloud.png)

Lagos Observer社説・読者投稿・Lagos Weekly Record社説の語彙使用パターンの比較

#### 4. Lagos Weekly Record 記事数推移
![Lagos Weekly Record timeline](images/4_lagos_weekly_record_timeline.png)

Lagos Weekly Recordの長期間（1890-1920年代）にわたる記事数推移

## 研究成果 / Publications

本コードは以下の研究に関連しています:
- 「世界の数え方：テキストマイニングによる黎明期ナイジェリア新聞の地理的表象分析」
- 『台頭するアフリカ地域大国ナイジェリアの諸相』（2026年刊行予定）第7章

## ライセンス / License

MIT License

本プロジェクトはMITライセンスで公開されています。以下が可能です：

**許可事項:**
- 商用・非商用問わず自由に使用
- 修正・改変
- 再配布
- 私的利用

**条件:**
- 著作権表示とライセンス表示を保持すること
- 作者は無保証であること

### 引用例 / Citation Example

コードを使用する場合は、以下のように引用してください：

**Pythonコード内:**
```python
# Based on Counting Worlds Analysis by Nozomi Sawada
# https://github.com/nozomi-sawada/counting-worlds-analysis
# MIT License
```

**学術論文:**
```
Sawada, N. (2025). Counting Worlds: Text Mining Analysis of Early Nigerian Newspapers. 
GitHub repository: https://github.com/nozomi-sawada/counting-worlds-analysis
```

## 著者 / Author

澤田 望 (Nozomi Sawada)

## 謝辞 / Acknowledgments

本研究はJSPS科研費19K13372およびJSPS科研費23H00013の助成を受けたものである。

This work was supported by JSPS KAKENHI Grant Numbers 19K13372 and 23H00013.

## 注意事項 / Notes

- 本コードは研究目的での使用を想定しています
- 商用利用も自由ですが、必ず引用してください（MITライセンスの条件）
- データファイルの再配布は著作権の関係で制限されています

## Contact

研究内容に関するお問い合わせは、GitHubのIssueをご利用ください。