# wikipedia_importer
wikipedia data import script
## download database
https://dumps.wikimedia.org/jawiki/  
https://dumps.wikimedia.org/jawiki/latest/  


# Wikipediaの記事データの最新ファイル
curl -O https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-pages-articles.xml.bz2  
bzip2 -d jawiki-latest-pages-articles.xml.bz2  

# xml2sqlをインストール
$ git clone https://github.com/Tietew/mediawiki-xml2sql.git  
$ cd mediawiki-xml2sql  
$ ./configure  
$ make  
$ sudo make install  

cat jawiki-latest-pages-articles.xml | sed -e 's/<dbname>.*<\/dbname>//' -e 's/<ns>.*<\/ns>//' -e 's/<parentid>.*<\/parentid>//' -e 's/<sha1>.*<\/sha1>//' -e 's/<model>.*<\/model>//' -e 's/<format>.*<\/format>//' -e 's/<redirect>.*<\/redirect>//' -e 's/<redirect.*\/>//' | xml2sql
 
 ```
 CREATE TABLE page (
  page_id int unsigned NOT NULL auto_increment,
  page_namespace int NOT NULL,
  page_title varchar(255) binary NOT NULL,
  page_restrictions tinyblob NOT NULL,
  page_counter bigint unsigned NOT NULL default '0',
  page_is_redirect tinyint unsigned NOT NULL default '0',
  page_is_new tinyint unsigned NOT NULL default '0',
  page_random real unsigned NOT NULL,
  page_touched binary(14) NOT NULL default '',
  page_latest int unsigned NOT NULL,
  page_len int unsigned NOT NULL,

  PRIMARY KEY page_id (page_id),
  UNIQUE INDEX name_title (page_namespace,page_title),
  
  -- Special-purpose indexes
  INDEX (page_random),
  INDEX (page_len)
);

CREATE TABLE revision (
  rev_id int unsigned NOT NULL auto_increment,
  rev_page int unsigned NOT NULL,
  rev_text_id int unsigned NOT NULL,
  rev_comment tinyblob NOT NULL,
  rev_user int unsigned NOT NULL default '0',
  rev_user_text varchar(255) binary NOT NULL default '',
  rev_timestamp binary(14) NOT NULL default '',
  rev_minor_edit tinyint unsigned NOT NULL default '0',
  rev_deleted tinyint unsigned NOT NULL default '0',
  rev_len int unsigned,
  rev_parent_id int unsigned default NULL,

  PRIMARY KEY rev_page_id (rev_page, rev_id),
  UNIQUE INDEX rev_id (rev_id),
  INDEX rev_timestamp (rev_timestamp),
  INDEX page_timestamp (rev_page,rev_timestamp),
  INDEX user_timestamp (rev_user,rev_timestamp),
  INDEX usertext_timestamp (rev_user_text,rev_timestamp)

)  MAX_ROWS=10000000 AVG_ROW_LENGTH=1024;

CREATE TABLE text (
  old_id int unsigned NOT NULL auto_increment,
  old_text mediumblob NOT NULL,
  old_flags tinyblob NOT NULL,
  
  PRIMARY KEY old_id (old_id)

) MAX_ROWS=10000000 AVG_ROW_LENGTH=10240;
```


 $ mysqlimport -u root -p -d -L jawikipedia text.txt  
 $ mysqlimport -u root -p -d -L jawikipedia page.txt  
 $ mysqlimport -u root -p -d -L jawikipedia revision.txt  
 $ mysql -u root -p DATABASE < jawiki-latest-categorylinks.sql  

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

