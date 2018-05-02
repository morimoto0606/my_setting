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

# Git
    - merge: --no-ff とするとbranchからマージしたことが明確に残る
    - git log --graph ブランチを視覚的に見る
# zsh, bash
    - set -o vi: zsh がvim keybind になる。
    - bind '"jj": vi-movement-mode'
# Tex by windows
    - Vim-Latex (Latex-Suite) のwikiを参考にした。
    - pathogenにプラグイン置く
    - vimrcをコピー、gvimの部分のみvimに修正
