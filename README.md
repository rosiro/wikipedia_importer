# wikipedia_importer
wikipedia data import script
## download database
https://dumps.wikimedia.org/jawiki/  
https://dumps.wikimedia.org/jawiki/latest/  


# Wikipediaの記事データの最新ファイル
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-pages-articles.xml.bz2

# 以下個別に欲しい場合のみ

#### 多言語情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-babel.sql.gz  
#### カテゴリ一覧
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-category.sql.gz  
#### カテゴリのリンク情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-categorylinks.sql.gz  
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-change_tag.sql.gz  
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-change_tag_def.sql.gz  
#### 外部リンク情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-externallinks.sql.gz  
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-geo_tags.sql.gz  
#### 画像情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-image.sql.gz  
#### 画像のリンク先情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-imagelinks.sql.gz  
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-iwlinks.sql.gz  
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-langlinks.sql.gz  
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-md5sums.txt  
#### ページ情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-page.sql.gz  
#### ページリンク情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-pagelinks.sql.gz  
#### ページのプロパティ
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-page_props.sql.gz  
#### ページの保護情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-page_restrictions.sql.gz  
#### ページ間のリンク情報
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-pagelinks.sql.gz  


# import mysql
mysql> create database jawikipedia;

$ mysql -u root jawikipedia < jawiki-latest-page.sql
$ mysql -u root jawikipedia < jawiki-latest-pagelinks.sql
$ mysql -u root jawikipedia < jawiki-latest-categorylinks.sql
