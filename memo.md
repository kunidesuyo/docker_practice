# Dockerを用いた開発環境構築　練習
## Docker
- メリット
    - 簡単に環境構築できる
    - 環境の共有が簡単
    - 環境が軽い
- デメリット
    - windows serverに対応していない？
> windows homeのOSでは「Docker Toolbox」というソフトをインストール必要  
> →Virtual Box(仮想マシン)上にDockerを立ち上げる仕組みのソフト  
> windows pro(自分のデスクトップPCはこっちだった)ならHyper-Vという仮想化システムが提供されているためそのまま使用できる

## 環境構築の流れ
1. イメージはDockerfileをbuildして作る方法と、DockerHubからpullしてくる方法がある。

2. イメージを取得したらcreateコマンドを実行してコンテナ(≒サーバー)を作成

3. コンテナが出来たらstartコマンドを実行してコンテナ起動  
アプリの実行環境完成！
runコマンドでpull+create+startができる。

## Dockerをwindows proにインストール
[参考サイト](https://qiita.com/minato-naka/items/e9cd026747693759800c)  
1. Hyper-Vを有効化する
- windowsボタンを右クリックで[アプリと機能]
- [関連設定]の下の[プログラムと機能]
- [Windowsの機能の有効化または無効化]
- [Hyper-V]を選択して[OK]
2. Docker Desktopをダウンロード  
install required windows components for WSL 2を外す  

**エラー発生**
cannot enable Hyper-V service    
- [Hyper-Vを有効にする？](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)  
だめ
- BIOSでIntel VT(Intel Virtualization Technoloy)を有効にした  
タスクマネージャーのCPUの仮想化の項目を確認して、有効に変わった  
docker起動確認

## Dockerの基本操作練習
[参考サイト](https://qiita.com/minato-naka/items/e1f91e1df2c4fe7411dc)  
### プログラムソースをマウント
- Dockerfileからimage作成  
`$ docker build -t my-apache-vim-image .`
- imageをrun(オプションで、ポートの設定、マウントするディレクトリの設定)  
`$ docker run --name my-apache-vim-container -p 80:80 -v $PWD/source:/usr/local/apache2/htdocs my-apache-vim-image`  
ホストOSのカレントディレクトリ/sourceとdockerのコンテナの/usr/local/apache2/htdocsを同期(マウント)  
Apacheの設定ファイルもマウント

**基本**  
dockerでのチーム開発では
- Dockerfile
- ソースファイル
- サーバーの設定ファイル
以上の3つを共有する。

## DockerComposeを使ったコンテナの管理
[参考サイト](https://qiita.com/minato-naka/items/8b31d28823cabaa9487a)  
docker-compose.ymlにコンテナの作成やマウントのオプションを記述  