## Angularプロジェクト作成
```
npm install -g @angular/cli
// Angular CLIをグローバルインストール（初回のみ必要）

ng new <プロジェクト名> --standalone
// --standaloneフラグを付けることで、従来のAppModuleを使わずに、コンポーネント単体で動かすモダン構成になる
// この構成はAngular v17以降の推奨構成
// スタイルシートはCSS、Server-Side RenderingはNoを選択

cd <プロジェクト名>
ng serve
// angularアプリの起動
```

## ホーム画面の作成
下記コマンドでファイルを生成
```
ng generate component pages/home --standalone
// 画面を作成する際は"pages/"配下に配置
// 複数画面で再利用するパーツは"components/"配下に配置
```



# MyAngularApp

This project was generated using [Angular CLI](https://github.com/angular/angular-cli) version 19.2.7.

## Development server

To start a local development server, run:

```bash
ng serve
```

Once the server is running, open your browser and navigate to `http://localhost:4200/`. The application will automatically reload whenever you modify any of the source files.

## Code scaffolding

Angular CLI includes powerful code scaffolding tools. To generate a new component, run:

```bash
ng generate component component-name
```

For a complete list of available schematics (such as `components`, `directives`, or `pipes`), run:

```bash
ng generate --help
```

## Building

To build the project run:

```bash
ng build
```

This will compile your project and store the build artifacts in the `dist/` directory. By default, the production build optimizes your application for performance and speed.

## Running unit tests

To execute unit tests with the [Karma](https://karma-runner.github.io) test runner, use the following command:

```bash
ng test
```

## Running end-to-end tests

For end-to-end (e2e) testing, run:

```bash
ng e2e
```

Angular CLI does not come with an end-to-end testing framework by default. You can choose one that suits your needs.

## Additional Resources

For more information on using the Angular CLI, including detailed command references, visit the [Angular CLI Overview and Command Reference](https://angular.dev/tools/cli) page.
