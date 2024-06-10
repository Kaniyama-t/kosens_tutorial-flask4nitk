
Flask入門：意味のあるデータを返してみよう
===


[![hackmd-github-sync-badge](https://hackmd.io/FyPat9bFRSCdL01PZeG3wQ/badge)](https://hackmd.io/FyPat9bFRSCdL01PZeG3wQ)

:::danger
<h1>この記事は書き途中です</h1>
<h4>フィードバックお待ちしてます(hackmdにログインしてコメントください。)</h4>
:::

:::warning
**前提**
- **サーバとクライアントとは？を読了済**
- **Python&Library installationを読了済**
    - **PC内に、Python安定版(最新)が入っている**
    - **PC内にあるPython安定版(最新)について、パスが通っている**
- Flask入門: A Minimal Application を読了済
    - もしくは、Flaskの最低限の構成or書き方がわかること

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

さて、前回はサンプルコードをすこし改変してみたので、今回は**Python標準ライブラリとFlaskを組み合わせて、webサーバが意味のあるデータを返すようにカスタマイズしてみましょう**。

## セットアップ

作業ディレクトリを用意し、vscodeで開きましょう。
開いた後にターミナルを立ち上げて、`pip install Flask`を打ちましょう。

## 意味のあるデータを返すには

前セクションで実行した、サンプルコードを再度見てみましょう。

```python=hello.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

Flaskでは、`@app.route`で指定したURLにアクセスがあったとき、その直下の関数を実行しています。

たとえば、この場合はhttp[:]//127[.]0[.]0[.]1:xxxx/にアクセスがあったときに、`hello_world`関数が実行されると書かれています。

`hello_world`関数内には、**ただ`return "<p>Hello, World!</p>"`と書かれているだけ**であり、実際には何も処理を行わずに、いつでも`"<p>Hello, World!</p>"`という文字を返すだけになっています。

==つまり、**`return`の手前に処理を書き、return文へ処理結果を渡すことで【意味のあるデータを返す】ことができます。**==

## いまなんじ？

それでは、アクセスしたときに"現在の時刻を表示するWebサーバ"を作ってみましょう。
Pythonに慣れている人向けに、あえてサンプルコードは隠しておきます。わからない人は開いてください。

<details>
    <summary>サンプルコード</summary>

```python=now.py
from flask import Flask
import datetime

app = Flask(__name__)

@app.route("/")
def current_datetime():
    now = datetime.datetime.now()
    now = str(now) # datetime型を文字列に強制キャスト(非推奨)
    return "<p>" + now + "</p>"
```
    
</details>

<br /><br />

どういったコードを書くかで若干の違いがありますが、以下のように日時がふんわりわかればOKです。

![image](https://hackmd.io/_uploads/rytKPXQSR.png)

## いまなんじ？（解説編）

さて、私が書いたコードは以下でした。

```python=now.py
from flask import Flask
import datetime

app = Flask(__name__)

@app.route("/")
def current_datetime():
    now = datetime.datetime.now()
    now = str(now) # datetime型を文字列に強制キャスト(非推奨)
    return "<p>" + now + "</p>"
```

まずpython標準ライブラリである`datetime`を用いて「いまの日時」をnowという変数に格納しています。これは、"python 現在日時 取得" とかでググると出てきます。

![image](https://hackmd.io/_uploads/HkHqtX7SC.png)

このとき、datetime.now()で取得したデータは、どんな形で返されているでしょうか。実際に検証プログラムを書いて、実行してみましょう。

![image](https://hackmd.io/_uploads/rk-S977rR.png)

試しにnow変数の型と中身を出力したところ、型はdatetime.datetime型で、中身を文字で出力すると、ちゃんと日時が出ていることがわかります。

つまり

- **now変数は、文字列型(String)ではない**
- **文字列型ではないが、強制的に文字として出力させることができる**
    - （※`print`関数は、渡されたものが変数単体の時、強制的に文字列型へキャスト(≒変換)します）

ことがわかります。

ですので、now変数を文字列型へ強制的にキャスト(≒変換)してやれば、returnで返却が可能であることがわかりました。

特定の型を文字列型へ強制的に変換するには、`str()`関数を使います。(※そういうものです)

そして、文字列になった日時情報をブラウザへレスポンスとして返してやる必要があります。~~実はpタグで囲む必要はないのですが~~ 文字列の結合は、文字列とString(文字列)型の変数でやる場合 `+` でつながるので、

```python=now.py.splited
    now = datetime.datetime.now()
    now = str(now)
    return "<p>" + now + "</p>"
```

とすることで、いまなんじかを表示することができます。

:::info
### 関数名について
---
今回、関数名は `hello_world`ではなく`current_time`に改めています。
ただ単にサンプルとして動作させる分には変えなくてよいのですが、複数のエンドポイント("/"と"/record"と"/show-graph"で処理を分けるなど)を用意する場合は、複数の関数を用意する必要があります。この時、関数名が重複するとエラーが出て困ってしまうので、実際の処理内容に合わせて関数名も変えておく癖をつけておくと良いでしょう。
:::


:::warning
### Python標準ライブラリとrequirements.txt
---
**結論：Python標準ライブラリだけはrequirements.txtに書かなくてよい(書いたらエラーが出る)**

前述でrequirements.txtにライブラリ名を列挙しておくと良いと書きましたが、Python標準ライブラリのみ扱いが違います。
requirements.txtに記載したライブラリ名は、その用途上 `pip` コマンドに食わせるためのものなので、https://pypi.org/ (pipコマンドを介してアクセスするライブラリの保管庫的なもの)上になければなりません。
しかし、Python標準ライブラリはpythonコマンド(厳密には、Pythonランタイム環境)のインストール時に勝手に入るものなので、pypiには登録されていません。
ですので、Python標準ライブラリには記載する必要がありません。
同様に、自分が勝手に作ったモジュールも、`import <muModuleName>`のように`.py`ファイルには記載する必要がありますが、requirements.txtには記載する必要がありません。
:::

## さらにワンステップ踏み込んで

「たったこれだけ」のことですが、実は私たちは大いなる力を手に入れています。これで私たちは、Pythonでアクセスできる全情報を、Webページとして表示できるようになりました。

例えば、今のPCのCPU使用率を出してみましょう

:::danger
### この先は `--host 0.0.0.0` を付けるな
---
PCのCPU使用率やメモリ使用量、ページのアクセス回数といった情報は、それ自体が直接なにか情報を漏洩しているわけではありません。が、作ったWebサーバプログラムが実行されているマシンのスペックを間接的に知ることができるため、DDoS攻撃等を行う場合の指標建てがしやすくなるなどの問題があります。(特に今、DDoS攻撃がさかんに行われています。)
また、PythonランタイムやWindows自体に脆弱性があった場合、他の攻撃と組み合わせてCPU使用率の出力を司るメモリや処理上に任意のコードの実行結果を出力できる場合があります。
別に、ただHTMLを返しているだけのWebサーバにも言えることではありますが、【ITインフラや実行環境の設定をしっかりしていないのにWANや公衆LANに公開することは自殺行為】なので注意しましょう。
:::

:::danger
### 導入ライブラリが安全かどうか確認を
---
pypi.org に公開されているコードは、インターネットにアクセスできる誰かが作ったライブラリです。
よくある話として、
- **よく使われているライブラリに脆弱性(≒意図的に悪意あるコードが追加されていた)があり、裏でC2サーバと通信していた。それを使用していた複数の企業の製品が脆弱性有りとして問題になった。**
    - [Example 1: https://www.cybereason.co.jp/blog/malware/7166/](https://www.cybereason.co.jp/blog/malware/7166/)
        - [JVN情報](https://jvndb.jvn.jp/ja/contents/2020/JVNDB-2020-014179.html)
        - [もうちょっとわかりやすい記事: https://www.creationline.com/tech-blog/cloudnative/aquasecurity/46221](https://www.creationline.com/tech-blog/cloudnative/aquasecurity/46221)
        - [影響を受けた製品のページ例: https://www.dell.com/support/kbdoc/ja-jp/000191930/dsa-2021-181-dell-emc-power-protect-data-manager-update-for-multiple-security-vulnerabilities](https://www.dell.com/support/kbdoc/ja-jp/000191930/dsa-2021-181-dell-emc-power-protect-data-manager-update-for-multiple-security-vulnerabilities)
- **あまり使われていない複数のライブラリについて、その開発者が悪意あるコードを含んで公開していた。**
    - [Example 2: https://news.mynavi.jp/techplus/article/20231120-2823025/](https://news.mynavi.jp/techplus/article/20231120-2823025/)

などが挙げられます。
これらが個人的なリソース(個人的に借りたVPSやクラウド上のランタイム)であるならばやらかしちゃったで済むパターンが多いですが、自宅鯖を運用する場合や、**まして学校や職場のネットワークやそれらで公開しているサーバ上で問題が起きた場合は責任問題になってしまいます。**

**なので、==インターネットにアクセスできる仕様のシステム==にpypi.orgのライブラリを使いたい場合は、原則以下を守りましょう。**

1. 作ったプログラムは脆弱性スキャンを掛けること。（過信しないこと）
2. **公開する場合は最低限、サーバへのアクセス自体にアクセス制限を掛けておくこと。**
    - Ex: クライアント証明書による認証、VPN + Authorizationヘッダのtokenを検証する方式などなど
3. **自分で建てた環境上のものをインターネットに公開する場合(Ex. AWS IoTなどを用いずにWANへraspberry piを公開するなど)は、毎週のシステムメンテナンスが必須だと思うこと。**

ただし、
- PaaSの場合
    - AWS IoTなどのIoT用クラウド製品
        - raspberry piなどのマイコンからのデータをクラウドサービスがプロキシ・管理してくれる場合
    - Cloud Runなどのコンテナホスティングサービス
        - "環境構築の一部をクラウドベンダーに丸投げできる場合"
- クローズドな環境下でシステムを公開する場合
    - (職場／学内LAN限定公開(VPNは用いない場合)など)

は、クラウドベンダーや各組織がITインフラレベルで不必要な通信をしないように制限を掛けてくれている場合があります。
この場合は、そのライブラリが危険かどうかをふんわりググって確認する程度でも大丈夫な場合があります。（※ただし、インターネットに公開するシステムなら絶対週次のチェックは必要だと思う）
**自分で環境構築をしてネットに公開したい・・・という場合は、下調べをした上できちんと関係者からアドバイスを仰ぎましょう。**
**問題にならないように、あなたが所属する組織の情報セキュリティチームに確認を取るようにしてください。**（趣味開発の場合はISPやクラウドベンダーのポリシーを見ようね！）
:::

例えば、CPU使用率を表示できる `psutil` ライブラリを使ってみると

```python=cpu_usage.py
from flask import Flask
import psutil

app = Flask(__name__)

@app.route("/")
def cpu_usage():
    cpu_percent = psutil.cpu_percent(percpu=True)
    mem = psutil.virtual_memory() 
    return "<ul><li>CPU Usage: " + str(cpu_percent) + "%</li><li>Memory Usage: " + str(mem) + "</li></ul>"
```

~~ちなみにpsutilも昔脆弱性報告されてました https://jvndb.jvn.jp/ja/contents/2019/JVNDB-2019-011835.html~~

これを実行すると...

![image](https://hackmd.io/_uploads/rJ9-5NQBR.png)

==TBD==


#### Links

- [Trouble Shooting](https://hackmd.io/@kaniyama-t/ryuD1rmBA)