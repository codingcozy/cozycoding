---
title: "여러 HTML 요소간에 고정 위치를 연결하는 방법"
description: ""
coverImage: "/assets/img/2024-08-21-Drawingconnectiveelementsbetweenfixedpositionsonseveralhtmlelements_0.png"
date: 2024-08-21 16:59
ogImage: 
  url: /assets/img/2024-08-21-Drawingconnectiveelementsbetweenfixedpositionsonseveralhtmlelements_0.png
tag: Tech
originalTitle: "Drawing connective elements between fixed positions on several html elements"
link: "https://medium.com/@fixitblog/solved-drawing-connective-elements-between-fixed-positions-on-several-html-elements-cae5ee78cfc1"
isUpdated: true
updatedAt: 1724245963360
---


웹 프로그래밍에는 아직 입문 단계라 좀 서투르게 작업할 수 있어요. 저는 HTML, CSS 및 자바스크립트를 사용하여 다양한 DNA 서열을 시각화하고 있습니다. 각 서열은 HTML 요소의 그래프로 표현되며 DNA 서열의 길이에 해당하는 고정 길이를 갖고 있어요. 각 그래프에서 일부 모티프를 나타내기 위해 고정 지점에 Span이 배치되어 있어요. 웹 프레임워크로는 Django를 사용하고 있어요.

이렇게 하여 여러 서열을 웹페이지에 한 줄씩 만들어 나가고 있어요. 지금 이 서열 중 일부는 서로 유사할 수 있어요. 아래에 표시된 서열과 유사한 부분을 시각적으로 나타내고 싶어요. 위와 같이 작업하고 있어요.

```js
<div class="graph" style='--graph-length: {sequence_length}'>
    <hr class="line">
  { for key, value2 in value.items }
    { if value2.start_position }
        <span class='motif' style="--start: { value2.start_position }; --stop: { value2.stop_position };"></span>
    { endif }
  { endfor }
</div>
```

이렇게 함으로써 웹페이지에 여러 서열을 나란히 표시하고 있어요. 요청하신 그림처럼 아래에 표시된 서열과 유사한 영역을 시각화하여 표시하기 위해 노력하고 있어요. 부족한 부분이 있을 수 있지만 노력하고 있습니다.

<div class="content-ad"></div>

위 예제에서 그래프1과 2, 그리고 그래프2와 3에 의해 표현되는 시퀀스는 유사한 영역을 가지고 있지만 시퀀스의 총 길이(따라서 그래프에 표시된 모티프의 크기)가 다르기 때문에 그래프2와 3 사이의 넓은 파란 요소가 발생합니다.

각 그래프에 대한 것은 각 그래프가 표현해야 하는 시퀀스의 길이, 해당 그래프의 모티프의 시작 및 끝 위치, 그리고 두 그래프(시퀀스)가 유사한 영역(그림에서 녹색)을 포함하는 경우 각 그래프에서의 유사한 영역의 시작 및 끝 위치입니다.

그래서 제 질문은: 두 그래프 사이의 유사한 영역을 나타내는 요소(그림에서 파란색)를 HTML, CSS 또는 JavaScript를 사용하여 어떻게 생성할 수 있을까요? (또는 이미 갖고 있는 것에 구현할 수 있는 다른 모든 것)

어떻게 접근할 수 있는지에 대한 힌트는 언제든지 환영이며, 궁금한 점이 있다면 알려주세요!

<div class="content-ad"></div>

수정: 실제로 제 응용 프로그램에 통합하기에 더 적합하다고 생각하기 때문에 부탁드립니다. 

# 해결책

SVG를 사용하거나 캔버스를 사용할 수 있습니다.

여기 캔버스로의 미니멀한 접근 방식이 있습니다. 아마도 귀하의 것과 유사한 다이어그램을 제공할 가능성이 매우 높은 라이브러리에 의존하는 것이 좋을 것입니다.

<div class="content-ad"></div>

하지만 긁는 것은 결코 아프지 않아요.

아래에서, 각 그래프는 축을 적절하게 조정하는 것이 책임이에요.

마침내, 사다리꼴은 해당하는 꼭지점들의 크기를 조정하기 위해 의존하는 그래프에 의존해요.

텍스트를 더 조건적으로 표시하려면 Bar와 함께 노는 게 좋아하며 표시할지 여부를 나타내는 부울 값을 제공할 수 있어요.

<div class="content-ad"></div>

캔버스의 스타일을 선의 굵기 등으로 더 창의적으로 꾸밀 수도 있지만, 그저 여러분이 쉽게 설정할 수 있다는 데모예요.

```js
const canvas = document.querySelector('canvas')
  let ctx = canvas.getContext('2d')
  let bars = [
    [Bar(200, 300), Bar(1800,2300), Bar(2500, 4500), Bar(5000,5200), Bar(8000,8500)],
    [Bar(1100,1300), Bar(3000, 3800), Bar(4000, 4200), Bar(7000, 7500)],
    [Bar(1, 2700)]
  ]
  function Bar(a,b){return [a, b]}
  class Graph{
    constructor ({anchorY, width, min, max}) {
      this.anchorY = anchorY
      this.width = width
      this.dw = width / (max - min )
      this.min = min
      this.max = max
    }

    plot (bars) {
      // plot bars
      // resize bars dimension to fit canvas
      const e = 5
      ctx.fillStyle = 'orange'
      const scaledBars = bars.map(([a, b]) => [ a, b, a * this.dw, b * this.dw ])
      scaledBars.forEach(([_, dum, left, right])=>{
        ctx.fillRect(left, this.anchorY - e, right - left, 2*e)
      })

      // plot line
      ctx.strokeStyle = 'black'
      ctx.beginPath()
      ctx.moveTo(0, this.anchorY)
      ctx.lineTo(this.width, this.anchorY)
      ctx.closePath()
      ctx.stroke()

      ctx.strokeStyle = 'green'
      ctx.font = '10px serif'
      scaledBars.forEach(([origLeft, origRight, left, right]) => {
        ctx.strokeText(origLeft, left, this.anchorY - 10)
        if (right - left > 100 ) {
          ctx.strokeText(origRight, right, this.anchorY - 10)
        }
      })
      
    }

    //x will be shifted automatically
    moveTo (x) {
      ctx.moveTo(x * this.dw, this.anchorY)
    }


    lineTo (x) {
      ctx.lineTo(x * this.dw, this.anchorY)
    }
  }
  const graphs = [
    new Graph({anchorY:100, width: canvas.width, min: 1, max: 10000}),
    new Graph({anchorY:200, width: canvas.width, min: 1, max: 8500}),
    new Graph({anchorY:300, width: canvas.width, min: 1, max: 4000})
  ]
  
  // g first graph, (a,b) left, right anchors in first graph
  // g2 second graph, c right anchor, d left anchor in second graph
  function trapeze(g, a, b, g2, c, d){
    ctx.beginPath()
    g.moveTo(a)
    g.lineTo(b)
    g2.lineTo(c)
    g2.lineTo(d)
    ctx.closePath()
    ctx.fillStyle = 'rgba(84, 147, 158, 0.5)'
    ctx.fill()
  }

  const [g1, g2, g3] = graphs
  const trapezes = [
    [g1, 1800, 4500, g2, 3800, 1100],
    [g1, 8000, 8500, g2, 7500, 7000],
    [g2, 1100, 3800, g3, 2700, 1],
  ]
  trapezes.forEach(t => trapeze(...t))

  graphs.forEach((g, i) => {
    g.plot(bars[i])
  })
canvas{background:#eeeeee;}
<canvas width="400" height="400"></canvas>
```

<div class="content-ad"></div>

이 답변은 stackoverflow에서 수집한 것이며, cc by-sa 2.5, cc by-sa 3.0 및 cc by-sa 4.0으로 라이센스가 부여되어 있습니다.