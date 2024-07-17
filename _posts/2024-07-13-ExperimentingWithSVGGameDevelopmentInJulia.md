---
title: "Julia에서 SVG 게임 개발을 실험하는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-ExperimentingWithSVGGameDevelopmentInJulia_0.png"
date: 2024-07-13 21:40
ogImage: 
  url: /assets/img/2024-07-13-ExperimentingWithSVGGameDevelopmentInJulia_0.png
tag: Tech
originalTitle: "Experimenting With SVG Game Development In Julia"
link: "https://medium.com/chifi-media/experimenting-with-svg-game-development-in-julia-77a5fdf4b8d7"
---


![이미지](/assets/img/2024-07-13-ExperimentingWithSVGGameDevelopmentInJulia_0.png)

## 소개

요즘은 Julia를 사용하여 게임을 만드는 잠재력에 대해 고민해 왔습니다. 확실한 것은, 전혀 게임을 즐기는 유형은 아닌데요 — 심즈 2에만 새밧을 뜨기도 했지만, 전반적으로 비디오 게임에 대해 훨씬 더 많은 엔지니어적인 관점을 가지고 관심을 갖는 편입니다 — 비디오 게임은 거대하고 복잡하며 흥미진진한 소프트웨어 덩어리입니다.

어떤 언어든 언어의 한계와 사용 방법이 있습니다. 당연히 Julia도 마찬가지입니다. Julia는 게임 개발에 적용할 때 자연스레 많은 제한 사항을 가지고 있습니다. 그럼에도 불구하고 나는 '안 된다'고 말 받기를 좋아하지 않아요 — 경량 Julia 게임 개발을 위한 몇 가지 멋진 옵션이 있습니다. 그러나 Component 기반 및 Toolips, ToolipsSession, ToolipsSVG 및 기타 확장이 제공하는 다양한 기능을 고려할 때, JLChat를 구축할 때 사용한 것과 유사한 클라이언트-서버 rpc 시스템을 만드는 가능성에 대해 실험해보고 싶었습니다. 해당 기사를 놓쳤다면, 이 링크를 참조하세요: [링크](링크 주소)

<div class="content-ad"></div>

다음은 오늘 작업할 "게임 엔진" 프로젝트 Vulta.jl에 대한 링크입니다:

[Vulta.jl](http://www.example.com/vulta)

다음은 Vulta.jl로 구축할 예제 프로젝트에 대한 링크입니다:

[Example Project](http://www.example.com/example-project)

명확하게 말하면, 이것은 부수적인 프로젝트입니다. 저는 이미 충분히 바빠서 이와 같은 작업에 할애할 시간이 많지 않지만, 더 많은 내용을 추가할 때 블로그에 기록할 예정입니다! 그렇다고 해도, 이 프로젝트는 완전히 재미있을 것입니다 (아마도 미래의 개인 프로젝트 중 일부)! 그리고 이 프로젝트 전체를 프로그래밍하는 거대한 단계는 곧 하지 않을 것입니다.

## 설정 및 접근

<div class="content-ad"></div>

앞서 간단히 언급한대로, 이 프로젝트를 만드는 데 최상의 방법에 대해 얼마 동안 고민해왔습니다. 다행히도 이미 ToolipsSession에서 rpc 구문을 통해 서버와 클라이언트간에 명령을 전송하는 편리한 방법이 있습니다. 이것은 Vulta 쪽에서는 충돌 및 키프레이밍과 같은 기능들을 찾아내는 것만 생각해야 합니다! 이 방법은 모든 것을 쉽게 만들어주진 않겠지만, 처음부터 프로그래밍하는 것보다는 훨씬 쉬울 것입니다! 이 모듈과 해당 종속 항목을 살펴보겠습니다:

```js
module Vulta
using Toolips
using ToolipsSession
using ToolipsSVG

end # - module
```

지금 해야 할 일은 일관된 창을 만들어야 하고, 여기서 쉽게 변역할 수 있는 무엇인가를 만드는 데 집중해야 합니다. 또한 다른 웹 앱인 VultaTest 라는 것을 만들 계획입니다.

```js
julia> using Toolips; Toolips.new_webapp("VultaTest")
  프로젝트 VultaTest 생성 중:
    VultaTest/Project.toml
    VultaTest/src/VultaTest.jl
```

<div class="content-ad"></div>

지금 Vulta에서 개발을 시작할 거야:

```js
(VultaTest) pkg> develop ./Vulta
   패키지 버전 확인 중...
에러: 패키지 ToolipsSVG [8ae86ec9]의 요구사항 충족 불가능:
 ToolipsSVG [8ae86ec9] 로그:
 ├─ToolipsSVG [8ae86ec9]에는 알려진 버전이 없습니다!
 └─Vulta [40234ac0]에 의해 버전 *로 제한됨 — 남은 버전 없음
   └─Vulta [40234ac0] 로그:
     ├─가능한 버전: 0.1.0 또는 미설치됨
     └─Vulta [40234ac0]이 버전 0.1.0으로 고정됨

(VultaTest) pkg> add https://github.com/ChifiSource/ToolipsSVG.jl
    git-repo 업데이트 중 `https://github.com/ChifiSource/ToolipsSVG.jl`
   패키지 버전 확인 중...
    `~/dev/toolips/VultaTest/Project.toml` 업데이트 중
  [8ae86ec9] + ToolipsSVG v0.1.0 `https://github.com/ChifiSource/ToolipsSVG.jl#main` 추가
    `~/dev/toolips/VultaTest/Manifest.toml` 업데이트 중

(VultaTest) pkg> develop ./Vulta
   패키지 버전 확인 중...
    `~/dev/toolips/VultaTest/Project.toml` 업데이트 중
  [40234ac0] + Vulta v0.1.0 `../Vulta` 추가
    `~/dev/toolips/VultaTest/Manifest.toml` 업데이트 중
```

첫 번째로 할 일은 기본적인 상태 확인 데모를 만들어보는 것이야:

```js
function home(c::Connection)
    testbod = body("mainbody")
    mainwindow = svg("mainwindow", width = 500, height = 500)
    push!(mainwindow, circle("testcircle", cx = 250, cy = 250, "stroke-width" => 5px,
    r = 5))
    push!(testbod, mainwindow)
    write!(c, testbod)
end
```

<div class="content-ad"></div>

<img src="/assets/img/2024-07-13-ExperimentingWithSVGGameDevelopmentInJulia_1.png" />

그럼 우리가 해야 할 일은 줄리아에서 Vector2s와 Vector3s를 잘 표현하고 기본적인 충돌 수학을 해결해야 합니다. 고유한 함수와 기능을 만들어서 — 데모에서는 이를 RPC로 시도하여 멀티 플레이어 퐁게임을 만들겠습니다. 여기에 파일이 일반적으로 어떻게 되어야 하는지에 대한 기본적인 개요가 있습니다:

```js
abstract type VultaVector end

mutable struct Vec2 <: VultaVector
    x::Int64
    y::Int64
end

mutable struct Vec3 <: VultaVector
    x::Int64
    y::Int64
    z::Int64
end

mutable struct VultaModifier <: ToolipsSession.AbstractComponentModifier
    rootc::Dict{String, Servable}
    changes::Vector{String}
    function VultaModifier(cm::ComponentModifier)

    end
end

function translate!(cm::VultaModifier, comp::Component{<:Any}, v::Vec2)

end

function is_colliding(vm::VultaModifier, s::Component{<:Any}, s2::Component{<:Any})

end

function force!(vm::VultaModifier, s::Component{<:Any}, force::Vec2)

end

function spawn!(vm::VultaModifier, s::Component{<:Any})

end
```

이 부분은 물론 많이 변경될 것이지만, 이것은 나머지 내용을 추가하기 위한 훌륭한 방향을 제시합니다. 우리는 VultaModifier의 데이터를 확장하고, 메서드를 만들고, 간단한 함수 래퍼를 만들면 됩니다.

<div class="content-ad"></div>

## **vulta modifier와 spawn!**

프로젝트 진행을 위해 먼저 해야 할 일은 이 새로운 Modifier 유형을 확장하는 것입니다. 내부 VultaVector 유형에 대한 매개변수를 추가했습니다. 이것은 2D 대 3D 항목을 추적하는 데 도움이 될 것입니다. 이것은 존재하지는 않지만 만약 나중에 만들고 싶어진다면 몇 가지 빈 함수도 있습니다. 기본적으로 이를 둘러싼 RPC 래퍼를 작성할 계획이며, 이 작업은 정말 쉬울 것으로 생각됩니다. 먼저 새로운 Modifier를 살펴봅시다:

```js
mutable struct VultaModifier{V <: VultaVector} <: ToolipsSession.AbstractComponentModifier
    c::Connection
    delta::Int64
    rootc::Dict{String, Servable}
    comps::Dict{String, V}
    changes::Vector{String}
    function VultaModifier(c::Connection,
        delta::Int64, width::Int64, height::Int64, cm::ComponentModifier)
        comps::Dict{String, Vec2} = Dict{String, Vec2}()
        for comp in cm.rootc
            if comp[2].tag ==  "circle"
                push!(comps, comp[2].name => Vec2(parse(Int64, comp[2]["cx"]),
                                                parse(Int64, comp[2]["cy"])))
            end
        end
        new{Vec2}(c, delta, cm.rootc, comps, cm.changes)
    end
    function VultaModifier(delta::Int64, width::Int64, height::Int64, depth::Int64,
         cm::ComponentModifier)
         throw!("3D가 아직 구현되지 않았습니다")
    end
end
```

다음으로, 여러분의 open 및 다른 rpc 메서드가 있습니다:

<div class="content-ad"></div>

```js
function open(f::Function, c::Connection, bod::Component{:body},
    width::Int64 = 1280, height::Int64 = 720; tickrate::Int64 = 100)
    open_rpc!(c, "vulta-rpc", tickrate = tickrate)
    push!(c[:Session].peers,
     getip(c) => Dict{String, Vector{String}(getip(c) => Vector{String}()))
     delta::Int64 = 0
    script!(c, getip(c) * "rpc", time = tickrate) do cm::ComponentModifier
        delta += 1
        push!(cm.changes, join(c[:Session].peers[getip(c)][getip(c)]))
        vm = VultaModifier(c, delta, width, height, cm)
        f(vm)
        c[:Session].peers[getip(c)][getip(c)] = Vector{String}()
    end
    wind = svg("vulta-main", width = width, height = height)
    push!(bod, wind)
    style!(wind, "padding" => 0px, "position" => "absolute")
    write!(c, bod)
end

function rpc!(vm::VultaModifier)
    mods::String = find_client(vm)
    [push!(mod, join(cm.changes)) for mod in values(vm.c[:Session].peers[mods])]
    deleteat!(cm.changes, 1:length(cm.changes))
end

find_client(vm::VultaModifier) = findfirst(x -> getip(c) in keys(x),
                                            vm.c[:Session].peers)

function close(vm)

end
```

이제 테스트 앱 내부에서 모두 시도해 봅시다:

```js
function home(c::Connection)
    pongbod = body("mainbody")
    Vulta.open(c, pongbod) do vm::VultaModifier
        if ~("mycirc" in keys(vm.rootc))
            mycirc = circle("mycirc", "stroke-width" => 5px, "r" => 5)
            spawn!(vm, mycirc, Vec2(100, 100))
        else
            
        end
    end
end
```

여기서 우리는 공을 생성할 것이며, 이것이 최종적으로 퐁 공이 될 것입니다. 이를 로딩해보겠습니다:


<div class="content-ad"></div>


![이미지](/assets/img/2024-07-13-ExperimentingWithSVGGameDevelopmentInJulia_2.png)

지금 다른 생성물체를 추가할 거에요! 왼쪽 패들을 만들기 위해:

```js
function spawn!(vm::VultaModifier{Vec2}, s::Component{:rect}, v::Vec2 = Vec2(0, 0))
    s["x"] = v.x; s["y"] = v.y
    style!(s, "transition" => 1seconds)
    append!(vm, "vulta-main", s)
end
```

그리고 우리의 테스트 프로젝트에서 다시 호출할 거에요:


<div class="content-ad"></div>

```js
함수 home(c::Connection)
    pongbod = body("mainbody")
    Vulta.open(c, pongbod) do vm::VultaModifier
        if vm.delta == 1
            pongball = circle("mycirc", "stroke-width" => 5px, "r" => 10)
            spawn!(vm, pongball, Vec2(640, 360))
            pongpaddle_left = rect("paddle-left", width = 10,
            height = 50)
            spawn!(vm, pongpaddle_left, Vec2(320, 180))
        else

        end
    end
end
```
## 몇 가지 더 메소드

이제 빠르게 이러한 두 번역 함수를 만들어 보겠습니다. 그런 다음 충돌에 대해 작업할 것입니다!

```js
함수 translate!(cm::VultaModifier{Vec2}, comp::Component{:circle}, v::Vec2)
    cm[comp] = "cx" => v.x; cm[comp] = "cy" => v.y
end

함수 translate!(cm::VultaModifier{Vec2}, comp::Component{:rect}, v::Vec2)
    cm[comp] = "x" => v.x; cm[comp] = "y" => v.y
end
```

<div class="content-ad"></div>

쉬운 작업이네요! 이제 강제! 함수로 넘어가 보도록 하죠. 이 작업을 수행하려면 스폰에 몇 가지 새로운 요소를 추가해야 할 것 같아요.

```js
function spawn!(vm::VultaModifier{Vec2}, s::Component{:rect}, v::Vec2 = Vec2(0, 0))
    s["x"] = v.x; s["y"] = v.y; s["fx"] = 0; s["fy"] = 0
    style!(s, "transition" => 1seconds)
    append!(vm, "vulta-main", s)
end

function spawn!(vm::VultaModifier{Vec2}, s::Component{:circle}, v::Vec2 = Vec2(0, 0))
    s["cx"] = v.x; s["cy"] = v.y; s["fx"] = 0; s["fy"] = 0
    style!(s, "transition" => 1seconds)
    append!(vm, "vulta-main", s)
end
```

이러한 새로운 속성인 fx와 fy는 힘을 나타내게 될 거예요. 이제 계산을 처리하는 새로운 force 함수를 만들면 ...

```js
function force(cm::ComponentModifier, comp::Component{:circle})
    if ~("fx" in keys(comp.properties))
        return
    end
    decay = parse(Int64, comp["decay"])
    currx = parse(Int64, comp["cx"])
    curry = parse(Int64, comp["cy"])
    fx = parse(Int64, comp["fx"])
    fy = parse(Int64, comp["fy"])
    if fx != 0
        cm[comp] = "cx" => currx + fx
        if fx > 0
            cm[comp] = "fx" => (fx - decay)
        else
            cm[comp] = "fx" => fx + (decay)
        end
    elseif fy != 0
        cm[comp] = "cy" => curry + fy
        if fy > 0
            cm[comp] = "fy" => (fy - decay)
        else
            cm[comp] = "fy" => fy + (decay)
        end
    end
end

function force(cm::ComponentModifier, comp::Component{<:Any})
    if ~("fx" in keys(comp.properties))
        return
    end
end

function force(cm::ComponentModifier, comp::Component{:rect})
    if ~("fx" in keys(comp.properties))
        return
    end
    decay = parse(Int64, comp["decay"])
    currx = parse(Int64, comp["x"])
    curry = parse(Int64, comp["y"])
    fx = parse(Int64, comp["fx"])
    fy = parse(Int64, comp["fy"])
    if fx != 0
        cm[comp] = "x" => currx + fx
        if fx > 0
            cm[comp] = "fx" => (fx - decay)
        else
            cm[comp] = "fx" => fx + (decay)
        end
    elseif fy != 0
        cm[comp] = "y" => curry + fy
        if fy > 0
            cm[comp] = "fy" => (fy - decay)
        else
            cm[comp] = "fy" => fy + (decay)
        end
    end
end
```

<div class="content-ad"></div>

## 충돌

이제 힘과 모든 것을 다 다한 상태이므로, 이제는 충돌에 집중할 차례입니다. 이 두 구성 요소에 대한 길이 및 너비 접근자가 필요할 것입니다.

```js
length(c::Component{:circle}) = c["r"]

length(c::Component{:rect}) = c["length"]

width(c::Component{:circle}) = c["r"]

width(c::Component{:rect}) = c["width"]
```

이렇게 하면 이러한 정보에 액세스하는 것이 쉬워집니다. 이제 두 가지 사이의 거리가 5픽셀 이내인지 확인하는 조건문을 만들겠습니다.

<div class="content-ad"></div>


```js
function is_colliding(vm::VultaModifier{Vec2}, s1::Component{<:Any},
    s2::Component{<:Any})
    s1 = vm.rootc[s1.name]
    s2 = vm.rootc[s2.name]
    s1pos = position(s1); s2pos = position(s2)
    diffx, diffy = abs(s1pos.x - s2pos.x), abs(s1pos.y - s2pos.y)
    if diffy < 5 && diffx < 5
        true
    else
        false
    end
end
```

이제 테스트를 설정하겠습니다. 패들을 공의 왼쪽에 두고 오른쪽 방향으로 강제로 이동하여 충돌이 감지되면 경고가 나타납니다:

```js
function home(c::Connection)
    pongbod = body("mainbody")
    on(c, pongbod, "click") do cm::ComponentModifier
        cm["paddle-left"] = "x" => 400
    end
    Vulta.open(c, pongbod) do vm::VultaModifier
        pongpaddle_left = rect("paddle-left", width = 10,
        height = 50)
        pongball = circle("mycirc", "stroke-width" => 5px, "r" => 10)
        if vm.delta == 1
            spawn!(vm, pongball, Vec2(640, 360))

            spawn!(vm, pongpaddle_left, Vec2(620, 360))
        elseif vm.delta == 55
            force!(vm, pongpaddle_left, Vec2(10, 0))
        else
            if is_colliding(vm, pongpaddle_left, pongball)
                alert!(vm, "colliding!")
            end
        end

    end
end
```

<img src="/assets/img/2024-07-13-ExperimentingWithSVGGameDevelopmentInJulia_3.png" /


<div class="content-ad"></div>

## 결론

내일 다음 기사에서 이 퐁 게임을 마무리할 예정이에요. 정말 재미있을 거에요! 이 기사에서 정말 어려운 부분들을 대부분 다루었지만, 물론 할 일은 항상 더 있을 거에요! Vulta가 제 주요 프로젝트는 아니지만, 여전히 정말 멋진 부가 임무가 되고 있어요! 제가 쓰는 만큼 여러분도 즐기셨으면 좋겠어요! 즐거운 하루 되세요!