---
title: "고랭(Golang)으로 OpenSearch 쿼리 빌더 만드는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-21 17:58
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "OpenSearch Query Builder in Golang"
link: "https://medium.com/@confusedcyberwarrior/opensearch-query-builder-in-golang-3beaf9e12884"
isUpdated: true
updatedAt: 1724245646248
---


안녕하세요! 이 페이지를 방문해 주신 모든 분들!

만약 Go 언어로 대용량 데이터 세트를 검색해야 한다면, OpenSearch는 여러분의 강력한 도구가 될 것입니다. 이 빠른 가이드에서는 간단한 영화 데이터베이스를 예시로 활용해 Go에서 복잡한 OpenSearch 쿼리를 구축하는 방법을 안내해 드리겠습니다. 마지막에는 원하는 내용을 정확히 찾을 수 있는 강력한 쿼리를 작성할 수 있을 겁니다.

# 설정하기

시작하려면 필요한 Go 패키지를 설치해야 합니다. Go가 이미 설치되어 있다면, 다음 명령어를 실행하세요:

<div class="content-ad"></div>

```js
go get github.com/opensearch-project/opensearch-go
go get github.com/defensestation/osquery
```

이러한 패키지들은 OpenSearch 인스턴스에 연결하고 쿼리를 효율적으로 작성하는 데 도움이 될 것입니다.

# 영화 데이터베이스 쿼리

영화 데이터베이스가 있다고 가정해보겠습니다. "Christopher Nolan"이 감독한 "Sci-Fi" 태그가 지정된 영화를 검색하고 있습니다. 또한 평균 등급을 계산하고 이 쿼리에서 가장 높게 평가된 영화를 찾고 싶습니다. OpenSearch 쿼리 DSL로 이 작업을 수행하는 방법은 다음과 같습니다:

<div class="content-ad"></div>

```js
GET movies/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "director": "Christopher Nolan"
          }
        }
      ],
      "filter": [
        {
          "term": {
            "genre": "Sci-Fi"
          }
        }
      ]
    }
  },
  "aggs": {
    "average_rating": {
      "avg": {
        "field": "rating"
      }
    },
    "highest_rating": {
      "max": {
        "field": "rating"
      }
    }
  },
  "size": 10
}
```

그리고 해당하는 Go 코드는:

```js
package main

import (
    "context"
    "log"

    "github.com/defensestation/osquery"
    "github.com/opensearch-project/opensearch-go"
)

func main() {
    // OpenSearch에 연결
    osclient, err := opensearch.NewDefaultClient()
    if err != nil {
        log.Fatalf("클라이언트 생성 실패: %s", err)
    }

    // 검색 쿼리 작성 및 실행
    res, err := osquery.Search().
        Query(
            osquery.Bool().
                Must(osquery.Term("director", "Christopher Nolan")).
                Filter(osquery.Term("genre", "Sci-Fi")),
        ).
        Aggs(
            osquery.Avg("average_rating", "rating"),
            osquery.Max("highest_rating", "rating"),
        ).
        Size(10).
        Run(
            osclient,
            osclient.Search.WithContext(context.TODO()),
            osclient.Search.WithIndex("movies"),
        )
    if err != nil {
        log.Fatalf("검색 쿼리 실패: %s", err)
    }

    defer res.Body.Close()

    log.Println("성공적으로 검색 실행됨")
}
```

# 코드 이해하기


<div class="content-ad"></div>

- OpenSearch에 연결: 먼저, 우리는 OpenSearch 인스턴스에 연결합니다. 
- 쿼리 작성: "크리스토퍼 놀란" 감독의 "과학 기술" 장르 영화를 검색하는 쿼리를 생성합니다.  
- 집계 추가: 이 영화들의 평균 평점을 계산하고 그 중에서 가장 높은 평점을 찾기 위해 집계를 포함시킵니다. 
- 쿼리 실행: 마지막으로, 우리는 "movies" 인덱스에서 쿼리를 실행하고 최대 10개의 결과를 검색합니다.

# 결론

Go로 OpenSearch 쿼리를 작성하는 데 필요한 것은 이것뿐입니다! 영화 데이터베이스나 다른 데이터 세트를 검색할 때도 이 접근 방식을 통해 필요한 정확한 정보를 가져올 수 있는 능력이 주어집니다. 여러분의 프로젝트에서 한 번 시도해 보고, Go에서 OpenSearch와 작업하는 것이 얼마나 쉬운지 확인해보세요.

좋은 코딩 되세요! 즐거운 학습 되세요!

<div class="content-ad"></div>

만약 이 글이 유용했다면, 이 글이 도움이 될 수 있는 다른 사람들과 공유해주세요.

만약 제를 지원하고 싶으시다면, 커피 한 잔 사주세요. :)