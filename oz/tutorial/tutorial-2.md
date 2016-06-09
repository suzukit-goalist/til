# ローカル環境でOZを動かす

## 目的

* ローカル環境にOZの動作環境を構築すること
* リクナビネクストの求人情報をcsv形式で取得すること

## 前提条件

* SVNアカウント作成を作成済みであること
 
  未作成の場合は、入倉さんか盛次さんに作成を依頼する

* eclipseをインストール済みであること

  英語版のEclipseを使用した場合の手順を記載しています。    
  日本語版のEclipseを使用している場合は、適宜読み替えてください。

* JDK7をインストール済みであること


## 開発環境作成

### ソースコードのチェックアウト

1. SVNリポジトリに接続するため、VPN接続にする(ローカルPCにパブリックIPを割り当てる)

2. Eclipseにて、パースペクティブを「SVNリポジトリエクスプローラー」に設定する

3. 画面左側のパッケージエクスプローラ上部の「新規リポジトリーロケーション」アイコンをクリックする

4. ダイアログにて、以下を入力し、完了ボタンを押す  

    * URL  
        SVNリポジトリのURLを入力する  
    * 認証  
        SVNアカウントのユーザ名とパスワードを入力する  

5. ツリーより、以下のフォルダを右クリックし、「チェックアウト」を選択する  

    * OZ  
    * OZlib  

6. チェックアウト後、パースペクティブをJavaにする  
   画面左側のナビゲータにチェックアウトしたプロジェクトが表示されていることを確認する

7. AWS SDKをインストールする  

    7.1 「ヘルプ > 新規ソフトウェアのインストール」より、AWS SDKをダウンロードする

    * 更新サイトに以下を選択する  
        http://aws.amazon.com/jp/eclipse  
    * 以下のモジュールを選択する  
        * AWS Core Management Tools
        * AWS Deployment Tools/ AWS Lambda Plugin  

* メモ

    ```
    一旦AWSアクセスキー等の入力は無視してOK  
    ソースコード(クラスファイル)は$HOME/aws-java-sdkにインストールされる  
    ```

### プロジェクトの設定

チェックアウト直後では、OZプロジェクトに複数のコンパイルエラーが発生しているので、  
以下の手順に従い、コンパイルエラーを除去する。

#### Java7でコンパイルする設定にする  

OZプロジェクトを右クリックし、「プロパティ > Javaコンパイラー」より、以下の手順に従いコンパイラーの準拠レベルを1.7にする  
    
1. JDK Comliance 内の 「Use compliance from execution environment 'JavaSE-1.6' on the 'Java Build Path'」のチェックを外す

2. 「Compiler compliance level:」を1.7に設定する

3. 「Window > Preferences > Java > Installed JREs」より、JDK7内のJREを選択する


#### プロジェクトのビルドパスを設定する

OZプロジェクトを右クリックし、「プロパティ > Javaのビルドパス」を選択する   

* ライブラリ  
   * デフォルトで設定されているant.jarからxmlbeans-2.3.0.jarまでを除去する  
   * 「Add JARs」より、OZ/lib内のjarファイルを全て選択する  
   * JREシステムライブラリに1.7が選択されていることを確認する。選択されていない場合は、1.7を選択する  
       * 「JREシステムライブラリ」を選択し、「編集」を押す
       * 「Execution environment:」に「JavaSE-1.7 (jdk1.7.0_xx)」を設定する。
   * Java AWS SDKを追加する  
* ソース  
   * デフォルトで設定されているresourcesを除去する  
   * チェックアウトしたOZプロジェクトのresourcesを追加する  
   * 「Link Folder > Create New Folder」より、Ozlib/srcを参照するよう設定する
     * Linked folder location  
       (例)C:\Users\suzukit\workspace\Ozlib\src  
     * Folder name  
       OZlib  
   * 同様に、OZlib/resourceを追加する(不要？手順から削除予定。)  
   
* メモ  

  OZプロジェクト配下の .classpath 等を修正すれば、上記の幾つかの手順は不要になるので、後々対応したい。

## OZ  

### 出力ディレクトリ作成  

ドライブ直下(ルート直下)に以下のディレクトリとファイルを作成する  

```
	oz/
		oz.ini
		ozCrawler/
			RNN.ini
			RNN.lst
		ozScraper/
			RNN-TC.ini
			RNN-TC.lst
	ebs/
```

ファイルの中身については、以下を参考にする  
https://github.com/suzukit-goalist/til/tree/master/oz/tutorial/files/tutorial-2/oz


### クロールする  

1. OzCrawlerのmainメソッドをエクリプス上で実行する。  
   その際、mainメソッド内の記載を以下のように変更する。

   ```java
	args = new String[] {"RNN"};
   ```

   以下のエラーはログ出力に関するエラーなので、一旦無視する。

   ```
   ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console.
   ```

2. htmlファイルが出力されることを確認する。
   
   ebs\oz\${ジョブID}\html ディレクトリ配下にhtmlファイルが生成されていることを確認する。  
   何件か出力されていることを確認できれば、OzCrawlerを停止してOK  

   (例)C:\ebs\oz\20160608120054\html\cmi0082115023_nx1_rq0012862880.html

### スクレイプする  

1. ebs/oz/～～～～～より、htmlファイルが出力されたディレクトリ名を確認する  
   このディレクトリ名がジョブIDとなる。 (例)20160608120054
   以下、ジョブIDを${ジョブID}と表記する

2. OzScraperのmainメソッドをエクリプス上で実行する。  
   その際、mainメソッド内の記載を以下のように変更する。  

   ```java
	args = new String[2];
	args[0] ="RNN-TC";
	args[1] ="${ジョブID}";
   ```  
   
   以下のエラーはログ出力に関するエラーなので、一旦無視する。

   ```
   ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console.
   ```

3. csvファイルが出力されることを確認する。  
   ebs\oz\${ジョブID}配下に、csvファイルが出力されることを確認する  
   (例) C:\ebs\oz\20160608120054\RNN-TC_20160608121023.csv  
