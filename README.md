# 環境構築
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

生成されたhome.component.htmlの中身を下記のように修正
```
<h1>ホーム画面</h1>
<p>これはホーム画面です。</p>
```

src/app/app.component.tsの中身を下記のように修正
```
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { HomeComponent } from './pages/home/home.component';

@Component({
  selector: 'app-root',
  imports: [RouterOutlet,HomeComponent],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent {
  title = 'address-app';
}
```

src/app/app.component.htmlの中身を下記のように修正
```
<main class="main">
  <app-home></app-home>
</main>
<router-outlet />
```

以上で作成したHome画面が表示されるようになる。
```
ng serve
```

# 追加機能
## HttpClient関連（API叩くとき）
①src/app.config.tsを下記のように修正
```
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes), // ルーターの定義
    provideHttpClient() // APIを使用する際に必要
  ]
};
```

②src/app/pages/home/home.component.tsに下記のコードを追記
```
import { Component, OnInit } from '@angular/core'; // 追加箇所
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { HttpClient } from '@angular/common/http'; // 追加箇所

@Component({
  selector: 'app-home',
  imports: [CommonModule, FormsModule],
  templateUrl: './home.component.html',
  styleUrl: './home.component.css'
})

// 追記箇所
export class HomeComponent {
  expresswords: string = '';
  error: string = '';

  constructor(private http: HttpClient) {}

  // ngOnInitは画面リロード時に実行されるもの
  ngOnInit(): void {
    this.initTest();
  }

  // ngOnInitで実行する関数(APIあり)
  initTest() {
    const url = `http://localhost:5000/api/init`;
    this.http.get<any>(url).subscribe({
      next: (res) => {
        console.log(res.message);
        this.expresswords = res.message;
      },
      error: () => {
        this.error = 'API通信に失敗';
      }
    })
  }

  // ボタンを押した際に実行する関数（APIあり）
  test() {
    const url = `http://localhost:5000/api/hello`;
    this.http.get<any>(url).subscribe({
      next: (res) => {
        console.log(res);
        console.log(res.message);
        this.expresswords = res.message;
      },
      error: () => {
        this.error = 'API通信に失敗';
      }
    })
  }
}
```

③src/app/pages/home/home.component.htmlを下記のように修正
```
<h1>ホーム画面</h1>
<p>ここでバックエンドとの通信を試みる</p>
<button (click)="test()">バックエンドテスト</button>
<div>
    <p>{{ expresswords }}</p>
</div>
```

## ページ（Component）を増やしてルーティング設定
### 住所検索画面を作成
下記コマンドでファイルを生成
```
ng generate component pages/zipcode-search --standalone
```

src/app/pages/zipcode-search/zipcode-search.component.htmlの中身を下記のように修正
```
<h2>住所検索画面（準備中）</h2>
<p>ここに検索機能を作っていきます。</p>
```

### ルーティング設定（画面遷移）
src/app/app.routes.tsを下記のように修正
```
import { Routes } from '@angular/router';
import { HomeComponent } from './pages/home/home.component';
import { ZipcodeSearchComponent } from './pages/zipcode-search/zipcode-search.component';

export const routes: Routes = [
    { path: '', component: HomeComponent },
    { path: 'search', component: ZipcodeSearchComponent }
  ];
```

src/app/app.component.tsを下記のように修正
```
import { Component } from '@angular/core';
import { RouterLink,RouterOutlet } from '@angular/router';
//import { HomeComponent } from './pages/home/home.component';

@Component({
  selector: 'app-root',
  imports: [RouterLink,RouterOutlet],
  //imports: [RouterOutlet,HomeComponent],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})

export class AppComponent {
  title = 'address-app';
}
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
