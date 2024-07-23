---
title: "Nextjs 14 프로젝트 구조 완벽 이해하기 필수 가이드"
description: ""
coverImage: "/assets/nextjs.png"
date: 2024-07-23 20:01
ogImage: 
  url: /assets/nextjs.png
tag: Tech
originalTitle: "Next.js Project Structure"
link: "https://nextjs.org/docs/getting-started/project-structure"
---


# Next.js 프로젝트 구조

이 페이지는 Next.js 애플리케이션의 프로젝트 구조에 대한 개요를 제공합니다. 이는 최상위 파일 및 폴더, 설정 파일 및 앱 및 페이지 디렉토리 내의 라우팅 규칙을 다룹니다.

각 규칙에 대해 자세히 알아보기 위해 파일 및 폴더 이름을 클릭하세요.

## 최상위 폴더

<div class="content-ad"></div>

Top-level 폴더는 애플리케이션의 코드와 정적 자산을 구성하는 데 사용됩니다.

![이미지](/assets/img/2024-07-23-NextjsProjectStructure_0.png)

| 폴더 | 설명 |
| ---- | ---- |
| [app](/docs/app/building-your-application/routing) | 앱 라우터 |
| [pages](/docs/pages/building-your-application/routing) | 페이지 라우터 |
| [public](/docs/app/building-your-application/optimizing/static-assets) | 제공될 정적 자산 |
| [src](/docs/app/building-your-application/configuring/src-directory) | 선택 사항 애플리케이션 소스 폴더 |

## 최상위 파일

<div class="content-ad"></div>

최상위 파일은 애플리케이션을 구성, 의존성을 관리, 미들웨어를 실행, 모니터링 도구를 통합하고 환경 변수를 정의하는 데 사용됩니다.

Markdown 형식으로 표를 변경하였습니다.

## 앱 라우팅 규칙

다음 파일 규칙은 앱 라우터에서 경로를 정의하고 메타데이터를 처리하는 데 사용됩니다.

<div class="content-ad"></div>

### 라우팅 파일

| 이름 | 확장자 | 설명 |
|---|---|---|
| [layout](/docs/app/api-reference/file-conventions/layout) | `.js` `.jsx` `.tsx` | 레이아웃 |
| [page](/docs/app/api-reference/file-conventions/page) | `.js` `.jsx` `.tsx` | 페이지 |
| [loading](/docs/app/api-reference/file-conventions/loading) | `.js` `.jsx` `.tsx` | 로딩 UI |
| [not-found](/docs/app/api-reference/file-conventions/not-found) | `.js` `.jsx` `.tsx` | 찾을 수 없음 UI |
| [error](/docs/app/api-reference/file-conventions/error) | `.js` `.jsx` `.tsx` | 오류 UI |
| [global-error](/docs/app/api-reference/file-conventions/error#global-errorjs) | `.js` `.jsx` `.tsx` | 전역 오류 UI |
| [route](/docs/app/api-reference/file-conventions/route) | `.js` `.ts` | API 엔드포인트 |
| [template](/docs/app/api-reference/file-conventions/template) | `.js` `.jsx` `.tsx` | 재렌더된 레이아웃 |
| [default](/docs/app/api-reference/file-conventions/default) | `.js` `.jsx` `.tsx` | 병렬 라우트 펄백 페이지 |

### 중첩 라우트

| 폴더 | 설명 |
|---|---|
| [folder](/docs/app/building-your-application/routing#route-segments) | 라우트 세그먼트 |
| [folder/folder](/docs/app/building-your-application/routing#nested-routes) | 중첩된 라우트 세그먼트 |

<div class="content-ad"></div>

### 동적 라우트


|                           |                      |
|---------------------------|----------------------|
|  [folder](/docs/app/building-your-application/routing/dynamic-routes#convention)  |  동적 라우트 세그먼트  |
|  [...folder](/docs/app/building-your-application/routing/dynamic-routes#catch-all-segments)  |  모든 라우트 세그먼트 캐치  |
|  [[...folder]](/docs/app/building-your-application/routing/dynamic-routes#optional-catch-all-segments)  |  선택적 모든 라우트 세그먼트 캐치  |


### 라우트 그룹 및 프라이빗 폴더


|                           |                        |
|---------------------------|------------------------|
|  (folder)(/docs/app/building-your-application/routing/route-groups#convention)  |  라우팅에 영향을 미치지 않은 라우트 그룹  |
|  _folder(/docs/app/building-your-application/routing/colocation#private-folders)  |  폴더 및 모든 하위 세그먼트를 라우팅에서 제외하는 폴더  |


<div class="content-ad"></div>

### 병렬 및 가로막는 라우트

| 기호 | 용도 |
|---------------------|------------------|
| [@folder](/docs/app/building-your-application/routing/parallel-routes#slots) | 이름있는 슬롯 |
| [(.)folder](/docs/app/building-your-application/routing/intercepting-routes#convention) | 동일한 수준 가로막기 |
| [(..)folder](/docs/app/building-your-application/routing/intercepting-routes#convention) | 한 수준 위 가로막기 |
| [(..)(..)folder](/docs/app/building-your-application/routing/intercepting-routes#convention) | 두 수준 위 가로막기 |
| [(...)folder](/docs/app/building-your-application/routing/intercepting-routes#convention) | 루트부터 가로막기 |

### 메타데이터 파일 관례

#### 앱 아이콘

<div class="content-ad"></div>

아래는 Markdown 형식으로 표가 변경되었습니다.

### Open Graph 및 Twitter 이미지

| 사용법                                | 확장자                                       | 설명                   |
| ------------------------------------- | ------------------------------------------- | ---------------------- |
| [opengraph-image](/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-image) | `.jpg` `.jpeg` `.png` `.gif` | Open Graph 이미지 파일   |
| [opengraph-image](/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx) | `.js` `.ts` `.tsx`           | 생성된 Open Graph 이미지 |
| [twitter-image](/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-image) | `.jpg` `.jpeg` `.png` `.gif` | Twitter 이미지 파일      |
| [twitter-image](/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx) | `.js` `.ts` `.tsx`           | 생성된 Twitter 이미지    |

### SEO

<div class="content-ad"></div>


| 이름 | 확장자 | 설명 |
| --- | --- | --- |
| [sitemap](/docs/app/api-reference/file-conventions/metadata/sitemap#sitemap-files-xml) | .xml | 사이트맵 파일 |
| [sitemap](/docs/app/api-reference/file-conventions/metadata/sitemap#generating-a-sitemap-using-code-js-ts) | .js .ts | 생성된 사이트맵 |
| [robots](/docs/app/api-reference/file-conventions/metadata/robots#static-robotstxt) | .txt | 로봇 파일 |
| [robots](/docs/app/api-reference/file-conventions/metadata/robots#generate-a-robots-file) | .js .ts | 생성된 로봇 파일 |

## 페이지 라우팅 규칙

다음 파일 규칙은 페이지 라우터에서 경로를 정의하는 데 사용됩니다.

### 특수 파일


<div class="content-ad"></div>

### 라우트

|               |                   |                          |
|---------------|-------------------|--------------------------|
| 폴더 규칙    |                    |                          |
| [`index`](/docs/pages/building-your-application/routing/pages-and-layouts#index-routes) | `.js` `.jsx` `.tsx` | 홈페이지       |
| [`folder/index`](/docs/pages/building-your-application/routing/pages-and-layouts#index-routes) | `.js` `.jsx` `.tsx` | 중첩 페이지 |
| 파일 규칙   |                   |                           |
| [`index`](/docs/pages/building-your-application/routing/pages-and-layouts#index-routes) | `.js` `.jsx` `.tsx` |  홈페이지       |
| [`file`](/docs/pages/building-your-application/routing/pages-and-layouts) | `.js` `.jsx` `.tsx` | 중첩 페이지 |

### 동적 라우트

<div class="content-ad"></div>


| Folder convention                                                                                          |                               |                               |
|-------------------------------------------------------------------------------------------------------------|-------------------------------|-------------------------------|
| **[folder]/index <!-- -->**                                                                                  | **.js** .jsx **.tsx**         | Dynamic route segment        |
| [folder]<!-- -->/index                                                                                          | .js .jsx .tsx                 | Catch-all route segment      |
| [\[...folder\]]<!-- -->/index                                                                                    | .js .jsx .tsx                 | Optional catch-all route segment  |
| **File convention**                                                                                         |                               |                               |
| [file]                                                                                                      | .js .jsx .tsx                 | Dynamic route segment        |
| [\[...file\]]                                                                                               | .js .jsx .tsx                 | Catch-all route segment      |
| [\[...file\]]                                                                                               | .js .jsx .tsx                 | Optional catch-all route segment  |
