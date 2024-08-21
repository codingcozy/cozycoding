---
title: "처음부터 시작하는 플러터 아키텍처 구축 두 번째 이야기"
description: ""
coverImage: "/assets/img/2024-08-21-HowtoCreateaFlutterArchitecturefromScratchTheSecondPart_0.png"
date: 2024-08-21 18:35
ogImage: 
  url: /assets/img/2024-08-21-HowtoCreateaFlutterArchitecturefromScratchTheSecondPart_0.png
tag: Tech
originalTitle: "How to Create a Flutter Architecture from Scratch The Second Part"
link: "https://medium.com/@denis-lomov/how-to-create-a-flutter-architecture-from-scratch-the-second-part-bcefa967046e"
isUpdated: false
---


![이미지](/assets/img/2024-08-21-HowtoCreateaFlutterArchitecturefromScratchTheSecondPart_0.png)

안녕하세요! Denis Lomov입니다. 저는 플러터 시리즈의 두 번째이자 마지막 부분을 공유할 준비가 되어 있습니다. 첫 번째 부분은 여기에서 확인할 수 있습니다.

이 기사에서는 제 동료인 모바일 개발자 Eugene Efanov이 의존성 주입 구현, 아키텍처를 계층으로 구조화하는 방법 및 생성한 로직을 테스트하는 방법을 안내합니다.

# 의존성 주입

<div class="content-ad"></div>

의존성 주입은 테스트 중에 의존성을 교체할 수 있도록 해주기 때문에 아키텍처에서 중요합니다.

이를 구현하기 위해서 우리는 클래스들을 MvvmInstance에 연결하는 인터페이스를 생성하고 객체 생성을 위한 코드를 생성해야 합니다. 우리의 구현에서는 기본 생성자를 사용하기 때문에 이 코드를 생성하는 것은 간단합니다.

인스턴스를 저장하기 위해서는 준비된 인스턴스의 사전을 포함하는 싱글톤이 필요합니다.

이런 클래스의 간소화된 구현은 다음과 같을 것입니다:

<div class="content-ad"></div>

```js
mixin SavableStatefulMvvmInstance<State, Input> on StatefulMvvmInstance<State, Input> {
  StreamSubscription<State>? _storeSaveSubscription;

  Map<String, dynamic> get savedStateObject => {};

  @protected
  void restoreCachedStateSync() {
    if (!stateFullInstanceSettings.isRestores) {
      return;
    }

    final stateFromCacheJsonString = UMvvmApp.cacheGetDelegate(
      stateFullInstanceSettings.stateId,
    );

    if (stateFromCacheJsonString == null || stateFromCacheJsonString.isEmpty) {
      return;
    }

    final restoredMap = json.decode(stateFromCacheJsonString);
    onRestore(restoredMap);
  }

  void onRestore(Map<String, dynamic> savedStateObject) {}

  @override
  void initializeStore() {
    _subscribeToStoreUpdates();
  }

  void initializeStatefullInstance() {
    initializeStore();
    restoreCachedStateSync();
  }

  void _subscribeToStoreUpdates() {
    if (!stateFullInstanceSettings.isRestores) {
      return;
    }

    _storeSaveSubscription = _store.stream.listen((_) async {
      final stateId = state.runtimeType.toString();
      await UMvvmApp.cachePutDelegate(stateId, json.encode(savedStateObject));
    });
  }

  @override
  void disposeStore() {
    _storeSaveSubscription?.cancel();
  }

  StateFullInstanceSettings get stateFullInstanceSettings => StateFullInstanceSettings(
        stateId: state.runtimeType.toString(),
      );
}
```

여기에서는 생성할 빌더들의 사전과 이미 생성된 객체의 컨테이너를 저장합니다.

또한 사전을 스코프로 나눠서 인스턴스를 분리합니다. 기본 스코프로는 싱글톤이 저장되어 앱이 최종 초기화될 때 초기화됩니다. 고유 스코프를 추가하여 항상 새로운 인스턴스가 생성됩니다. 추가로 약한 스코프도 추가할 수 있습니다. 전역 스코프와 비슷하지만 메모리 누수를 방지하기 위해 모든 종속 인스턴스가 파괴될 때 모든 객체가 파괴됩니다. 다른 사용자 정의 스코프는 약한 스코프와 비슷하게 작동합니다. 모든 종속 인스턴스가 파괴될 때 스코프 내의 모든 객체도 파괴됩니다.

```js
class InstanceCollection {
  final container = ScopedContainer<MvvmInstance>();
  final builders = HashMap<String, Function>();

  static final InstanceCollection _singletonInstanceCollection = InstanceCollection._internal();

  static InstanceCollection get instance {
    return _singletonInstanceCollection;
  }

  InstanceCollection._internal();

  void addBuilder<Instance extends MvvmInstance>(Function builder) {
    final id = Instance.toString();

    builders[id] = builder;
  }

  Instance get<Instance extends MvvmInstance>({
    DefaultInputType? params,
    int? index,
    String scope = BaseScopes.global,
  }) {
    return getWithParams<Instance, DefaultInputType?>(
      params: params,
      index: index,
      scope: scope,
    );
  }

  Instance getWithParams<Instance extends MvvmInstance, InputState>({
    InputState? params,
    int? index,
    String scope = BaseScopes.global,
  }) {
    final runtimeType = Instance.toString();

    return getInstanceFromCache<Instance>(
      runtimeType,
      params: params,
      index: index,
      scopeId: scope,
    );
  }

  void addWithParams<InputState>({
    required String type,
    InputState? params,
    int? index,
    String? scope,
  }) {
    final id = type;
    final scopeId = scope ?? BaseScopes.global;

    if (container.contains(scopeId, id, index) && index == null) {
      return;
    }

    final builder = builders[id];

    final newInstance = builder!() as MvvmInstance;

    container.addObjectInScope(
      object: newInstance,
      type: type,
      scopeId: scopeId,
    );

    if (!newInstance.isInitialized) {
      newInstance.initialize(params);
    }
  }

  Instance constructAndInitializeInstance<Instance extends MvvmInstance>(
    String id, {
    dynamic params,
    bool withNoConnections = false,
  }) {
    final builder = builders[id];

    final instance = builder!() as Instance;

    instance.initialize(params);

    return instance;
  }

  Instance getInstanceFromCache<Instance extends MvvmInstance>(
    String id, {
    dynamic params,
    int? index,
    String scopeId = BaseScopes.global,
    bool withoutConnections = false,
  }) {
    final scope = scopeId;

    final instance = container.getObjectInScope(
      type: id,
      scopeId: scope,
      index: index ?? 0,
    ) as Instance;

    if (!instance.isInitialized) {
      instance.initialize(params);
    }

    return instance;
  }
}
```

<div class="content-ad"></div>

해당 인스턴스가 우리의 사전에 아직 생성되지 않았다면, 해당 인스턴스를 구축하고 초기화를 호출한 후 초기화된 인스턴스를 반환합니다. 이미 존재한다면, 기존 객체를 그대로 반환합니다.

source_gen을 사용하여 빌더의 사전을 생성할 수 있습니다.

먼저 우리의 인스턴스를 표시하는 주석을 만듭니다. 그런 다음, 생성기에서 이들을 검색하고 해당하는 빌더를 생성합니다. 우리는 두 가지 주석으로 시작할 수 있습니다: 하나는 일반 객체에 대한 것이고 다른 하나는 싱글톤을 위한 것입니다. 싱글톤은 자동으로 전역 범위에 배치됩니다.

```js
class TestInstance1 extends MvvmInstance {}

class TestInstance2 extends MvvmInstance {}

void testInstanceCollection() {
  // 싱글톤 인스턴스
  final singletonInstance = InstanceCollection.instance.get<TestInstance1>(scope: BaseScopes.global);

  // 약한 인스턴스
  final weakInstance = InstanceCollection.instance.get<TestInstance2>(scope: BaseScopes.weak);
  final weakInstance2 = InstanceCollection.instance.get<TestInstance2>(scope: BaseScopes.weak); // 같은 인스턴스

  // 고유한 인스턴스
  final uniqueInstance = InstanceCollection.instance.get<TestInstance2>(scope: BaseScopes.unique);
  final uniqueInstance2 = InstanceCollection.instance.get<TestInstance2>(scope: BaseScopes.unique); // 새 인스턴스
}
```

<div class="content-ad"></div>

사전을 통해 객체의 목록을 얻은 후, 이를 MvvmInstance에 연결할 수 있습니다.

이를 위해 의존성을 인스턴스 구성에 추가할 것입니다. 구성에는 "커넥터" 목록이 포함되어 있으며, 각각 입력 데이터와 연결된 엔티티가 검색되어야 하는 범위와 같은 매개변수가 포함됩니다.

```js
class Instance {
  final Type inputType;
  final bool singleton;
  final bool isAsync;

  const Instance({
    this.inputType = Map<String, dynamic>,
    this.singleton = false,
    this.isAsync = false,
  });
}

const basicInstance = Instance();
const singleton = Instance(singleton: true);

class MainAppGenerator extends GeneratorForAnnotation<MainApp> {
  @override
  FutureOr<String> generateForAnnotatedElement(
    sg.Element element,
    ConstantReader annotation,
    BuildStep buildStep,
  ) async {
    const className = 'AppGen';
    final classBuffer = StringBuffer();

    final instanceJsons = Glob('lib/**.mvvm.json');

    final jsonData = <Map>[];

    await for (final id in buildStep.findAssets(instanceJsons)) {
      final json = jsonDecode(await buildStep.readAsString(id));
      jsonData.addAll([...json]);
    }

    // ...

    classBuffer
      ..writeln('@override')
      ..writeln('void registerInstances() {');

    classBuffer.writeln('instances');

    for (final element in instances) {
      classBuffer.writeln('..addBuilder<${element.name}>(() => ${element.name}())');
    }

    classBuffer.writeln(';');

    // ...

    return classBuffer.toString();
  }
}
```

이제 MvvmInstance에 대한 의존성 목록이 준비되었으므로, 초기화 중에 인스턴스 사전에서 이를 검색할 수 있습니다.

<div class="content-ad"></div>


class Connector {
  final Type type;
  final dynamic input;
  final String scope;
  final bool isAsync;

  const Connector({
    required this.type,
    this.input,
    this.scope = BaseScopes.weak,
    this.isAsync = false,
  });
}

class DependentMvvmInstanceConfiguration extends MvvmInstanceConfiguration {
  const DependentMvvmInstanceConfiguration({
    super.isAsync,
    this.dependencies = const [],
  });

  final List<Connector> dependencies;
}


# 아키텍처 컴포넌트

<img src="/assets/img/2024-08-21-HowtoCreateaFlutterArchitecturefromScratchTheSecondPart_1.png" />

이제 이벤트, 상태 및 종속성을 처리하는 클래스가 있으므로 아키텍처를 계층 구조로 구성할 수 있습니다.


<div class="content-ad"></div>


![이미지](/assets/img/2024-08-21-HowtoCreateaFlutterArchitecturefromScratchTheSecondPart_2.png)

각 MvvmInstance는 이벤트에 연결되어 있습니다.

도메인 레이어에서는 인터랙터에 집중하며, 상태를 보유하는 엔티티에 중점을 둡니다. 이 엔티티는 포스트 목록을 관리하는 것과 같은 논리적 구성 요소를 격리하는 데 유용합니다. 여기서는 서버에서 포스트를 검색하고 특정 포스트의 좋아요 이벤트에 구독하여 객체의 상태에서 컬렉션을 업데이트할 수 있습니다.

```js
abstract class BaseInteractor<State, Input> extends MvvmInstance<Input?>
    with StatefulMvvmInstance<State, Input?>, DependentMvvmInstance<Input?> {
  @mustCallSuper
  @override
  void initialize(Input? input) {
    super.initialize(input);

    initializeDependencies();
    initializeStatefullInstance();
  }

  @mustCallSuper
  @override
  void dispose() {
    super.dispose();

    disposeStore();
    disposeDependencies();
  }

  @mustCallSuper
  @override
  Future<void> initializeAsync() async {
    await super.initializeAsync();
  }
}
```

<div class="content-ad"></div>

런타임 오류는 프로그램이 실행되는 동안 발생하는 오류입니다. 이러한 오류는 프로그램이 실행 중에 발생하는 예외로 인해 발생하며 일반적으로 심각한 오류인 경우 프로그램이 비정상적으로 종료됩니다.

런타임 오류는 이후에 코드를 수정하고 버그를 해결할 때 유용한 정보를 제공할 수 있습니다. 런타임 오류가 발생하면 프로그램이 어떤 부분에서 문제가 발생했는지 추적할 수 있습니다. 이를 통해 버그를 신속하게 식별하고 수정할 수 있습니다.

<div class="content-ad"></div>

프레젠테이션 수준에서 뷰 모델을 소개하고 있습니다. 기본적으로 '인터랙터'는 뷰에 연결되는 입력 데이터를 가지고 있습니다. 여기서 필요한 데이터와 표시를 위한 바인딩을 설정할 수 있습니다. 최종 버전에서 포스트를 로드하는 구조는 다음과 같이 구성됩니다:

```js
part 'main.mvvm.dart';
part 'main.mapper.dart';

class PostLikedEvent {
  final int id;

  const PostLikedEvent({
    required this.id,
  });
}

@MappableClass()
class Post with PostMappable {
  const Post({
    required this.title,
    required this.body,
    required this.id,
    this.isLiked = false,
  });

  final String? title;
  final String? body;
  final int? id;
  final bool isLiked;

  static const fromMap = PostMapper.fromMap;
}

@MappableClass()
class PostsState with PostsStateMappable {
  const PostsState({
    this.posts,
    this.active,
  });

  final StatefulData<List<Post>>? posts;
  final bool? active;
}

@mainApi
class Apis with ApisGen {}

@mainApp
class App extends UMvvmApp with AppGen {
  final apis = Apis();

  @override
  Future<void> initialize() async {
    await super.initialize();
  }
}

final app = App();

// ...

@basicInstance
class PostsInteractor extends BaseInteractor<PostsState, Map<String, dynamic>?> {
  Future<void> loadPosts(int offset, int limit, {bool refresh = false}) async {
    updateState(state.copyWith(posts: const LoadingData()));

    late Response<List<Post>> response;

    if (refresh) {
      response = await executeAndCancelOnDispose(
        app.apis.posts.getPosts(0, limit),
      );
    } else {
      response = await executeAndCancelOnDispose(
        app.apis.posts.getPosts(offset, limit),
      );
    }

    if (response.isSuccessful) {
      updateState(
        state.copyWith(posts: SuccessData(result: response.result ?? [])),
      );
    } else {
      updateState(state.copyWith(posts: ErrorData(error: response.error)));
    }
  }

  @override
  List<EventBusSubscriber> subscribe() => [
        on<PostLikedEvent>(
          (event) {
            // update state
          },
        ),
      ];

  @override
  PostsState get initialState => const PostsState();
}

class PostsListViewState {}

class PostsListViewModel extends BaseViewModel<PostsListView, PostsListViewState> {
  @override
  DependentMvvmInstanceConfiguration get configuration => DependentMvvmInstanceConfiguration(
        dependencies: [
          app.connectors.postsInteractorConnector(),
        ],
      );

  late final postsInteractor = getLocalInstance<PostsInteractor>();

  @override
  void onLaunch() {
    postsInteractor.loadPosts(0, 30, refresh: true);
  }

  void like(int id) {
    app.eventBus.send(PostLikedEvent(id: id));
  }

  Stream<StatefulData<List<Post>>?> get postsStream => postsInteractor.updates((state) => state.posts);

  @override
  PostsListViewState get initialState => PostsListViewState();
}

class PostsListView extends BaseWidget {
  const PostsListView({
    super.key,
    super.viewModel,
  });

  @override
  State<StatefulWidget> createState() {
    return _PostsListViewWidgetState();
  }
}

class _PostsListViewWidgetState extends BaseView<PostsListView, PostsListViewState, PostsListViewModel> {
  @override
  Widget buildView(BuildContext context) {
    // ...
  }
} 
```

<div class="content-ad"></div>

# 테스트

만들어 둔 논리를 테스트하기 위해 DI 컨테이너의 요소를 미리 생성된 요소로 교체하고 상태에 테스트 데이터를 전달할 수 있습니다.

이 라이브러리에는 우리가 만든 엔티티들에서 이벤트 디스패치를 확인하고 순환 종속성을 감지하는 메서드도 포함되어 있습니다. 구현 세부 정보는 여기에 제공하지 않겠지만 코드 저장소에서 확인할 수 있습니다.

아래는 각 엔티티를 테스트하는 방법의 예시입니다.

<div class="content-ad"></div>

```js
class MockPostsApi extends PostsApi {
  @override
  HttpRequest<List<Post>> getPosts(int offset, int limit) => super.getPosts(offset, limit)
    ..simulateResult = Response(code: 200, result: [
      Post(
        title: '',
        body: '',
        id: 1,
      )
    ]);
}

void main() {
  test('PostsInteractorTest', () async {
    await initApp(testMode: true);

    app.apis.posts = MockPostsApi();

    final postsInteractor = PostsInteractor();

    postsInteractor.initialize(null);

    await postsInteractor.loadPosts(0, 30);

    expect((postsInteractor.state.posts! as SuccessData).result[0].id, 1);
  });
}

// ...

class PostInteractorMock extends PostInteractor {
  @override
  Future<void> loadPost(int id, {bool refresh = false}) async {
    updateState(state.copyWith(
      post: SuccessData(result: Post(id: 1)),
    ));
  }
}

void main() {
  test('PostViewModelTest', () async {
    await initApp(testMode: true);

    app.registerInstances();
    await app.createSingletons();

    final postInteractor = PostInteractorMock();
    app.instances.addBuilder<PostInteractor>(() => postInteractor);

    final postViewModel = PostViewModel();
    const mockWidget = PostView(id: 1);

    postViewModel
      ..initialize(mockWidget)
      ..onLaunch();

    expect((postViewModel.currentPost as SuccessData).result.id, 1);
  });
}

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('PostsListViewTest', () {
    testWidgets('PostsListViewTest InitialLoadTest', (tester) async {
      await initApp(testMode: true);

      app.registerInstances();
      await app.createSingletons();

      app.apis.posts = MockPostsApi();

      await tester.pumpAndSettle();

      await tester.pumpWidget(const MaterialApp(
        home: Material(child: PostsListView()),
      ));

      await Future.delayed(const Duration(seconds: 3), () {});

      await tester.pumpAndSettle();

      final titleFinder = find.text('TestTitle');

      expect(titleFinder, findsOneWidget);
    });
  });
} 
```

# 결론

우리는 아키텍처의 각 레이어를 구현하기 위한 논리적인 컴포넌트 세트를 생성했으며, 모든 것을 테스트할 수 있는 능력을 포함했습니다. 특히 DI와 HTTP 작업에 대해서는 아키텍처 구조에만 의존하여 다른 솔루션을 사용할 수 있습니다.

실제 프로젝트에서 이 아키텍처의 모든 컴포넌트를 활발하게 활용하고 있습니다. 이러한 컴포넌트를 테스트하는 것은 비즈니스 로직이 유닛 테스트로 완전히 커버되어 편리합니다.

<div class="content-ad"></div>

비즈니스 로직은 특정 수의 상호 작용자로 구성되어 있으며 대규모 프로젝트에서도 빠른 의존성 주입이 가능하며 여유로운 초기화를 보장하여 어떠한 지연도 없이 작동합니다.

이벤트 메커니즘 덕분에 구성 요소 간의 결합이 줄어듭니다. 주요 전역 앱 구성 요소를 통해 이러한 구성 요소를 코드의 어디서든 사용할 수 있어 기능을 구현하는 자유를 얻으면서도 테스트 가능성을 훼손하지 않습니다.

SwiftUI 및 Compose에서 유사한 메커니즘을 구현하는 방법에 대해 다가오는 기사에서 설명하겠습니다. 저희 뉴스레터 구독하고 더 많은 유용한 통찰력을 즐겨보세요!

![이미지](/assets/img/2024-08-21-HowtoCreateaFlutterArchitecturefromScratchTheSecondPart_5.png)

<div class="content-ad"></div>

🛸 디지턈 아이디어에 대한 견적을 받아보세요 👉 hello@redcollar.co

💻 웹사이트 | 링크드인 | 트위터 | 인스타그램 | 베한스 | 포트폴리오