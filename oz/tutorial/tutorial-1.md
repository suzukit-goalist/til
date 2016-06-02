#  Jsoupを使った求人情報のクローリング&スクレイピング

## 目的

求人情報のクローリング&スクレイピングに使用している技術や仕組みを理解すること

## 期間

2~3日程度

## 前提

* eclipseをインストール済み
* Javaの文法を理解していること

## 依存するライブラリ

pom.xml  
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>jp.co.goalist</groupId>
	<artifactId>training.suzukit</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>training.suzukit</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>

		<!-- http://mvnrepository.com/artifact/org.jsoup/jsoup -->
		<dependency>
			<groupId>org.jsoup</groupId>
			<artifactId>jsoup</artifactId>
			<version>1.9.2</version>
		</dependency>
	</dependencies>
</project>
```

## チュートリアル

### Jsoupに慣れよう

Jsoupはjava製のDOM解析用のライブラリである。  
サーバ上やローカル環境のHTMLファイルのDOMを解析する際に有用である。  

https://jsoup.org/cookbook/  

利用方法

#### サーバ上のHTMLファイルを取得する。

```java
import java.io.IOException;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.select.Elements;

public class TrainingScraping {
    public static void main(String[] args) throws IOException {
        Document doc = Jsoup.connect("http://en.wikipedia.org/").get();
	Elements newsHeadlines = doc.select("#mp-itn b a");//cssセレクタを引数を渡すと対象のDOMを取得できる
	System.out.println(newsHeadlines.text());//Elements#textでDOMのテキスト部分を抽出できる。
	}
}	
```


## 参考

* jsoup使い方メモ  
  http://qiita.com/opengl-8080/items/d4864bbc335d1e99a2d7

* jsoup cookbook  
  jsoupで何ができるかはここを一通り見るとわかる  
  https://jsoup.org/cookbook/

* 意外と知らない!?CSSセレクタ20個のおさらい  
  chromeの開発者ツールでCSSセレクタは取得できるが、CSSセレクタの意味を知りたいならここを見る。
  http://weboook.blog22.fc2.com/blog-entry-268.html
