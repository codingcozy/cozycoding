---
title: "SwiftUI와 MapKit을 이용한 실시간 지도 앱 만들기"
description: ""
coverImage: "/assets/img/2024-08-26-SwiftUIMapKitLetsLookAround_0.png"
date: 2024-08-26 19:34
ogImage: 
  url: /assets/img/2024-08-26-SwiftUIMapKitLetsLookAround_0.png
tag: Tech
originalTitle: "SwiftUI  MapKit Lets Look Around"
link: "https://medium.com/gitconnected/swiftui-mapkit-lets-look-around-69c4a20ad3e1"
isUpdated: true
updatedAt: 1724742614108
---


![LookAroundPreview](/assets/img/2024-08-26-SwiftUIMapKitLetsLookAround_0.png)

LookAroundPreview은 MapKit의 내가 가장 좋아하는 기능 중 하나입니다. 이 기능은 선택한 위치의 거리 이미지를 360도로 제공합니다.

![Street View](https://miro.medium.com/v2/resize:fit:590/1*q0NjU2OEYf0vt6bcJ9wyGw.gif)

이 글에서는 우리의 지도 앱에 이 작은 Look Around 기능을 추가하는 방법을 확인해보겠습니다.

<div class="content-ad"></div>

# 간단한 소개

LookAroundPreview를 만들고 앱에 Look Around 기능을 추가하는 두 가지 방법이 있습니다.

## Way 1!

단순히 LookAroundPreview 구조체를 사용하세요.

<div class="content-ad"></div>

우리는 이렇게 초기화할 수 있어요.

- init(initialScene: MKLookAroundScene?, allowsNavigation: Bool, showsRoadLabels: Bool, pointsOfInterest: PointOfInterestCategories, badgePosition: MKLookAroundBadgePosition)을 사용하여 초기 MKLookAroundScene을 제공하거나,
- init(scene: Binding`MKLookAroundScene?`, allowsNavigation: Bool, showsRoadLabels: Bool, pointsOfInterest: PointOfInterestCategories, badgePosition: MKLookAroundBadgePosition)에 scene을 바인딩하여 제공할 수 있어요.

## 두 번째 방법!

우리는 lookAroundViewer 보기 수정자를 사용하여 직접 Look Around 뷰어를 만들고 이를 지도에 연결할 수도 있어요.

<div class="content-ad"></div>

Look Around 뷰어와 Look Around 미리보기가 어떻게 다른가요?

미리보기는 기본적으로 해당 장소의 이미지일 뿐이에요. 미리보기를 탭하면 전체 화면 Look Around 뷰어가 열리는데, 여기서 실제로 주변을 둘러볼 수 있고, 네비게이션이 허용된다면 움직일 수도 있어요.

저는 개인적으로 즉시 전체 화면 뷰어를 열지 않는 것을 선호하고, 사이에 미리보기가 있는 것을 감사하게 생각해요. 그래서 LookAroundPreview 구조체 접근 방식을 사용해 봅시다!

# 함께 둘러봐요!

<div class="content-ad"></div>

어서 오세요!

룩어라운드 프리뷰를 만드는 방법을 알려드리겠습니다.

- 관심 있는 CLLocationCoordinate2D를 가져옵니다.
- 좌표를 사용하여 MKLookAroundSceneRequest를 생성합니다.
- 요청에서 MKLookAroundScene을 가져옵니다.
- 씬을 사용하여 LookAroundPreview를 생성하고 필요에 따라 allowsNavigation, showsRoadLabels, pointsOfInterest 및 badgePosition을 설정할 수 있습니다.

## 참고

<div class="content-ad"></div>

MKLookAroundScene은 모든 위치에서 사용할 수 있는 것이 아닙니다! 만일 둘러보기 기능이 덮지 않은 위치에 CLLocationCoordinate2D로 요청을 보내면, MKLookAroundScene은 nil이 될 것입니다.

## 예시

코드를 작성해봅시다!

사용자가 지도 피처를 탭했을 때 LookAroundPreview를 보여주는 간단한 예시를 만들어 보겠습니다.

<div class="content-ad"></div>

```swift
import SwiftUI
import MapKit

private struct LookAroundView: View {
    var selection: MapFeature
    @State private var lookAroundScene: MKLookAroundScene?
    
    var body: some View {
        LookAroundPreview(initialScene: lookAroundScene)
            .overlay(alignment: .bottomLeading, content: {
                if let title = selection.title {
                    HStack {
                        Text(title)
                    }
                    .font(.caption)
                    .foregroundStyle(.white)
                    .padding(.all, 8)
                    .background(RoundedRectangle(cornerRadius: 4).fill(.black))
                    .padding(.all, 8)
                }
            })
            .onChange(of: selection, initial: true, {
                getLookAroundScene()
            })
    }
    
    func getLookAroundScene() {
        lookAroundScene = nil
        Task {
            let request = MKLookAroundSceneRequest(coordinate: selection.coordinate)
            do {
                lookAroundScene = try await request.scene
                if lookAroundScene == nil {
                    print("Look Around Preview not available for the given coordinate.")
                }
            } catch (let error) {
                print(error)
            }
        }
    }
}

struct LookAroundBaseView: View {
    
    // Tokyo, Shibuya
    private let initialPosition = MapCameraPosition.camera(
        MapCamera(
            centerCoordinate: CLLocationCoordinate2D(latitude: 35.6615, longitude: 139.703),
            distance: 300,
            heading: 0,
            pitch: 0
        )
    )
    
    @State private var selection: MapFeature? = nil
    @State private var lookAroundScene: MKLookAroundScene?

    
    var body: some View {
        Map(initialPosition: initialPosition, selection: $selection)
            .overlay(alignment: .top, content: {
                if let selection = selection {
                    LookAroundView(selection: selection)
                        .frame(height: 256)
                        .clipShape(RoundedRectangle(cornerRadius: 10))
                        .padding()
                }
            })
            
    }
}
```

그리고 이렇게 하면 됩니다!

<img src="https://miro.medium.com/v2/resize:fit:590/1*q0NjU2OEYf0vt6bcJ9wyGw.gif" />

원래는 콜로라도 주 아스펜을 중심으로 예제를 만들려고 했는데, 거기 주변에 엄청나게 MKLookAroundScene을 제공하는 MapFeatures가 없다는 것을 알게 되었어요…


<div class="content-ad"></div>

어쨌든!

읽어 주셔서 감사합니다!

오늘은 여기까지!

즐거운 살펴보기 되세요!