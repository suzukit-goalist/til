# ローカル環境でOZを動かす

## SVNリポジトリ

セキュリティの都合上、未記載

## SVNアカウント作成

盛次さんに依頼する

## 開発環境作成

### リポジトリのチェックアウト

1. VPN接続にする(ローカルPCにパブリックIPを割り当てる)

2. Eclipseにて、パースペクティブを「SVNリポジトリエクスプローラー」に設定する

3. 画面左側のナビゲータ上より、「新規リポジトリーロケーション」アイコンをクリック

4. ダイアログにて、以下を入力し、完了ボタンを押す

* URL  
		リポジトリのURLを入力する  
* 認証  
    ユーザ名とパスワードを入力する

5. ツリーより、以下のフォルダを右クリックし、「チェックアウト」を選択する

* OZ  
* OZlib  

6. チェックアウト後、パースペクティブをJavaにする。画面左側のナビゲータに上記ディレクトリが表示されていることを確認する

### コンパイルエラーを除去する

1. Java7でコンパイルする設定にする

Javaコンパイラー

		コンパイラーの準拠レベル
			1.7にする

2. OZプロジェクトを右クリックし、プロパティを選択する

2.1. Javaのビルドパス
		
* ライブラリ  
** ant.jarからxmlbeansを選択し、除去する  
** 「外部jarの追加」より、OZ/lib内のjarファイルを選択する  
** JREシステムライブラリが1.7になっていることを確認する。なっていない場合は、1.7にする  
* ソース  
** resourcesを除去
** チェックアウトしたOZプロジェクトのresourcesを追加する  
** OZlib/srcを追加する
** OZlib/resourceを追加する

3. AWS SDKをインストールする
	新規ソフトウェアのインストール
		http://aws.amazon.com/jp/eclipse
			Core
			Develop
				Lamdaplugin
	一旦AWSアクセスキーは無視
	$HOME/aws-java-sdkにソースコードがインストールされる	

	Javaのビルドパス
		ライブラリの追加	
			Java AWS SDK

4. OZを動かす

事前準備

ドライブ直下(ルート直下)に以下のディレクトリを作成する
	oz/
		oz.ini
			編集する
				instanceCd
		ozCrawler/
			ini
			lst
		ozScraper/
			ini
				shiftf-JIS
			lst
	ebs/

クロールする

OzCrawlerをエクリプス上で実行する
	args = new String[]　{"RNN"};

スクレイプをする

ebs/oz/～～～～～より、htmlファイルの出力

OzScraperをエクリプス上で実行する
	args = new String[2];
	args[0] ="RNN-TC"
	args[1] ="ジョブのID"

おまけ
	colud berry explorerをインストールすると幸せになれる
	zip化はozRegularJobでやる。
