---
title: "Angular 시그널로 object inputs이 여전히 별로인 이유"
description: ""
coverImage: "/assets/img/2024-08-26-ObjectinputsarestilldangerousevenwithAngularsignals_0.png"
date: 2024-08-26 18:32
ogImage: 
  url: /assets/img/2024-08-26-ObjectinputsarestilldangerousevenwithAngularsignals_0.png
tag: Tech
originalTitle: "Object inputs are still dangerous, even with Angular signals"
link: "https://medium.com/itnext/object-inputs-are-still-dangerous-even-with-angular-signals-9103a25d5e45"
isUpdated: true
updatedAt: 1724742709927
---


과거(2023년)에 @Input을 통해 객체를 전달할 때 컴포넌트 내부에서 조심해야 했던 것이 기억나시나요? 자식 컴포넌트 내에서 @Input을 변형하는 것은 좋지 않은 습관입니다. 왜냐하면 부모와 자식 간의 예상된 통신 채널 외부에서 발생하기 때문에 디버깅이 더 어려워집니다. 틀린 방향으로 가면(그리고 그럴 확률이 높습니다) 문제가 복잡해집니다.

안타깝게도 추가적인 주의를 기울이지 않고도 이러한 습관에 빠지기 쉬웠습니다. 하지만 요즘에는 입력 신호가 읽기 전용이기 때문에 입력 객체를 수정할 수 없게 되었습니다. 그러니 문제가 해결됐나요? 대부분은 그렇지만 조금의 부주의함으로 여전히 문제가 생길 수 있고 그 영향이 더 커질 수도 있습니다. 제가 설명해드릴게요.

![이미지](/assets/img/2024-08-26-ObjectinputsarestilldangerousevenwithAngularsignals_0.png)

# "옛 방식":

<div class="content-ad"></div>

```js
@Component({
  ...
  template: `
    <h1>입력 버전</h1>
    <app-old-input-way [objectInput]="parentObject"></app-old-input-way>

    <hr class="solid">

    <p> 부모 오브젝트 </p>
    { parentObject | json }
  `,
})
export class App {
  parentObject: ObjectInput = {
    name: 'parent',
    value: 0,
    props: { id: 'id1' },
  };
}

@Component({
  ...
  template: `
      { extendedObject | json }
      <button (click)='onSomeAction()'>액션!</button>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class OldInputWayComponent {
  private _objectInput: ObjectInput | undefined;
  @Input()
  set objectInput(objectInput: ObjectInput) {
    this._objectInput = objectInput;
    this.extendedObject = { ...objectInput };
    this.extendedObject.props.enabled = true;
  }

  get objectInput(): ObjectInput | undefined {
    return this._objectInput;
  }

  extendedObject: ExtendedObjectInput | undefined;

  onSomeAction() {
    this._objectInput!.value++;
  }
}
```

간단한 설정으로, 부모 구성 요소가 있는데 그 구성 요소에는 자식 구성 요소에 대한 입력으로 사용되는 객체가 있습니다. 자식 구성 요소 자체는 그 동일한 객체의 장식된 버전이 필요하지만 내부 작업에만 사용됩니다 (어떤 이유에서든, 가장 흔한 이유는 레거시일 때 😆). 여기서 유일한 차이점은 부모의 props에 있는 enabled입니다. 이 모든 것이 좋은 설계는 아니지만, 작은 부주의로 상황이 얼마나 나빠질 수 있는지 보여주는 것이 목적입니다. enabled는 부모에 나타나며, 값 변경은 부모에만 영향을 줍니다.

여기서 올바른 방법 중 하나는 (동일한 객체의 확장 버전이 실제로 필요한 경우) 프라이빗 변수에 할당할 때 입력 객체를 깊은 복사하는 것입니다. 프라이빗 변수가 변경될 때 부모에게 변경 사항을 알리기 위해 @Output을 호출하는 것도 중요합니다. 등가성 확인을 사용하여 순환 설정을 피하는 것을 잊지 마세요.


<div class="content-ad"></div>

```js
export class OldInputWayComponent {
  private _objectInput: ObjectInput | undefined;
  @Input()
  set objectInput(objectInput: ObjectInput) {
    if(isEqual(objectInput, this._objectInput) {
      return;
    }
    this._objectInput = cloneDeep(objectInput);
    this.extendedObject = { ...this._objectInput };
    this.extendedObject.props.enabled = true;
  }

  get objectInput(): ObjectInput | undefined {
    return this._objectInput;
  }

  @Output() objectChange = new EventEmitter<ObjectInput>()

  extendedObject: ExtendedObjectInput | undefined;

  onSomeAction() {
    this.extendedObject.value++;
    const newObj = { ...this.extendedObject, props: { id: this.extendedObject.id } };
    this.objectChange(newObj);
  }
}
```

이것은 매우 정제되지 않은 구현 방법이며, 사고의 철학의 청사진일 뿐이며 컴파일되지 않습니다. 그대로 복사하지 마세요. 좋은 설계를 복사하고 싶다면 모델과 뷰 모델이 분리된 ngModel의 구현을 확인해보세요.

# 신호로:

신호 입력은 읽기 전용이며, 어떻게 그것을 엉망으로 만들 수 있을까요? 내 라떼 아보카도를 가져다주세요:


<div class="content-ad"></div>

```js
export class SignalInputWayComponent {
  objectInput = input.required<ObjectInput>(); // <-- 신호 입력

  extendedObject = computed<ExtendedObjectInput>(() => {
    const base: ExtendedObjectInput = this.objectInput();
    base.props.enabled = this.enabled();
    return base;
  });

  private enabled = signal<boolean>(true); // <-- 확장 부분

  onSomeAction() {
    const obj = this.objectInput();
    obj.value++;
  }
}
```

첫눈에 보면 어느 정도 꼬일 수 있을 것 같지만, 다행히 그리 많은 노력이 필요하지는 않다는 것이 보입니다. 우리가 원래 한 것은 원본에서 확장된 객체를 생성하고 그것을 자식 컴포넌트에서 어떻게든 사용하는 것이었습니다. 속성 값을 가져오거나 설정하며 작업하는 것이 여기에서는 작동하지 않습니다. 확장 된 객체를 업데이트하기 위해 계산된 신호가 필요하며, 계산된 신호는 직접 업데이트 할 수 없기 때문에 확장 부분에 대한 신호가 필요합니다. 보통 확장 신호를 사용하여 확장된 객체를 설정하고 필요한 사람에게 전달합니다.

문제점: base를 통해, extendedObject는 다시 부모 객체에 대한 참조를 보유합니다. 그리고 마지막으로 onSomeAction, 이것이 무엇을 하는지 살펴봅시다:


<div class="content-ad"></div>

부모 객체에 확장을 추가했으며 두 신호의 값도 수정했습니다(다시 말하지만, 읽기 전용입니다).

아무 규칙을 지키지 않고, 2개의 잘못된 해결책을 결합해봅시다(그리고 무엇이 잘못될 수 있는지 빠르게 확인해봅시다).

![image](https://miro.medium.com/v2/resize:fit:1104/1*9_ADYT6VVXGKCQbbRcPHiA.gif)

```javascript
export class SignalInputWayComponentFixed {
  objectInput = input.required<ObjectInput>();
  change = output<ObjectInput>();

  extendedObject = computed<ExtendedObjectInput>(() => {
    const base: ExtendedObjectInput = _.cloneDeep(this.objectInput()); // 이것은 비용이 많이 들 수 있으므로 모든 사용 사례에 권장하지 않습니다
    return {
      ...base, // 기본 obj > 확장 obj, 확장 부분을 제외하고
      props: _.merge(this._extendedObject().props, base.props), 
    };
  });

  private _extendedObject = signal<ExtendedObjectInput>({
    name: '',
    value: 0,
    props: { id: '', enabled: false },
  });

  onSomeAction() {
    this._extendedObject.update((obj) => {
      obj.props.enabled = !obj.props.enabled;
      obj.value++;
      return obj;
    });
    const newExtendedObj = this._extendedObject();
    this.change.emit({
      ...this.objectInput(),
      value: newExtendedObj.value,
    });
  }
}
```

<div class="content-ad"></div>

# 요약

가능하다면 이러한 종류의 객체 확장을 피하십시오. 불가피한 경우 내부 상태와 외부 상태를 최대한 분리하고 항상 비확장 부분에 대한 입력으로 제공되는 외부 상태에 우선권을 부여하십시오.

어떻게 아름답게 작동하는지 보세요 🙂

![이미지](/assets/img/2024-08-26-ObjectinputsarestilldangerousevenwithAngularsignals_3.png)