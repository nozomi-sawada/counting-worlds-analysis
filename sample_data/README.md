# Sample Data / サンプルデータ

**English** | [日本語は下記](#サンプルデータ日本語)

These are sample datasets for demonstration purposes.
The actual historical newspaper data is not included for copyright reasons.

## File Descriptions

### sample_editorial.csv
- **Format**: Structural example of LOE/LWRE (editorial) data
- **Columns**: id, text, Publication Date, Year, Years
- **Content**: 5 fictional editorial articles from 1882

### sample_correspondence.csv
- **Format**: Structural example of LOC (readers' correspondence) data
- **Columns**: no, id_1, text, year, date
- **Content**: 5 fictional readers' letters from 1882

## Usage

```python
# Example: testing with the unified data-loading function
editorial_df = load_newspaper_data('./sample_data/sample_editorial.csv')
correspondence_df = load_newspaper_data('./sample_data/sample_correspondence.csv')

# Check the data
print("Editorial columns:", list(editorial_df.columns))
print("Correspondence columns:", list(correspondence_df.columns))
```

## Notes

- These are not real historical data
- Intended for demo, testing, and development purposes
- For actual research, please use properly licensed historical newspaper data

---

# サンプルデータ（日本語）

これらはデモ用のサンプルデータです。
実際の歴史的新聞データは著作権保護のため含まれていません。

## ファイル説明

### sample_editorial.csv
- **形式**: LOE/LWRE（社説）データの構造例
- **カラム**: id, text, Publication Date, Year, Years
- **内容**: 1882年の架空の社説記事5件

### sample_correspondence.csv
- **形式**: LOC（読者投稿）データの構造例
- **カラム**: no, id_1, text, year, date
- **内容**: 1882年の架空の読者投稿5件

## 使用方法

```python
# 統一データ読み込み関数でのテスト例
editorial_df = load_newspaper_data('./sample_data/sample_editorial.csv')
correspondence_df = load_newspaper_data('./sample_data/sample_correspondence.csv')

# データ確認
print("Editorial columns:", list(editorial_df.columns))
print("Correspondence columns:", list(correspondence_df.columns))
```

## 注意事項

- これらは実際の歴史的データではありません
- デモ・テスト・開発目的での使用を想定しています
- 実際の研究には、適切なライセンスを持つ歴史的新聞データをご使用ください
