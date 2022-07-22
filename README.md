# 問い合わせフォーム

rdmsys.net向けに作成した、汎用性の高い問い合わせフォームです。

## 特長

- AWS Amplifyにより、サーバ環境の構築が不要
- Vue.js（Nuxt.js）とMailgunのAPIを活用することで、フロントエンドのみでアプリの開発からデプロイまでを実現した
- JSONファイルでフォームの入力項目構成を記述できたことで、vueテンプレートおよび実行ロジックからの分離を実現した
    
    ```json
    [
        {
            "is": "Radio",
            "id": "category",
            "label": "問い合わせの種別",
            "name": "カテゴリ",
            "required": "required",
            "options": [
                "サイトの内容に関する問い合わせ",
                "サイトの不具合に関する問い合わせ",
                "サイト管理者に関する問い合わせ",
                "その他"
            ]
        },
        （中略）
        {
            "is": "Input",
            "type": "email",
            "id": "email",
            "required": "required",
            "label": "メールアドレス",
            "name": "メールアドレス",
            "placeholder": "mailto@address.com"
        },
        {
            "is": "Input",
            "type": "text",
            "id": "name",
            "label": "お名前",
            "name": "名前",
            "placeholder": "例： 佐藤 和夫"
        }
    ]
    ```
    

## 仕様

### 環境定数

mailgunのAPIキーと運営側のメールアドレスは「環境定数」として定義しています。

.envファイルおよびAmplifyの「環境定数」として定義する内容は、.env.exampleに記載しています。

### JSONファイルの中身

| プロパティ | タイプ | 説明 |
| --- | --- | --- |
| is | String | 入力項目コンポーネントを選択します。<br>/components/fm/parts/ 配下にあるフォーム入力項目コンポーネントの名前です。<br>Input, Radio, Checkbox, Select, File, Rating, Textarea |
| type | String | type属性の値。emailの場合は入力項目が電子メールアドレスかどうかをチェックし、適合しないとエラーを発生させます。 |
| name | String | name属性の値で、メール送信時のフォームのキーとして表示されます。 |
| required | String | 必須項目。ラベルの脇に必須のバッジが表示され、未入力の場合「入力必須項目です」のエラーを発生させます。 |
| id | String | フォームのid属性で、jsonファイル内で一意である必要があります。 |
| label | String | 入力画面に表示される、入力項目の名前です。 |
| placeholder | String | placeholderの値として反映されます。 |
| options | Array | Radio、Checkbox、Select、Ratingにおける選択肢の記載を行います。<br>Rating以外の実装方法はBootstrapVueに準じます。<br>Radio: https://bootstrap-vue.org/docs/components/form-radio#radio-group-options-array<br>Checkbox: https://bootstrap-vue.org/docs/components/form-checkbox#checkbox-group-options-array<br>Select: https://bootstrap-vue.org/docs/components/form-select#options-property<br>Ratingの場合は、星の数に応じてテキストがメッセージに反映されます。例えば★1の場合は、options配列の1番目のテキストが送信されます。 |
| accept | String | （Fileコンポーネントのみ）accept属性として反映されます。 |
