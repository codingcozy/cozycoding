---
title: "Langchain 문서 로더 예제로 알아보는 방법"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-26 17:28
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "Langchain Document Loader with Example"
link: "https://medium.com/@patnaik.sankar/langchain-document-loader-with-example-5c831f8095da"
isUpdated: true
updatedAt: 1724742829217
---


LangChain은 여러 문서 로더를 제공하여 다양한 유형의 문서를 애플리케이션으로 손쉽게 가져올 수 있도록 도와줍니다. 이러한 로더들은 다양한 파일 형식을 처리할 수 있도록 설계되어 다양한 소스에서 데이터를 처리하고 통합하는 것을 더 쉽게 만들어줍니다. 아래에서 LangChain에서 제공되는 주요 문서 로더 몇 가지를 살펴보겠습니다:

1. TextLoader

- 목적: 일반 텍스트 파일을 불러옵니다.
- 특징: 인코딩 및 오류 처리를 지정할 수 있는 옵션과 함께 기본 텍스트 파일을 처리합니다.
- 사용 방법

```js
from langchain.document_loaders import TextLoader

loader = TextLoader("nvda_news_1.txt")
documents = loader.load()
documents

## 만약 텍스트 파일에 utf8이 있는 경우
loader = TextLoader("genai_future.txt", encoding='utf-8')
```

<div class="content-ad"></div>

2. PDF로더

- 목적: PDF 파일을 로드하고 처리합니다.
- 특징: 다양한 레이아웃과 구조를 처리하여 PDF에서 텍스트를 추출합니다.
- 사용법

```python
from langchain.document_loaders import PyPDFLoader
loader = PyPDFLoader('2024prq1.pdf') ##2024prq1은 샘플 pdf 파일입니다.
documents = loader.load()
documents
```

3. CSV로더

<div class="content-ad"></div>

- 목적: CSV 파일에서 데이터를 로드합니다.
- 특징: CSV 행을 구조화된 문서로 변환하며, 헤더 및 데이터 처리를 사용자 정의할 수 있습니다.
- 사용 방법

```js
from langchain.document_loaders.csv_loader import CSVLoader
loader = CSVLoader(file_path="movies.csv")
data = loader.load()
data

## source_column을 추가하려면
loader = CSVLoader(file_path="movies.csv", source_column="title")
data = loader.load()
data
data[0].page_content
```

## 4. UnstructuredURLLoader

Langchain의 UnstructuredURLLoader는 내부적으로 URL에서 콘텐츠를 로드하기 위해 비구조적인 python 라이브러리를 사용합니다.

<div class="content-ad"></div>

https://unstructured-io.github.io/unstructured/introduction.html

```js
#필수 라이브러리 설치, libmagic는 파일 유형을 감지하는 데 사용됩니다
#!pip3 install unstructured libmagic python-magi
c python-magic-bin
loader = UnstructuredURLLoader(
    urls = [
        "https://www.moneycontrol.com/news/business/banks/hdfc-bank-re-appoints-sanmoy-chakrabarti-as-chief-risk-officer-11259771.html",
        "https://www.moneycontrol.com/news/business/markets/market-corrects-post-rbi-ups-inflation-forecast-icrr-bet-on-these-top-10-rate-sensitive-stocks-ideas-11142611.html"
    ]
)
loader = UnstructuredURLLoader(urls=urls)

data = loader.load()

data[0]
```

5. HTMLLoader

- 목적: HTML 파일을로드하며 주로 웹 페이지 콘텐츠에 사용됩니다.
- 기능: HTML 내용을 구문 분석하고 텍스트를 추출하며 HTML에 특화된 세부 사항을 처리합니다.
- 사용법

<div class="content-ad"></div>

```js
from langchain_community.document_loaders import UnstructuredHTMLLoader
file_path = "sample1.html"
loader = UnstructuredHTMLLoader(file_path)
data = loader.load()

print(data)
```

6. MarkdownLoader

- 목적: Markdown 파일을 로드합니다.
- 특징: Markdown에서 내용을 추출하면서 헤더 및 목록과 같은 구조를 보존합니다.
- 사용 방법

```js
from langchain_community.document_loaders import UnstructuredMarkdownLoader
loader = UnstructuredMarkdownLoader('README.md',mode="elements", strategy="fast",)
documents = loader.load()
documents[0]
```

<div class="content-ad"></div>

7. DocxLoader

- 목적: Microsoft Word `.docx` 파일을 로드합니다.
- 기능: Word 문서에서 텍스트 및 구조를 추출하며 복잡한 형식을 처리합니다.
- 사용법:

```js
#!pip install --upgrade --quiet  docx2txt
from langchain_community.document_loaders import Docx2txtLoader
loader = Docx2txtLoader('AI Era.docx')
documents = loader.load()
documents
```

<div class="content-ad"></div>

- 목적: JSON 파일에서 데이터를 로드합니다.
- 기능: JSON 필드를 문서 구조로 매핑하여 복잡한 데이터 추출 및 처리를 가능하게 합니다.
- 사용 방법

```js
#!pip install -U jq
#!pip install pathlib
from langchain_community.document_loaders import JSONLoader
import json
from pathlib import Path
file_path='example_2.json'
data = json.loads(Path(file_path).read_text())
loader = JSONLoader(
         file_path=file_path,
         jq_schema='.quiz',
         text_content=False)
documents = loader.load()
documents
```

9. ImageLoader

- 목적: 이미지에서 텍스트를 추출합니다 (OCR).
- 기능: 광학 문자 인식 (OCR)을 사용하여 이미지에서 텍스트를 추출합니다.
- 사용 방법

<div class="content-ad"></div>

```python
from langchain_community.document_loaders import UnstructuredImageLoader
loader = UnstructuredImageLoader('RAG.png', mode="elements", strategy="fast",)
documents = loader.load()
documents
```

10. SQLiteLoader

- 목적: SQLite 데이터베이스에서 데이터를 로드합니다.
- 기능: SQLite 데이터베이스에 대한 쿼리를 실행하고 결과를 문서로 변환합니다.
- 사용법

```python
from langchain.document_loaders import SQLiteLoader
loader = SQLiteLoader('your_database.db')
documents = loader.load()
```  

<div class="content-ad"></div>

TextSplitter

텍스트 스플리터가 처음부터 왜 필요한가요?

LLM은 토큰 제한이 있습니다. 따라서 텍스트를 작은 청크로 나누어야 합니다. 이를 통해 각 청크의 크기가 토큰 제한 이하가 되도록 합니다. 이것을 할 수 있게 해주는 랭체인에는 다양한 텍스트 스플리터 클래스가 있습니다.

```js
text = """인터스텔라는 2014년 크리스토퍼 놀란이 공동으로 기획, 감독 및 제작한 역사적인 공상 과학 영화입니다.
매튜 맥커너히, 앤 해서웨이, 제시카 차스테인, 빌 얼윈, 엘런 버스틴, 맷 데이먼, 마이클 케인이 출연합니다.
인류가 재앙과 기근에 휩싸인 멸망적인 미래를 배경으로 하며, 이 영화는 사토론 근처의 웜홀을 통과하는 우주 비행사들을 따릅니다.

브라더스 크리스토퍼와 조나단 놀란이 2007년에 개발한 대본에서 비롯된 각별한 스크립트를 작성했으며 원래는 스티븐 스필버그의 연출로 준비되었지만 결국 놀란이 연출했습니다.
캘텍 이론 물리학자이자 2017년 노벨 물리학상 수상자인 킵 토른은 이 영화의 실행 제작자이자 과학 컨설턴트로 활약했으며, 이에 대응하는 책인 '인터스텔라의 과학'을 썼습니다.
촬영감독인 호이트 반 호이테마는 파나비전 아나모픽 형식으로 35mm 영화 필름으로 촬영하고 IMAX 70mm로 제작되었습니다. 주요 촬영은 2013년 말에 앨버타, 아이슬란드, 로스앤젤레스에서 이루어졌습니다.
인터스텔라는 실제 및 소형 효과를 적극 활용하고, 더블 네거티브 사가 추가적인 디지털 효과를 만들었습니다.

인터스텔라는 2014년 10월 26일 로스앤젤레스에서 최초 상영되었습니다. 미국에서는 먼저 필름으로 발매되어 디지털 프로젝터를 사용하는 장소로 확대되었습니다. 영화는 비평가들로부터 일반적으로 긍정적인 평가를 받았으며 전 세계에서 6억 7700만 달러 이상(이후 재개봉을 통해 7억 1500만 달러)를 벌어내어 2014년에서 10번째로 많이 벌어들인 영화가 되었습니다.
이 영화는 과학적 정확성과 이론적 천문학의 묘사에 대해 천문학자들로부터 칭찬을 받았으며 제87회 아카데미 시상식에서 다섯 개 부문 후보에 올랐으며 최고의 시각 효과상을 수상하고 다른 다양한 포상도 받았습니다.""" 
```

<div class="content-ad"></div>

데이터를 청크로 나눌 수는 원래의 Python에서 가능하지만 귀찮은 과정일 수 있어요. 또한 필요한 경우, 각 청크가 해당 LLM의 토큰 길이 제한을 초과하지 않도록 보장하기 위해 다양한 구분자로 반복적으로 실험해야 할 수도 있어요.

Langchain은 텍스트 스플리터 클래스를 통해 더 나은 방법을 제공해요.

Langchain의 텍스트 스플리터 클래스 사용하기

## CharacterTextSplitter

<div class="content-ad"></div>


```js
from langchain.text_splitter import CharacterTextSplitter

splitter = CharacterTextSplitter(
    separator = "\n",
    chunk_size=200,
    chunk_overlap=0
)
chunks = splitter.split_text(text)
len(chunks)
for chunk in chunks:
    print(len(chunk))

#As you can see, even though we specified 200 as the chunk size, the chunks created ended up being larger than 200 due to the splitting based on \n.
```

Langchain에서 또 다른 클래스를 사용하여 텍스트를 재귀적으로 분할할 수 있습니다. 이 클래스는 RecursiveTextSplitter입니다. 그것이 어떻게 작동하는지 살펴봅시다.

RecursiveTextSplitter

```js
text
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.text_splitter import RecursiveCharacterTextSplitter

r_splitter = RecursiveCharacterTextSplitter(
    separators = ["\n\n", "\n", " "],  # 요구사항에 따른 구분 기준 목록 (기본값: ["\n\n", "\n", " "])
    chunk_size = 200,  # 생성되는 각 청크의 크기
    chunk_overlap  = 0,  # 컨텍스트 유지를 위해 청크 사이의 겹치는 크기
    length_function = len  # 크기를 계산하는 함수, 현재는 문자열의 길이를 나타내는 "len"을 사용하고 있지만, 다른 토큰 카운터를 전달할 수 있습니다)
)
chunks = r_splitter.split_text(text)

for chunk in chunks:
    print(len(chunk))
```

<div class="content-ad"></div>

LangChain의 이 문서 로더들은 여러 데이터 원본을 자연어 처리 또는 기계 학습 파이프라인에 쉽게 통합할 수 있게 도와줍니다.

샘플 예제를 보시려면 제 GitHub 링크를 참고해주세요.

좋은 학습되세요 👩‍💻