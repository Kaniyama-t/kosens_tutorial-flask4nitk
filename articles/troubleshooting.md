Flask入門：TroubleShooting
===


[![hackmd-github-sync-badge](https://hackmd.io/9xCN7OOdS7iFJrYN-p8Meg/badge)](https://hackmd.io/9xCN7OOdS7iFJrYN-p8Meg)

## flask runが動かない

可能性として以下が考えられます

- **コマンドを間違えている**
    - `flask --app <pythonファイル名(.py抜き)> run` です。hello以外にしている場合は、それに読み替えてください。`--app` プレフィクスは付けていないときちんと動作しません。
- **flaskのパスが通っていない**
    - flask run実行時に、コマンドがないと怒られる場合があります。その場合は、Python&Flaskを再導入してください。（このとき、パスを通すよう設定をしてください。）
    - 場合によっては、Flask導入時に、管理者権限で実行しているPowershell上で "pip install **-U** Flask" とすることで解決するかもしれません。（※ただしセキュリティ的によろしくないので、可能なら手動でユーザ環境変数にパス追加するのがいいかもしれない）
- **Windows PCのユーザ名 or 絶対パスに日本語がある**
    - ユーザ名が日本語の場合、うまく動作しない開発環境やソフトは結構あります。管理者権限でドライブ直下に英語の作業ディレクトリを置くか、思い切ってWindowsのアカウントを作り直す、フォルダの整理やリネームをする等をお願いします。
    - 確認方法: vscodeで作業フォルダを開いた後ターミナルを開き、 `pwd` と打ちます。赤ラインで示した部分に**絵文字や日本語・全角文字など**が**一文字でも入っていれば**アウトです。(初めの`C:\`だけは別枠。)
![image](https://hackmd.io/_uploads/SytC2fmHC.png)
- **vscodeの▷ボタンを押した、もしくは`python hello.py`を実行した場合**
    - ▷ボタンでは、`python <Yourfile>.py` が実行されます。デフォルトのvscodeではflaskを自動認識してコマンドを変えてくれたりはしません。ですので、自身でターミナルを開いて`flask run`を叩いてください。なお、**どうしても`python`ランタイム上で直で動かしたい場合は、Pythonスクリプトの末尾に以下コードを追記することで、`python`コマンドでも実行することができます。(非推奨)**
```python=hello.py
# 末尾に記載すること（非推奨）
if __name__ == "__main__":
    app.run(debug=True)
```
- **flask runを実行している端末とは異なる端末でアクセスしようとしている、もしくはWSL, Hyper-VやDockerなどlocalhostのネットワーク構成が特殊である場合**
    - Flaskは、基本的に127.0.0.1(実行PC or コンテナ自身)以外からのアクセスをリジェクトします。以下のように、`--host 0.0.0.0` プレフィクスをつけてやることで改善する可能性があります。==**ただし、これはLAN内に自分のWebアプリを公開している状態ですので、公共のWifiやWANに公開されているネットワークでのデバッグ時は基本的にやってはいけません。**==
```powershell=
$ flask --app hello run --host 0.0.0.0
```
- **その他**
    - 出力されたエラーでぐぐりましょう

