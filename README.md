# 4DStreetView SimpleViewerのインストールと実行方法

このドキュメントでは、MacOS12.1以上を対象として方法を記述する。

## STEP1: KVSのインストール

1.　必要なパッケージをインストールする
```
$ brew install git
$ brew install ffmpeg
$ brew install glfw     
```

2.　環境変数KVS_DIR（KVSをインストールするディレクトリ）を設定する
```
export KVS_DIR=~/local/kvs
export KVS_FFMPEG_DIR=/opt/homebrew/opt/ffmpeg
export PATH=$KVS_DIR/bin:$PATH
```

3.　環境変数KVS_FFMPEG_DIR（FFmpegのインストールディレクトリ）を設定する
```
export KVS_FFMPEG_DIR=/usr/local/opt/ffmpeg      (Intel Macの場合)
export KVS_FFMPEG_DIR=/opt/homebrew/opt/ffmpeg   (M1 Macの場合)
```

4.　GitHubよりKVSをダウンロードする
```
$ cd ~/Downloads
$ git clone https://github.com/naohisas/KVS.git
```
※　上の例では「~/Downloads」にソースコードをダウンロード（gitクローン）している。ダウンロードするディレクトリはどこでも良いが、環境変数KVS_DIR（インストールターゲット）で指定したディレクトリとは別にするほうが良い。

5.　kvs.confを編集して、FFmpegを有効化する
```
$ cd KVS         （KVSをクローンしたディレクトリに移動）
$ vi kvs.conf    （kvs.confをエディタで開き以下のように設定）
```
〈kvs.confの中身〉
```
KVS_ENABLE_OPENGL     = 1
KVS_ENABLE_GLU        = 1
KVS_ENABLE_GLEW       = 0
KVS_ENABLE_OPENMP     = 0
KVS_ENABLE_DEPRECATED = 0

KVS_SUPPORT_CUDA      = 0
KVS_SUPPORT_GLUT      = 1
KVS_SUPPORT_GLFW      = 0
KVS_SUPPORT_FFMPEG    = 1    <---「1」にする
KVS_SUPPORT_OPENCV    = 0
KVS_SUPPORT_QT        = 0
KVS_SUPPORT_PYTHON    = 0
KVS_SUPPORT_MPI       = 0
KVS_SUPPORT_EGL       = 0
KVS_SUPPORT_OSMESA    = 0
```

6.　KVSをコンパイル＆インストールする。
```
$ make
$ make install
```

7.　KVSが正しくインストールされているか確認する
```
$ kvscheck -version
```
`KVS version : 3.0.0` などバージョンが表示されれば成功である。

## STEP2: SimpleViewer用のサンプルデータをダウンロード
```
$ cd ~/Downloads　　（インストールしたいディレクトリに移動する）
$ git clone https://github.com/vizlab-kobe/4DSVTestData.git
```
## STEP3: SimpleViewerのインストール
※ 以下では、~/Workディレクトリにインストールする場合の手順を説明する。

1.　GitHubよりSimpleViewerをインストールする
```
$ cd ~/Work/　　（インストールしたいディレクトリに移動する）
$ git clone https://github.com/Ren-SAKAI/4DSVSimpleViewer.git
```

2.　コンパイルする
```
$ cd 4DSVSimpleViewer  （クローンしたディレクトリに移動する）
$ kvsmake -G
$ kvsmake
```

## STEP4: SimpleViewerの実行方法
1.　4DStreetViewer用のconfigファイル: conf.4dsv をサンプルファイル test.4dsvからコピーする
```
$ cp test.4dsv conf.4dsv
```

2.　conf.4dsv を編集する
```
$ vi conf.4dsv  （以下の仕様に従い編集する）
```
```
    1行目: 再生したい動画群が置かれているディレクトリ
    2行目: 再生するファイルの拡張子
    3行目: x,y,z各方向の視点数（x,y,zの順に半角スペースで区切る）
    4行目: 初期状態で表示したい視点位置（x,y,zの順に半角スペースで区切る）
    5行目: データの総フレーム数
    6行目: データの場所
```
3.　実行する
```
$ ./4DSVSimpleViewer conf.4dsv
```