## 碼表格式說明
- trime碼表由多套輸入方案構成。
- 每套輸入方案包含一個「方案定義」和一個「詞典」文件。不同方案可公用詞典文件。
- 推薦詞典文件使用正體中文，輸入法可設置輸出簡體。
- 在拼音方案不唯一的情況下，詞典文件推薦使用國際音標方案編碼。

### 相關知識
- [yaml]：一種數據描述語言，「方案定義」和「詞典」文件均使用該語言編寫而成。
- [正則表達式]：用於音節切分和各種規則轉換。java中使用的是ICU實現的正則表達式。

### 方案定義文件
方案定義，命名爲 <方案標識>.schema.yaml，是一份包含輸入方案配置信息的YAML文檔。 

    # TRIME schema
    # encoding: utf-8

    schema:
      name: "泰如拼音"
      version: "2014.6"
      author:
        - hisahara
      description: "http://thaerv.8048.my5m.com/"
      dictionary: thaerv
      alphabet: "[a-z0-8]+"
      syllable: "([pmftnlkhcsr]?[shrg]*)?([zviuy]|e[ur]?)?([aeou]|ae?|ei?)?((?<=[aeuoi])[nh])?([0-8])?"
      keyboard:
        - p|t|k|tr|ts|c|i|a|1|3|ph|th|kh|thr|tsh|ch|u|e|2|[57]|m|n|ng|sr|s|sh|y|o|ae|[68]|f|l|h|r|z|v|n|h|eu|er|泰如|四拼
        - 1|2|3|4|5|6|7|8|9|0
      pyspell:
        - /\d+(\d)\b/$1/
        - /h5/h7/
        - /h6/h8/
        - /(ch?|sh)([aeou])/$1i$2/
      py2ipa:
        - /\bng/ŋ/
      ipa2py:
        - /\bŋ/ng/
      ipafuzzy:
        - 陽去歸陰平 (6⇒1)/1/6/

- name: 方案名
- version: 版本號
- author: 發明人、撰寫者。如果您對方案做出了修改，請保留原作者名，並將自己的名字加在後面
- description: 簡要描述方案歷史、碼表來源、方案規則等
- dictionary： 方案使用的詞典文件名
- alphabet: 方案使用的字母。正則表達式。
- syllable: 由字母構成的合法音節，用於音節自動切分。正則表達式。
- keyboard: 鍵盤佈局，支持27、37、40、50鍵，默認爲37鍵。支持多鍵盤，如全拼、雙拼等。
- pyspell: 拼音的拼写規則。用於規範拼寫。如，可將ja自動轉換爲jia。
- py2ipa: 拼音轉音標的規則。檢索時使用。
- ipa2py: 音標轉拼音的規則。反查顯示時使用。
- ipafuzzy: 模糊音的規則。檢索時使用。

### 詞典文件
完全兼容[Rime的詞典文件]，命名爲 <詞典名>.dict.yaml。包含一份碼表及對應的規則說明。詞典文件的前半部份爲一份 YAML 文檔： 

    # Rime dictionary
    # encoding: utf-8
    #
    #

    ---
    name: thaerv
    version: "2014.6.3"
    sort: by_weight
    use_preset_vocabulary: true
    max_phrase_length: 6
    min_phrase_weight: 600
    ...

- name: 詞典名，內部使用，命名原則同「方案標識」；可以與配套的輸入方案標識一致，也可不同；
- version: 管理詞典的版本，規則同輸入方案定義文件的版本號；
- sort: 詞條初始排序方式，可選填 by_weight（按詞頻高低排序）或 original（保持原碼表中的順序）；
- use_preset_vocabulary: 填 true 或 false，選擇是否導入預設詞彙表【八股文】。 
- max_phrase_length: 配合<code>use_preset_vocabulary</code>，設定導入詞條最大詞長
- min_phrase_weight: 配合<code>use_preset_vocabulary</code>，設定導入詞條最小詞頻

[yaml]: http://yaml.org/
[正則表達式]: http://developer.android.com/reference/java/util/regex/Pattern.html
[Rime的詞典文件]: https://code.google.com/p/rimeime/wiki/RimeWithSchemata#碼表與詞典
