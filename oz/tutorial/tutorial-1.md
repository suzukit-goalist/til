  Jsoupを使った求人情報のクローリング&スクレイピング

## 目的

求人情報のクローリング&スクレイピングに使用している技術や仕組みを理解すること

## 期間

2~3日程度

## 前提

* eclipseをインストール済みであること
* Javaの文法を理解していること

## 依存するライブラリ

本ドキュメントでは以下のライブラリを使用している。

* jsoup 1.9.2

mavenを使用する場合は以下のpom.xmlを参考にする

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

### Jsoupに慣れる

Jsoupはjava製のDOM解析用のライブラリである。  
サーバ上やローカル環境のHTMLファイルのDOMを解析する際に有用である。  

利用方法の詳細については、以下のドキュメントを参照する。  
https://jsoup.org/cookbook/  

#### サーバ上のHTMLファイルを取得する。

```java
import java.io.IOException;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.select.Elements;

public class TrainingScraping {
    public static void main(String[] args) throws IOException {
        Document doc = Jsoup.connect("http://en.wikipedia.org/").get();//サーバ上のHTMLファイルを取得する
        Elements newsHeadlines = doc.select("#mp-itn b a");//cssセレクタを引数を渡すと対象のDOMを取得できる
        System.out.println(newsHeadlines.text());//Elements#text()でDOMのテキスト部分を抽出できる。
    }
}	
```

#### 任意のDOMのテキストのみを取得する

org.jsoup.select.Elements#html() ではなく、org.jsoup.select.Elements#text()を使う

* org.jsoup.select.Elements#html()の場合

```
    <p>「成長したい！」という意欲をお持ちの方を積極採用していきます。35歳迄（例外事由3号のイ）</p> 
    <p>【具体的には】<br>◆経験は一切問いません。 <br>経験がないというのは、 <br>新しいことをどんどん吸収できるということ。 <br>ですから、新卒の方と同じように、 <br>言葉づかいから名刺交換などのビジネスマナーにいたるまで、 <br>一つ一つ教え、育てたいと思っています。 <br> <br>≪ひとつでも当てはまったら、ぜひご応募を！≫ <br>・みんなで同じ目標に向かってがんばりたい <br>・新しいフィールドでチャレンジしたい <br>・成長企業で働きたい</p>
```

* org.jsoup.select.Elements#text()の場合

```
    「成長したい！」という意欲をお持ちの方を積極採用していきます。35歳迄（例外事由3号のイ） 【具体的には】 ◆経験は一切問いません。 経験がないというのは、 新しいことをどんどん吸収できるということ。 ですから、新卒の方と同じように、 言葉づかいから名刺交換などのビジネスマナーにいたるまで、 一つ一つ教え、育てたいと思っています。 ≪ひとつでも当てはまったら、ぜひご応募を！≫ ・みんなで同じ目標に向かってがんばりたい ・新しいフィールドでチャレンジしたい ・成長企業で働きたい
```

### 求人情報のクローリング&スクレイピングを行う

Comming soon

## 参考

* jsoup使い方メモ  
  http://qiita.com/opengl-8080/items/d4864bbc335d1e99a2d7

* jsoup cookbook  
  jsoupで何ができるかはここを一通り見るとわかる  
  https://jsoup.org/cookbook/

* 意外と知らない!?CSSセレクタ20個のおさらい  
  chromeの開発者ツールでCSSセレクタは取得できるが、CSSセレクタの意味を知りたいならここを見る。
  http://weboook.blog22.fc2.com/blog-entry-268.html
