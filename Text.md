OpenFOAMのカスタマイズ・ソースコード改造 超入門

# 目的 #

今回の講習の目的は，OpenFOAMをカスタマイズ（ソースコードを変更する）ための手順の全体像を学ぶことである。時間に制約があるため，ソースコードの詳細には触れない。

# 手順 #

1.   ベースとなるコードの選定
2.   ベースコードをユーザ作業ディレクトリに複製
3.   ベースコードの例題ディレクトリをユーザ実行ディレクトリに複製（名前の変更作業）
4.   ベースコードがコンパイルできることを確認する
5.   コンパイルしたベースコードが実行できることを確認する
6.   コードの変更
7.   コンパイル
8.   例題の変更
9.   実行   


# 実作業 #

## ベースとなるコードの選定 ##

OpenFOAMからオリジナルコードを作成する時には，既存のコードから目的に近いモノを選び，修正していくことを推奨する。

今回は，非定常，非圧縮，層流を解くicoFoamをベースとする。これに，温度場を求めるためのエネルギー方程式を追加する。ただし，温度場と速度場とは連成しないものとする。


## ベースコードをユーザ作業ディレクトリに複製 ##

使用しているOpenFOAMの環境を確認する。

    echo $WM_PROJECT_DIR

    /home/user/OpenFOAM/OpenFOAM-2.3.x

　OpenFOAMのソースコードは，$WM_PROJECT_DIR/applications/solvers

ソルバの場所
$FOAM_SOLVERS

ユーザーのプロジェクトディレクトリ
$WM_PROJECT_USER_DIR

    echo $WM_PROJECT_USER_DIR
    /home/user/OpenFOAM/user-2.3.x

一般的には，カスタマイズしたソルバは下記に保存する。

$WM_PROJECT_USER_DIR/applications/solvers

OpenFOAMインストール時の実行ファイルは，$FOAM_APPBIN ディレクトリに格納されている。

    echo $FOAM_APPBIN
    /home/user/OpenFOAM/OpenFOAM-2.3.x/platforms/linuxGccDPOpt/bin

本当に？確かめよう。icoFoamがどこにあるかは，次のコマンドで確認できる。

    which icoFoam
    /home/user/OpenFOAM/OpenFOAM-2.3.x/platforms/linuxGccDPOpt/bin/icoFoam


コンパイルに成功すると生成される実行ファイルは，$FOAM_USER_APPBIN に格納する。

    echo $FOAM_USER_APPBIN
    /home/user/OpenFOAM/user-2.3.x/platforms/linuxGccDPOpt/bin



## ベースコードの例題ディレクトリをユーザ実行ディレクトリに複製（名前の変更作業） ##

コピー

mv icoFoam.C my_icoFoam.C
rm icoFoam.dep


Make subdirectory の 'files' file を修正する。

    my_icoFoam.C
    EXE = $(FOAM_USER_APPBIN)/my_icoFoam



## ベースコードがコンパイルできることを確認する ##


## コンパイルしたベースコードが実行できることを確認する ##


## コードの変更 ##

createField.H に，DT と T を追加。

my_icoFoam.C に，温度場の式を追加。

system/fvSchemes ファイルと system/fvSolution ファイルに，温度場の解き方に関する設定を追加。

## コンパイル ##


## 例題の変更 ##

constant/transportProperties に，DTを追加。（nuを参考に）

0/ ディレクトリに，T ファイルを追加。（pを参考に）

## 実行    ##


# 参考情報 #
http://openfoamwiki.net/index.php/How_to_add_temperature_to_icoFoam

