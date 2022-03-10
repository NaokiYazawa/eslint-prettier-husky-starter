## 本記事の目標

本記事の目標は、**ESLint・Prettier・Husky を用いてコードの品質を管理出来るようなる** です。

## 本記事の構成

本記事は全部で 4 章から構成されています。以下が各章の内容です。

**第１章**：コードの品質管理  
**第２章**：ESLint・Prettier・Husky について  
**第３章**：TypeScript（with Tailwind CSS）の環境構築  
**第４章**：まとめ

## 第 1 章　コードの品質管理

本章では、コードの品質管理の必要性について説明していきます。

コードの品質管理はなぜ必要なのでしょうか。  
答えは、「**長い目で見た時に、エンジニアの負担を減らすため**」です。（個人的な意見です）

例えば、数人のエンジニアと一緒にゼロからアプリケーションを作るとしましょう。
アプリケーションに様々な機能を付与するにしたがって、コードはみるみる複雑になっていきます。
そして、アプリが無事リリースされた後も、ユーザの要望に答えるべく新たな機能を追加するため、コードはさらに複雑になっていきます。
新たなエンジニアが Join するかもしれません。
コードだけでなく、エンジニアの数も増えます。

このとき、開発者によってコードの書き方が異なっていると何が起こるでしょうか。

**統一されていないコードは、可読性が低くメンテナンス性が悪くなってしまいます**。  
また、開発者はコードを書くときに下記のようなことに悩むかもしれません。

- シングルクォーテーション or ダブルクォーテーション ?
- スペース or タブ ?
- セミコロン必要 or 不要 ?

コードのレビュアーも上記のようなことについて指摘する作業が生まれてしまいます。

統一性のあるコードスタイルが常に保たれていれば、エンジニアはもっとエッセンシャルなことに時間を割くことが出来ます。  
したがって、アプリケーションが大規模になればなるほど、コードの品質管理は重要となります。

しかし、

1. 開発者がコードスタイルを意識することなくコーディングに集中できる
2. 統一性の高いコードを作成できる

という 2 つを同時に実現することは可能なのでしょうか。  
（これらを手作業で行うのは費用対効果が得られそうにないので、できれば**自動**で行いたい！）

答えは「YES」です。そんな便利なツールを次の章で紹介します。

## 第 2 章　 ESlint・Prettier・Husky について

本章では、第１章で挙げた

1. 開発者がコードスタイルを意識することなくコーディングに集中できる
2. 統一性の高いコードを作成できる

を実現する ESlint・Prettier・Husky について説明していきます。

### 🟪 ESLint 🟪

ESLint とは、Node.js で書かれた JavaScript 専用の linter です。

> linter とは  
> コードを静的解析しコンパイラーで検出されない潜在的なバグを警告するプログラムです。linter を導入することでコーディングの品質を高めることが出来ます。

> 静的解析とは  
> **ソースコードを動作させず**に字句的に解析し、品質を測定する手法です。  
> 参考：[あきらめるにはまだ早い！ソースコードの品質向上に効果的なアプローチ](https://qiita.com/hirokidaichi/items/5a5cb63ef5499143bc40)

JavaScript は、動的で緩い型付けの言語であるため、特に開発者のミスが起こりやすいです。
ESLint を使うことによってコーディング規約を設定することができ、これらのエラーを最小限にとどめることができます。

### 🟨 Prettier 🟨

Prettier とは、決めたルールに従い、ソースコードを適切に**整形**してくれるツールです。
ESLint と役割を混同しがちですが、ESLint の役割はコードの構文チェックであり、Prettier の役割はコードの整形になります。

### 🟩 Husky 🟩

Husky とは、Git Commit する前、Git Push する前などにスクリプトを実行することができるツールです。
Husky を使うことによって、ESLint と Prettier を自動で動かすことが出来ます。

## 第 3 章　 TypeScript（with Tailwind CSS）の環境構築

本章では、ESLint × Prettier × Husky を使った TypeScript（with Tailwind CSS）の環境構築について説明します。

### 1. Visual Studio Code プラグインのインストール

まずは、ESLint と Prettier のプラグインをインストールしましょう。  
Visual Studio Code を開き「command+shift+x」で拡張機能を検索してください。  
以下 2 つのプラグインをインストールすれば OK です。

【ESLint】

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/e3d3cde2-8212-6830-3819-bc83b7691a1a.png)

【Prettier - Code formatter】

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/7ac00aa0-ddf1-c331-7ee7-ddfc708d48a0.png)

### 2. プロジェクトの作成

今回は、Next.js のプロジェクトを TypeScript で作成したいと思います。
アプリケーションの名前は「blog」としました。

```bash
$ npx create-next-app blog --typescript
```

上記のコマンドを打つと、

```bash
Installing devDependencies:
- eslint
- eslint-config-next
- typescript
- @types/react
- @types/node
```

と出力されるように、eslint・eslint-config-next・typescript・@types/react・@types/node が自動でインストールされます。

それでは、今作成したアプリに移動しましょう。

```bash
$ cd blog
```

### 3. eslint の設定

それでは、eslint の設定を行っていきます。  
下記コマンドを実行してください。

```bash
$ npx eslint --init
```

すると下記の表示が出るので、「y」を入力して enter を押してください。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/93f48d3c-579a-f015-cbba-954c4dd496c5.png)

これより先は、下記の画像の通りに選択してください。  
（矢印キーで選択肢を変更、enter で決定）

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/4d8ee87d-4a82-9067-1390-968f5baee16a.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/accf0fd3-5d2d-097e-2a16-ba49db60b458.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/07211ece-3aa1-3a36-0ca4-b6b46970606c.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/29937146-1b35-1e84-3335-9d0d2caf3958.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/97bef76c-cb75-462e-859c-d8b943a27738.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/48c231f2-fda1-08c7-1f9e-73865db7ac9d.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/c9d0410a-3383-d82c-74e1-d08d591effa0.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/3a6667a1-be54-c93e-8d23-30fd33ab13d4.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/d1c7ca9a-b8d1-a65e-d33e-00d91bf43d3e.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/83e6030a-1d68-d67c-08c1-0d9832821290.png)

上記が完了すると、**.eslintrc.js** が生成され下記の内容が記述されているはずです。

```javascript:.eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true
  },
  extends: [
    'plugin:react/recommended',
    'standard'
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 'latest',
    sourceType: 'module'
  },
  plugins: [
    'react',
    '@typescript-eslint'
  ],
  rules: {
  }
}
```

早速ですが、react の version を指定しておきましょう。

```javascript:.eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: ['plugin:react/recommended', 'standard'],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {},
  // 下記を追加
  settings: {
    react: {
      version: 'latest'
    }
  }
};
```

そして、ESLint のコーディング規約を適用しないファイル・フォルダを設定しておきましょう。  
**.eslintignore** を作成して下記を記述してください。

```bash
$ touch .eslintignore
```

```json:.eslintignore
.next
next-env.d.ts
node_modules
yarn.lock
package-lock.json
public
```

上記の設定はあくまでも例ですので、ご自身のプロジェクトに合わせて変更してください。

### 4. prettier の設定

次に prettier の設定を行っていきます。  
下記コマンドを実行してください。

```bash
$ npm i --save-dev prettier
```

これで prettier がインストールされました。
次に、下記コマンドも実行してください。

```bash
$ npm i --save-dev eslint-config-prettier
```

【eslint-config-prettier をインストールする理由】  
eslint などのリンターにはコーディング規約だけでなく、コードをフォーマットする機能も含まれています。  
これらフォーマットの機能を無効化して機能の競合を防ぎ、prettier にフォーマットを任せるように設定を行うため eslint-config-prettier を導入する必要があります。

eslint-config-prettier をインストール後に、**.eslintrc.js** の extends を編集してください。

```javascript:.eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  // 下記の extends に「'prettier'」を追加（必ず最後に追加してください）
  extends: ['plugin:react/recommended', 'standard', 'prettier'],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {},
  settings: {
    react: {
      version: 'latest',
    },
  },
};
```

それでは、prettier の設定を行っていきます。  
**.prettierrc** を作成してください。

```bash
$ touch .prettierrc
```

下記のように、prettier の設定を記述します。

```javascript:.prettierrc
{
  "endOfLine": "lf",
  "printWidth": 80,
  "tabWidth": 2,
  "trailingComma": "es5",
  "singleQuote": true,
  "jsxSingleQuote": true,
  "semi": true
}
```

上記の設定はあくまでも例ですので、ご自身のプロジェクトに合わせて変更していただいて構いません。

そして、ESLint と同様に Prettier のコードフォーマットを適用しないファイル・フォルダを設定しておきましょう。  
**.prettierignore** を作成して下記を記述してください。

```bash
$ touch .prettierignore
```

```json:.prettierignore
.next
next-env.d.ts
node_modules
yarn.lock
package-lock.json
public
```

最後に、Visual Studio Code 上で保存時に prettier のコードフォーマットを発動させるための設定を行います。

「command + ,」で Settings を開き、「Default Formatter」と検索してください。  
そして、下記のように **Prettier - Code formatter** を選択してください。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/aa9496c4-a3de-e0af-a858-f1573b969fe5.png)

次に、「Format on Save」と検索してください。  
そして、下記のように **Format On Save** にチェックを入れてください。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/be8fc362-2f65-d767-4eb7-5e2a275381a0.png)

これで、ファイル保存時に prettier によってコードが自動的にフォーマットされます。

また、**.vscode** ディレクトリ内に **settings.json** を作成し、今設定した事項を記述しましょう。  
こうすることで、GitHub などから clone したときに **settings.json** の設定が適用されるようになります。

```bash
$ mkdir .vscode
$ touch .vscode/settings.json
```

```json:settings.json
{
  "editor.formatOnPaste": true,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.fixAll.format": true
  }
}
```

### 5. ESLint の rules の変更

試しに、**index.tsx** の保存を実行してみましょう。  
prettier のコードフォーマットが実行され、ダブルクォーテーションがシングルクォーテーションに修正されています。（便利！）  
しかし、下記のような ESLint のエラーが表示されると思います。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/dfeb5ae2-261d-08d6-2745-816043c72c80.png)

上記のエラーは React を import すれば解消します。
しかし、NEXT.js を使用している場合、ファイルの先頭で React をインポートする必要はないので、今回は ESLint の rules の方を変更しましょう。

```javascript:.eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: ['plugin:react/recommended', 'standard', 'prettier'],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {
    // 下記を追加
    'react/react-in-jsx-scope': 'off',
  },
  settings: {
    react: {
      version: 'latest',
    },
  },
};
```

これで、index.tsx のエラーが解消されるはずです。

```bash
$ mkdir .vscode
$ touch .vscode/settings.json
```

### 6. Husky の設定

最後に、Git Commit 時に ESLint と Prettier による静的解析を走らせるために Husky の設定を行っていきます。

はじめに下記コマンドを実行してください。

```bash
$ npx husky-init
```

すると下記のようなログが表示されます。  
これで、コミット時に **.husky/pre-commit** というファイルが実行されるようになりました。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/c4bd952e-f01f-514a-c3ff-e693d186d0f9.png)

それでは、「コミット時に ESLint と Prettier による静的解析を走らせる」という設定を **pre-commit** ファイルに記述していきます。

```bash:pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# bash echo color
RED='\033[1;31m'
GREEN='\033[1;32m'
BLUE='\033[1;36m'
BOLD='\033[1;37m'
NC='\033[0m'

echo "\n 🚧🏗️  ${BOLD}Checking format, lint and types in your project before committing${NC}"

# Check Prettier standards
npm run check-format ||
(
    echo "\n ❌🟨 Prettier Check ${RED}Failed${NC}. 🟨❌\n Run ${BLUE}npm run format${NC}, add changes and try commit again.\n";
    false;
)

# Check ESLint Standards
npm run check-lint ||
(
    echo "\n ❌🟪 ESLint Check ${RED}Failed${NC}. 🟪❌\n Make the required changes listed above, add changes and try to commit again.\n"
    false;
)

# Check tsconfig standards
npm run check-types ||
(
    echo "\n ❌🟦 Type check ${RED}Failed${NC}. 🟦❌\n Make the changes required above.\n"
    false;
)

# If everything passes... Now we can build
echo "🔥🚀 ${BOLD}All passed... Now we can build.${NC} 🚀🔥"

npm run build ||
(
    echo "\n ❌🟩 Next build ${RED}Failed${NC}. 🟩❌\n View the errors above to see why.\n"
    false;
)

# If everything passes... Now we can commit
echo "✅✅ ${GREEN}Build is completed... I am committing this now.${NC} ✅✅\n"
```

最後に

- **npm run check-format**：Prettier で定義したフォーマットの検証
- **npm run check-lint**：ESLint で定義したフォーマットの検証
- **npm run check-types**：TypeScript の型検証

を **package.json** に定義しましょう。

```bash
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  "prepare": "husky install",
  "check-types": "tsc --pretty --noEmit",
  "check-format": "prettier --check .",
  "check-lint": "eslint . --ext ts --ext tsx --ext js",
  "format": "prettier --write .",
  "test-all": "npm run check-format && npm run check-lint && npm run check-types"
},
```

最終的なフォルダ構成は下記のようになります。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2279509/fba70197-331e-8794-981b-e4d1f0e224f5.png)

## 第 4 章　まとめ

お疲れ様でした。本記事では、ESLint × Prettier × husky を使った TypeScript の環境構築について解説しました。  
ESLint × Prettier × husky という組み合わせは、あなたのコードを高品質に保ってくれることでしょう。

最後まで読んでいただき有難うございます。
もしも誤植等ありましたらコメントしていただけると幸いです。
