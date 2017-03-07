# Autotrack [![Build Status](https://travis-ci.org/googleanalytics/autotrack.svg?branch=master)](https://travis-ci.org/googleanalytics/autotrack)

- [개요](#개요)
- [플러그인](#플러그인)
- [설치 및 사용법](#설치-및-사용법)
  - [NPM을-통한-Autotrack-로드](#NPM을-통한-Autotrack-로드)
  - [옵션 설정하기](#옵션-설정하기)
- [고급  설정](#고급-설정)
  - [커스텀 빌드](#커스텀-빌드)
  - [복수의 Tracker와 함께 Autotrack 사용하기](#복수의-Tracker와-함께-Autotrack-사용하기)
- [브라우져 지원](#브라우져-지원)
- [번역](#번역)

## 개요

Google Analytics의 기본 [기본 JavaScript 추적 코드] (https://developers.google.com/analytics/devguides/collection/analyticsjs/)는 웹 페이지가 처음 로드되고 페이지 뷰가 Google Analytics로 전송 될 때 실행 됩니다. 페이지 뷰 이외의 정보 (예 : 사용자가 클릭 한 위치, 스크롤 한 거리, 특정 요소를 보았던 위치 등)에 대해 알고 싶으면 직접 정보를 획득하는 코드를 작성해야 합니다.

대부분의 웹 사이트 소유자는 동일한 유형의 사용자 상호 작용을 중요하게 생각하므로 웹 개발자는 개발하는 모든 새 사이트에 대해 동일한 추적 코드를 반복 작성합니다.

Autotrack은 이 문제를 해결하기 위해 만들어졌습니다. 대부분의 사람들이 관심 있어하는 상호 작용에 대한 기본 추적 기능을 제공하며 사람들이 사이트와 상호 작용하는 방식을 이전보다 훨씬 쉽게 이해할 수 있도록 여러 가지 편리한 기능 (예 : 선언적 이벤트 추적)을 제공 합니다.

## 플러그인

이 Repository에 있는`autotrack.js` 파일은 작고 (7K gzipped) 모든 플러그인이 포함 되어 있습니다. 당신은 그것을 그대로 사용하거나, 사용하고 싶은 플러그인 만 포함하는 더 작은 [커스텀 빌드] (# 커스텀-빌드)를 만들 수 있습니다.

다음 표는 각 플러그인이 수행하는 작업을 간략하게 설명합니다. 플러그인 이름을 클릭하면 전체 설명서와 사용법을 볼 수 있습니다.:

<table>
  <tr>
    <th align="left">플러그인</th>
    <th align="left">설명</th>
  </tr>
  <tr>
    <td><a href="/docs/plugins/clean-url-tracker.md"><code>cleanUrlTracker</code></a></td>
    <td>Google Analytics에 표시 되는 URL 경로의 일관성을 보장합니다. 페이지 보고서의 여러 개별 행이 실제로는 같은 페이지를 가리키는 문제를 피하십시오.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/event-tracker.md"><code>eventTracker</code></a></td>
    <td>HTML 속성 마크업을 통해 선언적 이벤트 추적을 활성화합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/impression-tracker.md"><code>impressionTracker</code></a></td>
    <td>Viewport에서 요소가 표시되는 시점을 추적 할 수 있습니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/max-scroll-tracker.md"><code>maxScrollTracker</code></a></td>
    <td>사용자가 페이지 내에서 하단으로 스크롤 하는 거리를 자동으로 추적합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/media-query-tracker.md"><code>mediaQueryTracker</code></a></td>
    <td>페이지의 미디어 쿼리 매칭 및 미디어 쿼리에 의한 변경 추적을 사용합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/outbound-form-tracker.md"><code>outboundFormTracker</code></a></td>
    <td>외부 도메인으로의 Form Submit을 자동으로 추적합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/outbound-link-tracker.md"><code>outboundLinkTracker</code></a></td>
    <td>외부 도메인으로의 클릭을 자동으로 추적합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/page-visibility-tracker.md"><code>pageVisibilityTracker</code></a></td>
    <td>페이지가 실제 표시되고 있는 상태(백그라운드 탭에 페이지가 존재하는 상태의 반대)의 시간을 자동으로 추적합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/social-widget-tracker.md"><code>socialWidgetTracker</code></a></td>
    <td>공식 Facebook 및 Twitter 위젯과의 사용자 상호 작용을 자동으로 추적합니다.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/url-change-tracker.md"><code>urlChangeTracker</code></a></td>
    <td>단일 페이지 어플리케이션(Single Page Application)의 URL 변경 사항을 자동으로 추적합니다.</td>
  </tr>
</table>

**책임의 한계:** autotrack은 Google Analytics 개발자 플랫폼 팀의 멤버가 관리하며 주로 개발자를  대상으로합니다. 공식 Google Analytics 제품이 아니며 Google Analytics 360 지원 자격이 없습니다. 이 라이브러리를 사용하기로 선택한 개발자는 자신의 구현이 [Google Analytics 서비스 약관] (https://www.google.com/analytics/terms/us.html)의 요구 사항과 그들이 속한 나라의 법적 책임에 부합하는지 확인하여야 합니다.

## 설치 및 사용법

사이트에 autotrack을 추가하려면 다음의 두 가지 작업을 수행해야합니다.:

1. 이 Repository (또는 [커스텀 빌드] (# 커스텀-빌드))에 포함 된`autotrack.js` 스크립트 파일을 페이지에 로드하십시오.
2. 당신의 [추적코드](https://developers.google.com/analytics/devguides/collection/analyticsjs/tracking-snippet-reference)가  [tracker](https://developers.google.com/analytics/devguides/collection/analyticsjs/creating-trackers)를 사용 할 때 당신이 사용하고자 하는 autotrack의 다양한 플러그인들을  [요구하도록](https://developers.google.com/analytics/devguides/collection/analyticsjs/using-plugins) 수정 해주세요.

당신의 사이트가 현재 [기본 JavaScript 추적 코드] (https://developers.google.com/analytics/devguides/collection/analyticsjs/tracking-snippet-reference)를 사용하고 있다면 다음과 같이 수정할 수 있습니다.:

```html
<script>
window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
ga('create', 'UA-XXXXX-Y', 'auto');

// Replace the following lines with the plugins you want to use.
ga('require', 'eventTracker');
ga('require', 'outboundLinkTracker');
ga('require', 'urlChangeTracker');
// ...

ga('send', 'pageview');
</script>
<script async src='https://www.google-analytics.com/analytics.js'></script>
<script async src='path/to/autotrack.js'></script>
```

물론, 위의 코드를 다음과 같이 수정하여 autotrack을 필요에 맞게 사용자 정의 해야합니다.:

- `UA-XXXXX-Y`를 당신의 [추적코드](https://support.google.com/analytics/answer/1032385)로 변경하세요.
- 플러그인`require` 문의 샘플 목록을 사용하려는 플러그인으로 대체하세요.
- `path/to/autotrack.js`를 실제 서버에서 제공되고 있는 `autotrack.js` 실제 위치로 변경 하세요

**참고:** [analytics.js plugin system] (https://developers.google.com/analytics/devguides/collection/analyticsjs/using-plugins)은 비동기적으로 로드 된 스크립트를 지원 하도록 설계 되었으므로, 'autotrack.js `는 analytics.js 이전 또는 이후에 로드해도 됩니다. `autotrack.js` 라이브러리가 개별적으로 로드되거나 나머지 JavaScript 코드와 번들로 묶여 있는지 여부는 중요하지 않습니다.

### NPM을-통한-Autotrack-로드

npm과 [ES2015 imports] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)를 이해하는 모듈 로더 (예 : [Webpack] (https : /) /webpack.js.org/), [Rollup] (http://rollupjs.org/) 또는 [SystemJS] (https://github.com/systemjs/systemjs))를 사용하여 빌드에 autotrack을 포함시킬 수 있습니다. 다른 npm 모듈처럼 가져 오는 방법 :

```sh
npm install autotrack
```

```js
// In your JavaScript code
import 'autotrack';
```
**참고:** autotrack의 소스는 ES2015로 게시되어 있으므로 컴파일에서 제외시키지 않아야합니다. 자세한 내용은 [# 137] (https://github.com/googleanalytics/autotrack/issues/137)을 참조하십시오.

위의 `import` 문은 생성 된 소스 파일에있는 모든 autotrack 플러그인을 포함합니다. 특정 플러그인 세트 만 포함하려는 경우 플러그인을 개별적으로 가져올 수 있습니다:

```js
// In your JavaScript code
import 'autotrack/lib/plugins/event-tracker';
import 'autotrack/lib/plugins/outbound-link-tracker';
import 'autotrack/lib/plugins/url-change-tracker';
```

위의 예제는 사이트의 기본 JavaScript 번들에 autotrack 플러그인 소스를 포함시키는 방법을 보여줍니다. [2 단계 설치 프로세스] (# 설치 및 사용)의 첫 단계를 수행합니다. 그러나 추적 코드를 업데이트하고 추적기에 사용할 플러그인이 필요합니다.

```js
// Import just the plugins you want to use.
import 'autotrack/lib/plugins/event-tracker';
import 'autotrack/lib/plugins/outbound-link-tracker';
import 'autotrack/lib/plugins/url-change-tracker';

ga('create', 'UA-XXXXX-Y', 'auto');

// Only require the plugins you've imported above.
ga('require', 'eventTracker');
ga('require', 'outboundLinkTracker');
ga('require', 'urlChangeTracker');

ga('send', 'pageview');
```

#### 코드 분할

분석은 보통 애플리케이션 중요한 기능이 아니기 때문에 사이트의 주요 JavaScript 번들의 일부로 분석을 포함 시키는 것은 일반적으로 바람직하지 않습니다.

코드 분할을 지원하는 bundler를 사용 한다면 autotrack 플러그인을 느리게 로드하고 사이트의 중요 기능이 로드 될 때까지 초기화를 지연시키는 것이 좋습니다 :

```js
window.addEventListener('load', () => {
  const autotrackPlugins = [
    'autotrack/lib/plugins/event-tracker',
    'autotrack/lib/plugins/outbound-link-tracker',
    'autotrack/lib/plugins/url-change-tracker',
    // List additional plugins as needed.
  ];

  Promise.all(autotrackPlugins.map((x) => System.import(x))).then(() => {
    ga('create', 'UA-XXXXX-Y', 'auto');

    ga('require', 'eventTracker', {...});
    ga('require', 'outboundLinkTracker', {...});
    ga('require', 'urlChangeTracker', {...});
    // Require additional plugins imported above.

    ga('send', 'pageview');
  });
})
```

빌드 설정 시 코드 분할을 사용하는 방법을 잘 모르겠다면 [커스텀 빌드] (# 커스텀 빌드) 섹션에서 필요한 플러그인만으로 autotrack의 커스텀 버전을 생성하는 방법을 참조 하십시오.

### 옵션 설정하기

모든 autotrack 플러그인은 설정 객체를`require` 명령의 세 번째 매개 변수로 받아들입니다.

일부 플러그인 (예 :`outboundLinkTracker`,`socialWidgetTracker`,`urlChangeTracker`)에는 옵션을 설정하지 않고 작동하는 기본 동작이 있습니다. 다른 플러그인 (예 :`cleanUrlTracker`,`impressionTracker`,`mediaQueryTracker`)은 동작을 위해 특정한 옵션을 설정해야합니다.

플러그인 별 옵션을 참조하려면 개별 플러그인 설명을 참조 하십시오 (기본값이 존재한다면, 기본값이 무엇인지도 표시되어 있습니다.).

## 고급 설정

### 커스텀 빌드

Autotrack은 자체 빌드 시스템과 함께 제공 되므로 필요한 플러그인만 포함하는 autotrack 번들을 만들 수 있습니다. [NPM을-통한-Autotrack-로드] (#NPM을-통한-Autotrack-로드) 를 하였다면,`autotrack` 명령을 실행하여 커스텀 빌드를 생성 할 수 있습니다.

예를 들어, 다음 명령은`eventTracker`,`outboundLinkTracker` 및`urlChangeTracker` 플러그인에 대한`autotrack.js` 번들과 source map을 생성 합니다 ::

```sh
autotrack -o path/to/autotrack.custom.js -p eventTracker,outboundLinkTracker,urlChangeTracker
```

이 파일이 생성되면 HTML 템플릿에 `analytics.js`를 로드 할 수 있습니다. 두 스크립트 태그에 모두 'async` 속성을 사용 합니다. 이렇게 하면`analytics.js`와`autotrack.custom.js`가 나머지 사이트 컨텐츠 로딩을 방해하지 않게 됩니다.

```html
<script async src='https://www.google-analytics.com/analytics.js'></script>
<script async src='path/to/autotrack.custom.js'></script>
```

### 복수의 Tracker와 함께 Autotrack 사용하기

모든 autotrack 플러그인은 [multiple trackers] (https://developers.google.com/analytics/devguides/collection/analyticsjs/creating-trackers#working_with_multiple_trackers)를 지원하며`require` 명령에서 tracker 이름을 지정하여 작동합니다. 다음 예제에서는 두 개의 추적기를 만들고 각각에 여러 가지 autotrack 플러그인을 로드 합니다.

```js
// Creates two trackers, one named `tracker1` and one named `tracker2`.
ga('create', 'UA-XXXXX-Y', 'auto', 'tracker1');
ga('create', 'UA-XXXXX-Z', 'auto', 'tracker2');

// Requires plugins on tracker1.
ga('tracker1.require', 'eventTracker');
ga('tracker1.require', 'socialWidgetTracker');

// Requires plugins on tracker2.
ga('tracker2.require', 'eventTracker');
ga('tracker2.require', 'outboundLinkTracker');
ga('tracker2.require', 'pageVisibilityTracker');

// Sends the initial pageview for each tracker.
ga('tracker1.send', 'pageview');
ga('tracker2.send', 'pageview');
```

## 브라우져-지원

Autotrack은 잠재적으로 브라우져에서 지원되지 않는 코드에 대한 기능 감지가 적용되므로 오류 없이 모든 브라우저에서 안전하게 실행됩니다. 그러나 autotrack은 실행 중인 브라우저에서 지원되는 기능 만 추적합니다. 예를 들어 Internet Explorer 8을 실행하는 사용자는 Internet Explorer 8에서 미디어 쿼리 자체가 지원되지 않으므로 미디어 쿼리 사용을 추적 할 수 없습니다.

모든 자동 추적 플러그인은 다음 브라우저에서 테스트 되었습니다.:

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/alrra/browser-logos/39.2.2/src/chrome/chrome_48x48.png" alt="Chrome"><br>
      ✔
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/alrra/browser-logos/39.2.2/src/firefox/firefox_48x48.png" alt="Firefox"><br>
      ✔
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/alrra/browser-logos/39.2.2/src/safari/safari_48x48.png" alt="Safari"><br>
      6+
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/alrra/browser-logos/39.2.2/src/edge/edge_48x48.png" alt="Edge"><br>
      ✔
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/alrra/browser-logos/39.2.2/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png" alt="Internet Explorer"><br>
      9+
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/alrra/browser-logos/39.2.2/src/opera/opera_48x48.png" alt="Opera"><br>
      ✔
    </td>
  </tr>
</table>

## 번역

다음 번역은 커뮤니티에서 자발적으로 제공 한 것입니다. 이 번역들은 비공식적이며 부정확하거나 현재 유효하지 않은 정보를 담고 있을 수 있습니다..:

* [중국어](https://github.com/stevezhuang/autotrack/blob/master/README.zh.md)
* [프랑스어](https://github.com/DirtyF/autotrack/tree/french-translation)
* [일본어](https://github.com/nebosuker/autotrack)
* [폴란드어](https://github.com/krisu7/autotrack)

특정 번역본에 문제가 있는 경우 적절한 번역본을 제출하십시오. 자신의  번역을 제출하려면 다음 단계를 따르십시오.:

1. 이 repository를 Fork 하세요..
2. Fork 한 저장소의 설정에서 [이슈를 허용 하세요.](http://programmers.stackexchange.com/questions/179468/forking-a-repo-on-github-but-allowing-new-issues-on-the-fork).
3. 문서 파일을 제외한 모든 파일을 제거 하세요.
4. 번역된 버젼으로 문서 파일들을 업데이트 하세요.
5. 당신의 Fork를 위의 리스트에 추가하고 Pull request를 제출하세요.