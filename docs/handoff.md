---
layout: wiki
title: 接続を試す：図書交換会の箱札
---
# 接続を試す：図書交換会の箱札

架空の図書交換会で、受付係が箱ごとの冊数を登録し、会場係が箱札を印刷するとします。この二つの作業の間で何を約束すれば、会場係は受付係に聞き直さず札を作れるでしょうか。

## 受け渡しの約束（サンプル v1）

受付は「書式の版・箱の名前・数量・単位」を渡します。札側が扱う数量は0以上の整数、単位は「冊」だけです。たとえば `8` が「8箱」でもファイル自体は読めますが、「8冊」と印刷してはいけません。不明な版・単位は札を出さず、入力側へ差し戻します。

## 実際に試す

[架空の入力ファイル](sample.json)を `sample.json` として保存し、同じ場所で Python 3 を使います。通信や追加ライブラリは不要です。

```python
import json
from pathlib import Path

def label(record):
    if not isinstance(record, dict) or type(record.get('schema')) is not int or record['schema'] != 1:
        raise ValueError('未対応の書式')
    if record.get('unit') != '冊' or type(record.get('count')) is not int or record['count'] < 0:
        raise ValueError('数量または単位を確認')
    if not isinstance(record.get('box'), str) or not record['box'].strip():
        raise ValueError('箱の名前が必要')
    return f"{record['box']}：{record['count']}冊"

sample = json.loads(Path('sample.json').read_text(encoding='utf-8'))
print(label(sample))
```

**期待結果：** `こもれび：8冊`。次に `unit` だけを `箱` に変えると `ValueError: 数量または単位を確認` になります。未対応の版 `2` でも札は出ません。エラーを止めずに未記入の札を出すと、入力の問題を会場係が見落とします。

この例で検査できるのはデータの受理・拒否と札の文字列までです。実際の箱に8冊入っているか、札が実用に適するかは別の確認です。入力形式の意味は [PythonのJSONリファレンス](https://docs.python.org/ja/3/library/json.html)も参照してください。
