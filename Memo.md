# NeardTree
    - ctrl + e : tree 表示
    - fiele 操作: m
    - directory 作成: m + aでファイル作成と同様にしてdir名/ のようにスラッッシュをつける。

# tmux
    - ctrl + b: prefix(デフオルトのコマンド)
    - prefix + % : 垂直分割
    - prefix + " : 水平分割
    - prefix + & : window 消す
    - prefix + c: 新しいページ作成。
    - prefix + [: pageをスクロールする
    - prefix + o; 次の画面に移動
    - prefix + ;: 以前の画面（pain）に移動
    - prefix + q: 画面の番号の表示, さらに、番号を押すとその番号の画面に移動

# terminal
    - alt + N : new terminal - alt + shift + 方向キー: tab 移動
    - alt + w or exit : 現在のterminalを閉じる

# cmake
    -  cmake project で cmake
    -  make
    -  project file名で実行

# vim 
    - 検索 /hoge, nで次の候補
    - n: step over
    - s: step in
    - finish: step out
    - space + m 選択文字のハイライト
    - shift space + m 解除
    - yy : １行丸ごとコピー
    - P: 前にpaste
    - p: 後ろにpaste
    - ~: 大文字と小文字入れ替え
    - {: 前の段落
    - }: 次の段落
    - n: 検索を繰り返す
    - N: 逆方向に検索を繰り返す。
    - l1, l2s/old/new/gc: l1~l2の間のold new置換(確認しながら)
        - y: 一つずつ置換する
        - n: 一つずつ置換しない
        - a: 全て置換する
        - q: 全て置換しない
    - 手軽な置換：
        - /hoge ：hogeを検索
        - cwfuga: hugaに置換
        - n : 次に移動
        - . : 繰り返し
    - ヤンクした文字列のコピー: q/で検索する。
    - !ctags hoge.hpp : tagファイルを作ってくれ関数にジャンプできる。
    - tag funcNmae とすると、関数の定義にジャンプできる。戻るにはctrl t

# break point
    - fileにブレーク：br set -f "file名" -l "行番号" 
    - file ブレーク短縮版：b "file名":"行番号"
    - 関数にブレーク：br set -n "関数名"
    - br li: break point のリスト
    - br delete 番号：番号のブレークを削除
    - br enable/disable 番号：指定した番号のブレークを有効/無効にする

# 出力
    - po (print object) : 変数の出力
    - p : 構造体の中身を見る
    - br list(li) : break point 一覧

# 呼出履歴
    - bt : back trace (call stack 呼出履歴)
    - fr sl 番号: back trace の各frame に飛べる。
    - th sl 番号：番号のスレッドにジャンプ
    - そこでp で変数の値を確認すると良い

# GUI
    - gui: break pointの箇所で押すとGUIモードになる
    - h: help
    - n, s 等で通常通り進んでいく

# C++ Build(Compile Link)
- Compile: g++ file名たち -o object名 (e.g. g+++ hoge1.cpp hoge2.cpp main.cpp -o hoge)
-  -o で実行形式ファイルを指定、何も指定しないとa.outとなる。
- 実際はオブフェクトファイルを生成し、その後それらをリンクするという作業を行なっている. -c とすると、オブジェクトファイル作成までを行う(e.g. g++ hoge1.cpp hoge2. cpp main.cpp -c).
- object fileまで終わると、.o file達が出来ていて、これを使って、実行形式を作る事も出来る(e.g. g++ hoge1.o hoge2.o main.o -o fuga)。

# Makefileの活用
    - Makefileというファイルを作る
    - 以下のように処理を記述
    - １生成物: 元データ(e.g. hoge: hoge1.cpp hoge2.cpp main.cpp)
    - 2 生成手順を次の行に書く。Tabでスタートすることに注意,debug情報つけるのも普通に出来る。
    - (e.g.   g++ -g hoge1.cpp hoge2.cpp main.cpp -o hoge)
    - Makefileが出来たら、そのディレクトリでmakeすればOK

# CMakeの活用
    - CMakeは自動的にMakefileを作ってくれるツール
    - cmake でMakefile作成
    - makeでMakefilを実行
    - source ある場所にCMakelists.txtを作成。
    - cmake_minimum_required(VERSION 3.0)
    - project(hoge)
    - add_executable(hoge hoge1.cpp main.cpp): 実行ファイル名とそのビルドに必要なcpp file名
    - target_link_libralies: リンクするライブラリの指定
    - 別の場所にdeirectory作成。そのディレクトリでcmake .. するとそこにMakefileが生成される。
    - 後はcmake .. コマンドとmakeコマンドでそのディレクトリにexeができる。
    - Debug/Release mode 指定：cmake -DCMAKE_BUILD_TYPE=Debug or Release

# Library作成
    - 静的リンクライブラリと共有ライブラリがある。
    - 静的リンクライブラリ(.a)について
     1. オブジェクトファイルを作る g++ xxxx.cpp yyy.cpp -c
     2. アーカイブにする。(libライブラリ名.aにするのが慣習) ar -r libライブラリ名.a xxxx.o yyyy.o 
     3. コンパイル時にリンクする。 g++ -o zzzz.cpp libhoge.a
    - 共有ライブラリ（動的リンク）について
    1. cpp file を元にdylibを作る(libライブラリ名.dylibにするのが慣習)。
    g++ -dynamiclib -o libライブラリ名.dylib xxx.cpp yyy.cpp
    2. compile時にdylibファイルとリンクさせる。g++ -o xxx xxxx.dylib main.cpp

# CMakeを使った静的リンク
    - root/Hogeディレクトリに作ったLibライブラリをroot/main.cppにリンクする。
    - Hogeにfunc1.cpp, func2.cppがあるとする。
    - Hoge/CMakeLists.txtに以下を記述。
    - cmake_minimum_required(VERSION3.0)
    - add_library(Lib STATIC func1.cpp func2.cpp) 
    - => cmakeを行ったdirectory(root/debug or root/release)にlibLib.aが出来る.
    - root/CmakeLists.txtに以下を記述
    - cmake_minimum_required(VERSION 3.0)
    - project(static_lib)
    - add_subdirectory(Hoge)
    - add_executable(static_lib main.cpp func1 func2)
    - target_link_libralies(static_lib Lib)

# CMakeをつかった動的リンク
    - root/Hogeディレクトリに作ったHogeライブラリをroot/main.cppにリンクする。
    - Hogeにfunc1.cpp, func2.cppがあるとする。
    - Hoge/CmakeListsに以下を記述。
    - cmake_minimum_required(VERSION3.0)
    - add_library(Lib SHARED func1.cpp func2.cpp) 
    - => rootのcmakeを行ったdirectory(root/debug or root/release)にlibLib.dylibが出来る。
    - root/CmakeLists.txtに以下を記述
    - cmake_minimum_required(VERSION 3.0)
    - project(DynamicLib)
    - add_subdirectory(Hoge)
    - add_executable(static_lib main.cpp)
    - target_link_libralies(DynamicLib Lib)

# GTest のCMakeを使った設定方法
    - root, root/sourcedir, root/testdir がある場合。
    - root/CMakeListsの書き方
    - cmake_minimum_required(VERSION 3.0)
    - project(ProjectX)
    - add_subdirectory(sourcedir) //下にあるdirectoryのCMakelistsを読みにいく
    - add_subdirectory(testdir) //同上
    - enable_testing() // テストを有効にする
    - add_test(NAME ProjectX COMMAND Test) //testを追加　NAME projName, Command Testするライブラリ
    - root/source/CMakelists
    - "cmake_minimum_required(VERSION 3.0)"
     - add_library(my_lib STATIC mysource.cpp) 
    - //my_alg ライブラリを作成スタティックリンク libmy_alg.aができる。
    - 実質ar -o libmy_alg.a myalg.cppをしているのと同じ.
    - add_library(my_lib SHARED mysource.cpp) で共有リンクしても良い。
    - root/test/CMakeLists.txt
    - cmake_minimum_required(VERSION 3.0)
    - set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}) //runtime directoryをセットする
    - set(GTEST_ROOT ~/xxxx/gooletest/googletest) //googletestのパスをGTEST_ROOTに設定
    - include_directories(GTEST_ROOT/include/) //getestのinclude direcotryを指定
    - link_directories(GTEST_ROOT/build/) //gtestのbuildディレクトリをリンクディレクトリに追加
    - add_executable(Test ${CMAKE_CURRENT_CURRENT_DIR}/test.cpp) //Testという名前のtestのsourcefileのexeを追加
    - target_link_libralies(Test my_lib gtest gtest_main pthread)
    - このようにして、root/buildでcmake -DCMAKE_BUILD_TYPE=Debug or Release .. して、さらにmakeすれば
    - root/buildのtestの下にtestディレクトリが出来ていて、そこにexeが出来ている。

# C++ Unique ptr
    - ptr はメモリを指す変数
    - new したポインタを解放(delete) すると、メモリが解放される。
    - 従って、そのメモリに格納されていたオブジェクトは：w破棄される。
    - 別のポインタがそのメモリをさしていた場合、メモリエラーが起きる。
    - unique ptrはメモリを独占する。int a = 3;に対し、auto ptr1 = unique_ptr<int>(&a); 
    - auto ptr2 = unique_ptr<int>(&a); はできる。
    - 同じaをさすので、もしptr1が先に破棄された場合、ptr2はメモリリークになる。

#  C++ enable_if
    - 関数のオーバーロードみたいなも
    - 関数のオーバーロードは引数の違いでBranchingするが、Tの場合見分けられない。
    - enable_if<Condition, ret Type> func(xxx) とすると、ret typeは一緒でもBranchingできる。
 
# Markdown Vim
    - PrevimOpen : Firefoxが立ちあがってプレビューしてくれる。

# Vim 使用中にShellを動かしたくなったら。
    - ctrl + zでバックグラウンドにして、終わったらfgでvimに戻る。
    -
# Visual Studio for mac (C#)
    - root path はexe dll の置いてあるところになる。
    - run=> runwith=>custom configuration=>Run on external...のチェックをはずして実行する
    - 文字列の入力: StreamReader, StreamWriter
    - File 操作：File...
    - exeの実行時は外部プログラムの実行を外せばvisual studio内の画面に表示される。

# Git
## git の基本
git には２つのリポジトリがある。

    local repository
    remote repository

- 通常はremote repositoryをcloneしてきて
- local repositoryを作る。
- remote repositoryはデフォルトでorignという名前で登録。

そして、各レポジトリには３つの状態がある。

    - ワークツリー
    - インデックス
    - リポジトリ

- git add: ワークツリーの変更をindexに登録する。
- git commit: indexの変更をレポジトリに登録する.
- git commit -a: add commitを同時にやる
- 一つずつadd してcommitするのが正攻法
- まとめてaddするには
    - git add -A: 変更のあったworktreeのファイルをindexにadd
    - git add -u: 全てのworktreeのファイルをindexにadd

## remote repository
- git --bare init: localでremote の作成。バックアップの作成と同じ。
- GitHub のGUIで生成するとGitHubのサーバー上にremote repositoryが生成される。
- GitHubで生成したremote リポジトリはデフォルトでoriginと言う名前。
- repository path に別名をつける: git remote add <別命> <リポジトリパス> , e.g. git remote add origin xxxx
- remote => local : git clone <複製元リポジトリ>
- local => remote : git push <送信先リポジトリ> <送信元ブランチ>:<送信先ブランチ>, 
    -  <送信元ブランチ>:<送信先ブランチ> が同じ場合は単純にひとつで良い。e.g.: mater:master => masterで良い。
    -  送信先リポジトリはあるように通常はoriginと言う名前にしておく。(GitHubで生成したリポジトリはremoteという名前)
    -  典型例；git push origin master

## git diff でそれぞれの差分を見る方法
- git diff: worktreeとindexのdiff
- git diff --cached: index とHEAD(repository)のdiff
- git diff HEAD: worktreeとrepositoryのdiff

## 間違い修正
- worktreeを元に戻す: fileをコミットする前
- HEADでワークツリーを上書けばよい
    - git checkout HEAD .

- 誤ったcommitの修正
- commitを取り消す
    - git revert <取り消したいcommitのオブジェクト名>

- merge: --no-ff とするとbranchからマージしたことが明確に残る
- git log --graph ブランチを視覚的に見る

## brunchを使う
master: デフォルトのブランチ（repository生成時に最初に自動生成）
- master ブランチをプロジェクトの本流に使用する
- git branch: branch 一覧を表示、ワークツリーに*がつく
- git branch 新規ブランチ名：新しいブランチを作成

# Shell
## Vim key bind
    - set -o vi: zsh がvim keybind になる。
    - bind '"jj": vi-movement-mode'

## 正規表現
    [^abc] : a or b or c以外の任意の１文字
    x{n, m}: xがn回以上、m回以下


## grep
grep -xx '正規表現' file名 

    - -r: 再帰的に directoryないも探していく。
    - -n: 行を表示する
    - -E: 拡張正規表現を使う
    - -i: 大文字小文字区別しない
    - -H: ファイル名表示

## sed
sed 's/検索値/置換値/フラグ' file名

    - フラグ：g:全検索
    - sedはファイルを上書きしない, print するだけ。
    - fileに書き出すときはリダイレクトする

## awk
awk '{アクション}' file名

    - 例：awk '{print $2,$3}' file名
    - -F:
    - csv カンマ区切りをフィールド分割する
    - 例：awk -F '{sum += $NF} END{print sum}' file名

# Tex by windows
    - Vim-Latex (Latex-Suite) のwikiを参考にした。
    - pathogenにプラグイン置く
    - vimrcをコピー、gvimの部分のみvimに修正
    - latexmk -pdfdvi xxxx.tex

# Python
## Debug
- pudb
    - pudb hoge.py でデバッグモード
    - n: step over
    - s: step in
    - b: break

## Tensorflow
- sessionの考え方
    - 従来はwith tf.Session() as sess:
- AADの流れ
    - tf.reset_default_graph() : グラフ初期化
    - x = tf.placeholder(float32, 'x')
    - y = tf.placeholder(float32, 'y')
    - f = tf.f(x, y) : 適当に関数を定義する
    - dfdx, dfdy = tf.gradients(f, [x, y]
    - feeddict = {x:0.1, y:0.2}
    - with tf.Session as sess: Sessionブロック上で定義した関数や計算が実行される。変数はここで代入。計算グラフが作られる。
    - session.run(tf.global_variable_initializer())
    - session.run([dfdx, dfdy], feeddifct)
 
- Eager Execution の登場
    - import tensorflow.contrib.eager as tfe
    - tfe.enable_eager_execution()
    - と最初にやっておくと、sessionを立ち上げて、with ブロックの中でやらなくても良くなる

# Rust
- make project: cargo new --bin "project name"
- fn 5
- open debug add configuration
- select "LLDB" , then setting is written in launch.jsonk
- add "terminal": "integrated: in launch.json
https://murabitoleg.com/mac-rust-vscode/

# CPP in VSCode
- command + ` :  画面移動
- command + shift + B: window ビルド、
    - tasks.jsonを以下のように編集
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "0.1.0",
    "command": "clang",
    "isShellCommand": true,
    "args": [
        "test.c", // プログラムファイル
        "-g",     // デバッグ情報付加する。ないとデバッグできない。
        "-O0",    // コンパイル速度重視
        "-m32"    // 64bitの場合
    ],
    "showOutput": "always"
}
- run :./a.outで実行, commmand + shift + ` でterminal移動
- Debug
    - F5 or spyder マークを押すとlaunch.jsonがないと言われるので、自動設定する。
    基本デフォルトだが、programだけ、余分dものをとっって次のようにする。
    　"program": "${workspaceFolder}/a.out",

## VSCode 
### C++
    tasks.json : run (cmd shift B) した時に走るタスクを決めている(shell でコマンドを打つのと同じ)。 -gはデバッグ情報を出すのに必要
    e.g.
        "tasks": [
        {
            "label": "clang++",
            "type": "shell",
            "command": "clang++",
            "args": [
                "main.cpp",j
                "-g",
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]

    launch.json
    F5を押して、C/C++を初回実行すると作られる。
    実行するexeを決める。デフォルトa.outなので、その前にもともとついている文字を消せば良い。


    c_cpp_properties.json
        inculude path 等を指定できるもの
        e.g.
            "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "macFrameworkPath": [
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk/System/Library/Frameworks"
            ],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
    }
        
### Vscode C++ CMakeを使った場合
    Buildがdebug情報付きで終わっているとする。
    F5でC/C++を押せば、launch.jsonができる。
    それを適当に直せばBuildできる。
    今の所、exeをよしなに直せば動いている。

    attatch : 実行中のプロセスに張り付いて、デバッグすること.
    実行するプロセスをexeにすると、launchと変わらず実行される。

    ちゃんとcmakeでDebug buildする。
       cmake -DCMAKE_BUILD_TYPE=Debug ..

# Ubuntu Install
    Multi Boot
        - Application -> Utility ->Disc Utlity.app
        - Partition を入れる。
        - Ubuntu -> USB install (balena Etcher tool)
        - install (Dual boot OK, Multi boot Fail ??)
    Virtual Box
    - Virtual Box install
    - Ubuntu install 

# ssh
    - use open ssh server
    - sudo apt install openssh-server
    - adressの確認：ubuntuなら画面右上のネットワークをクリックすると確認できる。
    - SSH connection: ssh userName@Ip Adress or client name
     e.g ssh obi-one@192.168.0.8
    
# gdbserver : remote debug with VSCode
    - remote でのデバッグをするためのもの。
    - remote: gdbserver
    - local: gdb
    として、localのgdbでリモートのコードを辿れる。
    基本的には以下の通り。
    https://medium.com/@spe_/debugging-c-c-programs-remotely-using-visual-studio-code-and-gdbserver-559d3434fb78

## Without VsCode
    1. ssh でポートを開いて、remoteのポートとlocalのポートを綱ぐ。
        - local$ ssh -L(remotePortNum):localhost:(LocalPortNum) user@remoteIpadress 
        (port番号は被らなければ任意、リモートとローカルで同じ番号でも良い)
        e.g.  local:yoda, remote:obione の場合
        - local@ ssh -L9999@localhost:9999 obi-one@192.169.0.8
    2. remoteでgdbserver:portNum 実行ファイル を呼ぶ
        - remote@ gdbserver:9999 ./a.out
    3. local でgdb 起動、そして　target remote portNumと呼ぶ
        - gdb
        - target remote localhost:9999
    4. 通常通りlocal gdb debugするとremote のデバッグができる。
## With VsCode
    1. 同じ
    2. sshfsで remote のディレクトリをlocalにマウントする（ローカルのディレクトリにリンクする）
    e.g. remote@ /Test/main.cpp, a.outの場合
        - local@ mkdir /Test (localにマウント先のディレクトリを作る)
        - local@ sshfs remoteUser@IpAdress:mountDir localDir
        e.g local@ sshfs obi-one@192.168.0.8:Test ./Test
    3. gdbserver をリモートで起動. withoutの場合と同じ。
    4. mount されたdirをVScodeで開いてデバッグ開始。 launch.jsonは以下のような感じ
      "configurations": [
        {
          "name": "C++ Launch",
          "type": "cppdbg",
          "request": "launch",
          "program": "${workspaceRoot}/a.out",
          "miDebuggerServerAddress": "localhost:9999",
          "args": [],
          "stopAtEntry": false,
          "cwd": "${workspaceRoot}",
          "environment": [],
          "externalConsole": false,
          "MIMode": "gdb"
        }
        特に、j
          "miDebuggerServerAddress": "localhost:9999",
        が通常と違うところで、これをやると、remoteのコードにアクセスでき、VsCode上でデバッグできる。
    
    ただし、今の所std::vectorなどは見えない。
    remote上のpretty-printingの問題。


# Docker 
## make image build from docker-compose
    - docker-compose.ymlからビルドする
        - docker-compose build 
    - ymlに書かれてるコンテナを起動する　
        - docker-compose run dev bash
    - docker images: Show all images
    - docker ps -a : Show all processes

## docker run container from image obtained from DockerHub
    - docker create -it -v {mount host direcotry path (absolute)}:{path in container} {image name}
    - e.g. docker create -it -v /home/morimoto/Public/GitHub/QuantsCpp/GeometricSde:/dev/GeometricSde morimoto0606/geometricsde_dev
    - start {process id}

## docker command
    - docker images: Show all images
    - docker ps -a : Show all processes
    - docker rm {process id}
    - docker rmi {image name}
    - docker exit : exit process id
        - to restrat: 
        - docker restart process id
        - docker attatch process id
    
    - ctrl p q: not exit
        - to restrat: docker attatch process id

