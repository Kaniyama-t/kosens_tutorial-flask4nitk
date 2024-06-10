
Flask入門：A Minimal Application
===


[![hackmd-github-sync-badge](https://hackmd.io/FP3_XoUUTMuNz9vwxjNG7A/badge)](https://hackmd.io/FP3_XoUUTMuNz9vwxjNG7A)

:::info
<h4>フィードバックお待ちしてます(お気づきの点あればコメントくださると幸いです、要hackmdアカウント(多分))</h4>
:::

:::warning
**前提**
- **サーバとクライアントとは？を読了済**
- **Python&Library installationを読了済**
    - **PC内に、Python安定版(最新)が入っている**
    - **PC内にあるPython安定版(最新)について、パスが通っている**

<details>
    <summary><b>Python, flaskのパスについて</b></summary>
         
- パスが通っているかの確認方法
    - `Windows + R` キーを押す
    - `cmd` と入力しEnter
![image](https://hackmd.io/_uploads/rkpqBzXHA.png)
    - `python --version` と入力し、「コマンドがない」と**エラーが返ってこなければ** ひとまずOK
![image](https://hackmd.io/_uploads/Sym-8GXr0.png)
    - `python` だけ打って `>> ` と出てしまった人は、 `exit()` と入力して抜けて下さい。
- [**通っていない人は、Pythonを再セットアップしましょう。**](https://www.school.ctc-g.co.jp/columns/hishinuma/hishinuma50.html)
    - **注意: 再インストール後は、PCを一旦再起動すること。(環境変数は再起動すると確実に読み込まれるので)**
    
</details>

:::

## Flaskとは

Flaskは、Pythonで書かれた軽量なWebフレームワークです。
初心者にも扱いやすく、小規模から中規模のWebアプリケーションやAPIをかんたんにつくることができます。

## セットアップ

FlaskはPythonの標準ライブラリではないため、インターネットから**特定の方法で**ダウンロードしてくる必要があります。

1. vscodeを起動し、作業したいディレクトリを開く
2. ターミナルを立ち上げる : ```Windows + ` ```
3. 以下コマンドを実行(Tips: \$は打たない、\$がついている行だけ打つ)
```powershell
$ pip install Flask
... たくさんログが流れる(赤文字でない) ...
$ flask
```
4. ちゃんと使い方が出たら導入完了
![image](https://hackmd.io/_uploads/Sk60OzXSC.png)

:::info
### Tips: `requirements.txt` ファイルの作成
---
![image](https://hackmd.io/_uploads/HJUSFfmrR.png)

作業ディレクトリの直下に `requirements.txt` ファイルを作り、↑のように `Flask` を追記すると尚よいです。
これをpip install毎にすることで、ほかの人や他のマシンで同じプログラムを動かすときに、`pip install -r requirements.txt` と打つだけで依存関係が全部入ります。
~~全部pipコマンドが勝手にしてくれればいいのに！って方はPythonではなくてKotlinかgo言語を使いましょう。~~
:::


## A Minimal Application

まずは、Flaskを使ってかんたんなWebアプリケーションを作ってみましょう。

```python: hello.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

<br />

1. `hello.py` を作ります
2. 上のプログラムをコピペします。
3. ターミナルを開いて、以下を実行します。
```powershell=
$ flask --app hello run
```

### 実際にアクセスしてみよう

flask runコマンドを打った後、`Running on http://127.0.0.1:xxxx` と出力されれば、もうWebサーバとして動いています。もし動いていなければ、[**トラブルシューティング**](https://hackmd.io/@kaniyama-t/ryuD1rmBA)を確認してください。

![image](https://hackmd.io/_uploads/SJNJiGQHR.png)

==**ここで、`Running on http://127.0.0.1:xxx` のアドレス部分(http[:]//\~\~)を `Ctrl` キーを押しながらクリックしてみましょう。**==

ブラウザが立ち上がって、Hello Worldと表示されるはずです。

![image](https://hackmd.io/_uploads/ryHVAf7S0.png)

おめでとうございます、これで(コピペですが)オリジナルのWebサーバを作ることができました！

## 自分好みのWebサーバにカスタマイズしよう

さて、サンプルコードを作ることができたので、まずは表示されているテキストやURLを好きにカスタマイズしてみましょう。

上記のサンプルコードは、実は以下のような構成になっていました。

```python=
from flask import Flask # プログラムに"Flask"ライブラリを読み込む

app = Flask(__name__) # "Flask"ライブラリを起動

@app.route("/") # 「"/"(ルート)にアクセスが入った場合以下の関数を実行する」という意味
def hello_world(): # 関数名は任意でよいが、重複はNG
    return "<p>Hello, World!</p>" # HTMLテキストをPython上で作って、レスポンスとして返している(ブラウザに結果として表示される)
```

つまり、

1. `app = Flask(__name__)` は書かなければならない
2. `@app.route("/")` のダブルクオーテーション(`"`)の間を変えることで、URLを変更できる
3. `return "<p>Hello, World!</p>"` のダブルクオーテーション(`"`)の間を変えることで、ブラウザで見える文字を変えられる

**例えば、returnで返す文字を `"<h1>I hate report and todo.</h1>"`とすれば...**

![image](https://hackmd.io/_uploads/HktCfQQrC.png)

文字が大きくなり、ついでに中身が変わりました。
文字を大きくしたり小さくしたりというのは、HTMLの記法の話ですのでここでは割愛します。

<br />

**URLも、例えば`@app.route("/")`の`"/"`を`"/DONTCHECK/DONT/DONTCHECK/DONTCHECKYOU/AREYOUDEVIL"`とすると...**

![image](https://hackmd.io/_uploads/ryttmQ7BR.png)

http[:]//127[.]0[.]0[.]1:xxxx<b>/</b> (つまり、"/")ではアクセスできなくなりました。

末尾に`"/DONTCHECK/DONT/DONTCHECK/DONTCHECKYOU/AREYOUDEVIL"`を付けると...

![image](https://hackmd.io/_uploads/HJtf4QQBC.png)

ちゃんとURLをカスタマイズできましたね。

#### Links

- [Trouble Shooting](https://hackmd.io/@kaniyama-t/ryuD1rmBA)