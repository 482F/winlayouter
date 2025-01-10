# winlayouter

`winlayouter` は、アプリケーションの起動とウィンドウの配置を自動化するためのツールです  
AutoHotkey v2 を利用して、複数のアプリケーションを起動し、それぞれのウィンドウを指定した位置に配置します

## 使用法

定義を記述する際の細かい文法については既存の定義や [AutoHotkey v2 ドキュメント](https://ahkscript.github.io/ja/docs/v2/index.htm) を参照してください

### 起動対象の定義

`lib\apps.ahk2` の `_apps` 変数に、任意の名前をキー、定義を値とするオブジェクトの形式で記述します  
定義は下記のプロパティを持ちます

- **target**  
    実行するコマンド、exe ファイル、開くディレクトリを指定します
- **args**  
    コマンドの引数を配列で指定します
- **chromeUrl**  
    開く URL を指定します
- **profile**  
    chrome で開く場合のプロファイルディレクトリ名を指定します (例: `Profile 1`)  
    現在開いているプロファイルのディレクトリ名は chrome://version/ を開いた際の `プロフィール パス` の末尾で確認することができます
- **winTitle**  
    ウィンドウのタイトルや、実行ファイル名等を指定します。詳細は [AutoHotkey v2 ドキュメント](https://ahkscript.github.io/ja/docs/v2/misc/WinTitle.htm) を参照してください  
    `target` や `chromeUrl` によって開いたウィンドウを特定するために使用されます  
    マッチするウィンドウが実行前から存在する場合、ウィンドウを誤認して配置が行われる場合があります
- **workingDir**  
    `target` を実行するディレクトリを指定します  
    例えば `cmd.exe` を起動するとこのプロパティの値がカレントディレクトリになります
- **timeout**  
    起動の待ち時間 (秒) を指定します。この時間を過ぎてもウィンドウが見つからなかった場合はエラーが発生します  
    デフォルト値は 10 です
- **func**  
    実行する関数を指定します

### レイアウトの定義

`layout` ディレクトリに任意の名前の `.ahk2` ファイルを作成して記述します  
ファイルの先頭に `#include ..\lib\default.ahk2` を記述する必要があります  

`apps.fooBar()` のように記述することで、`lib\apps.ahk2` に定義した `fooBar` が起動します  
上記に繋げて下記のメソッドを呼び出すことでウィンドウの位置を変更することができます  

- **.monitor(num)**  
    num 番目のモニターにウィンドウを移動します
- **.left(), .right()**  
    ウィンドウを現在のモニターの左右に寄せます
- **.below(), .above()**  
    左右に寄ったウィンドウを現在のモニターの上下に寄せます  
    .left(), .right() の呼び出し後に実行してください
- **.full()**  
    ウィンドウを最大化します

例えば 2 番目のモニターの右下にウィンドウを移動させたい場合、`apps.fooBar().monitor(2).right().below()` のような記述になります

### 起動

`layout` ディレクトリに作成した `.ahk` ファイルを起動することで、定義に基づいてアプリケーションが起動しレイアウトが調整されます