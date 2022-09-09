# pot/po形式ファイルの管理（日本語化）

Crowdinサービスと連携して、pot/po形式ファイルをGitHub上で管理します。

1. ソースファイル（pot形式ファイル）のアップロード（GitHub→Crowdin）
1. 日本語翻訳ファイル（po形式ファイル）のダウンロード（Crowdin→GitHub）

# 準備

次の手順で、GitHubとCrowdinの準備を行います。

## GitHubリポジトリの作成

1. GitHubのリポジトリを開きます。  
https://github.com/jp-rad/crowdin-github-ja  
1. `Use this template`をクリックします。  
または、[ここをクリック - https://github.com/jp-rad/crowdin-github-ja/generate](https://github.com/jp-rad/crowdin-github-ja/generate)  
1. リポジトリ名を入力し、リポジトリを作成します。  

## Crowdinプロジェクトの作成

1. [Crowdin](https://crowdin.com/)でプロジェクトを新規作成します。作成時、`Target languages`で`Japanese`（日本語）を指定します。  
※ プロジェクトを作成するには、[crowdin](https://crowdin.com/)へのユーザー登録（SIGN UP）とログイン（LOG IN）が必要です。
1. プロジェクトを作成したら、その`プロジェクトID`を確認して、控えておきます。  
確認方法:  
プロジェクト一覧からプロジェクトのリンク先を開きます。プロジェクト名の下に`ID`が表示されていますが、その値（数値）が`プロジェクトID`です。
1. `パーソナル・アクセス・トークン`も必要ですので、[Account Settings - API](https://crowdin.com/settings#api-key)のページを開き、`Personal Access Tokens`で、新規作成します（`New Token`ボタン）。  
`Token Name`は、重複しない任意の名前を指定します。作成時にはパスワード確認を求められる場合がありますので、ログインユーザーのパスワードを入力し、`Confirm`ボタンで確認してください。  
作成された`パーソナル・アクセス・トークン`（`Access Token`）がダイアログボックスに表示されます。ここでクリップボードにコピーし、値を控えておきます。もし、コピーし忘れた場合は、`Personal Access Tokens`で、再度、新規作成します（`New Token`ボタン）。

## プロジェクトIDとパーソナル・アクセス・トークンの設定

GitHub ActionsからCrowdinプロジェクトに対してアップロードやダウンロードが行われる為、GitHubのリポジトリに`プロジェクトID`と`パーソナル・アクセス・トークン`を設定します。

1. 作成したGitHubのリポジトリを開きます。
1. `Settings`タブの`Actions secrets`ページを開きます（`Secrets`→`Actions`）。
1. `New repository secrets`ボタンで、次の表の２つの項目をそれぞれ追加します。

| # | Name | Secret |
|---|------|--------|
| 1 | CROWDIN_PROJECT_ID | (`プロジェクトID`の値) |
| 2 | CROWDIN_PERSONAL_TOKEN | (`パーソナル・アクセス・トークン`の値) |


# 使い方

次の手順で日本語化を行うことができます。

1. GitHubでソースファイルを管理します（`main`ブランチ）。
1. ソースファイルをCrowdinへアップロードします。
1. Crowdinで、翻訳（日本語化）します。
1. 日本語翻訳ファイルをCrowdinからダウンロードします(`l10n_crowdin_action`ブランチ)。
1. ダウンロードした日本語翻訳ファイルを`l10n_crowdin_action`ブランチから`main`ブランチへマージします。

## ソースファイルの管理

GitHubリポジトリの`main`ブランチで、翻訳元のソースファイルを管理します。ソースファイル(pot形式ファイル)は、`_sources`フォルダ内にコミットしてください。

## ソースファイルのアップロード

GitHubリポジトリからCrowdinプロジェクトへソースファイルをアップロードするには、次の手順でGitHub Actionsを実行します(ダウンロードと兼用)。

1. GitHubリポジトリの`Actions`タブを開きます。
1. `crowdin-action`ワークフローを選択し、`Run workflow`ボタンをクリックします。
1. `Branch:main`を選択し、`Run workflow`ボタンをクリックし、ワークフローを実行します。
1. ワークフローが完了（成功）するとCrowdinプロジェクトへ最新のソースファイルがアップロードされています。

## 翻訳

Crowdinプロジェクト内で、日本語への翻訳を行います。

## 日本語翻訳ファイルのダウンロード

CrowdinプロジェクトからGitHubリポジトリへ日本語翻訳ファイル(po形式ファイル)をダウンロードするには、次の手順でGitHub Actionsを実行します(アップロードと兼用)。

1. GitHubリポジトリの`Actions`タブを開きます。
1. `crowdin-action`ワークフローを選択し、`Run workflow`ボタンをクリックします。
1. `Branch:main`を選択し、`Run workflow`ボタンをクリックし、ワークフローを実行します。
1. ワークフローが完了（成功）すると、`l10n_crowdin_action`ブランチに日本語翻訳ファイルがダウンロードされています（`LC_MESSAGES`ディレクトリ内）。

## マージ（プルリクエスト）

`crowdin-action`ワークフローによって、最新の日本語翻訳ファイルは、`l10n_crowdin_action`ブランチにダウンロードされていますが、同時に、ワークフロー実行時に選択したブランチ（`Branch:main`）に対するプルリクエストも作成されます。

`l10n_crowdin_action`ブランチの`LC_MESSAGES`ディレクトリ内の日本語翻訳ファイルを確認し、GitHubリポジトリの`Pull requests`タブで、プルリクエストの受け入れ操作を行います。

1. `Pull requests`タブを開きます。
1. `New Crowdin translations by Github Action`リクエストを開きます（変更がない場合は、リクエストは作成されません）。
1. `Merge pull request`ボタンをクリックし、マージします。
1. `Confirm merge`ボタンで、マージを完了します。

# 参考

* Github Crowdin Action  
https://github.com/marketplace/actions/crowdin-action
* For more detailed descriptions of these options, 'with'.  
https://github.com/crowdin/github-action/blob/master/action.yml
* Crowdin configuration file, crowdin.yml  
https://developer.crowdin.com/configuration-file/
