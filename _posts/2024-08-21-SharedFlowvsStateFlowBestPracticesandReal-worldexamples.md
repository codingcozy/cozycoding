---
title: "SharedFlow vs. StateFlow 최선의 방법과 실제 예시들"
description: ""
coverImage: "/assets/img/2024-08-21-SharedFlowvsStateFlowBestPracticesandReal-worldexamples_0.png"
date: 2024-08-21 18:10
ogImage: 
  url: /assets/img/2024-08-21-SharedFlowvsStateFlowBestPracticesandReal-worldexamples_0.png
tag: Tech
originalTitle: "SharedFlow vs. StateFlow Best Practices and Real-world examples"
link: "https://medium.com/@mortitech/sharedflow-vs-stateflow-a-comprehensive-guide-to-kotlin-flows-503576b4de31"
isUpdated: false
---


![img](/assets/img/2024-08-21-SharedFlowvsStateFlowBestPracticesandReal-worldexamples_0.png)

Kotlin flow의 세계로 빠져들어보세요. SharedFlow와 StateFlow의 깊이 있는 비교를 통해 두 유형의 Flow와 그 사용 사례를 살펴보겠습니다.

SharedFlow와 StateFlow는 모두 Kotlin의 kotlinx.coroutines 라이브러리의 일부이며, 비동기 데이터 스트림을 처리하기 위해 특별히 설계되었습니다. 두 Flow 모두 Flow를 기반으로 작성되었으며 서로 다른 목적을 위해 만들어졌습니다.

- SharedFlow:

<div class="content-ad"></div>

- SharedFlow는 여러 수집기가 있는 핫 플로우로, 수집기와 관계없이 독립적으로 값들을 방출할 수 있습니다. 여러 수집기가 동일한 값들을 플로우로부터 수집할 수 있습니다.
- 동일한 값을 복수의 수집기에 방송하거나 동일한 데이터 스트림에 여러 구독자가 필요할 때 유용합니다.
- 초기 값이 없으며, 재생 캐시를 구성하여 새로운 수집기를 위해 이전에 방출된 값들을 일정 수만큼 저장할 수 있습니다.

![SharedFlow vs StateFlow Best Practices and Real-world examples](/assets/img/2024-08-21-SharedFlowvsStateFlowBestPracticesandReal-worldexamples_1.png)

예시 사용법:

```kotlin
val sharedFlow = MutableSharedFlow<Int>()

// sharedFlow로부터 값 수집
launch {
    sharedFlow.collect { value ->
        println("수집기 1이 수신한 값: $value")
    }
}

// sharedFlow로부터 값 수집
launch {
    sharedFlow.collect { value ->
        println("수집기 2가 수신한 값: $value")
    }
}

// sharedFlow에 값 방출
launch {
    repeat(3) { i ->
        sharedFlow.emit(i)
    }
}
```

<div class="content-ad"></div>

- StateFlow:

- StateFlow은 한 번에 하나의 값을 보유하는 상태를 나타내는 핫 플로우입니다. 또한 콘플레이티드 플로우이기도 한데, 새 값이 발생할 때 가장 최근의 값은 유지되고 즉시 새로운 수집기에 전달됩니다.
- 상태의 단일 진실의 원천을 유지하고 최신 상태로 모든 수집기를 자동으로 업데이트해야 할 때 유용합니다.
- 항상 초기값이 있으며, 가장 최근에 발생한 값만 저장합니다.

![StateFlow](/assets/img/2024-08-21-SharedFlowvsStateFlowBestPracticesandReal-worldexamples_2.png)

사용 예시:

<div class="content-ad"></div>

```kotlin
val mutableStateFlow = MutableStateFlow(0)
val stateFlow: StateFlow<Int> = mutableStateFlow

// stateFlow에서 값을 수집합니다.
launch {
    stateFlow.collect { value ->
        println("수집기 1이 받은 값: $value")
    }
}

// stateFlow에서 값을 수집합니다.
launch {
    stateFlow.collect { value ->
        println("수집기 2가 받은 값: $value")
    }
}

// 상태 업데이트하기
launch {
    repeat(3) { i ->
        mutableStateFlow.value = i
    }
}
```

최상의 실천법

여기 Kotlin에서 SharedFlow와 StateFlow를 사용하는 데 도움이 되는 몇 가지 최상의 실천법이 있습니다:

- 적절한 Flow를 선택하십시오:


<div class="content-ad"></div>

- 여러 수집기에 값들을 브로드캐스트하거나 동일한 데이터 스트림에 여러 구독자를 가질 때 SharedFlow를 사용하세요.
- 어떤 상태에 대한 단일 참 소스를 유지 및 공유하며 모든 수집기가 최신 상태로 자동으로 업데이트되어야 할 때 StateFlow를 사용하세요.

2. Mutable flows를 캡슐화하세요:

외부 변경을 방지하려면 mutable flow의 읽기 전용 버전을 노출하세요. MutableSharedFlow에 대해 SharedFlow 인터페이스를 사용하거나 MutableStateFlow에 대해 StateFlow 인터페이스를 사용하여 이를 수행할 수 있습니다.

```kotlin
class ExampleViewModel {
    private val _mutableSharedFlow = MutableSharedFlow<Int>()
    // 이 mutable shared flow를 읽기 전용 shared flow로 표현합니다.
    val sharedFlow = _mutableSharedFlow.asSharedFlow()

    private val _mutableStateFlow = MutableStateFlow(0)
    // 이 mutable state flow를 읽기 전용 state flow로 표현합니다.
    val stateFlow = _mutableStateFlow.asStateFlow()
}
```

<div class="content-ad"></div>

3. 자원을 적절하게 관리하세요:

SharedFlow나 StateFlow를 사용할 때, 더 이상 필요하지 않을 때는 코루틴이나 콜렉터를 취소하여 자원을 적절하게 관리해야 합니다.

```kotlin
val scope = CoroutineScope(Dispatchers.Main)

val sharedFlow = MutableSharedFlow<Int>()

val job = scope.launch {
    sharedFlow.collect { value ->
        println("Received: $value")
    }
}

// 나중에 콜렉터가 더 이상 필요하지 않을 때
job.cancel()
```

<div class="content-ad"></div>

- SharedFlow의 경우, 버퍼 용량과 재생 용량을 설정할 수 있습니다. 백프레셔 문제를 피하기 위해 적절한 버퍼 용량을 선택하고, 사용 사례 요구 사항에 따라 재생 용량을 설정하세요.
- StateFlow의 경우, 항상 크기 1의 재생 캐시를 가지고 있으며, 새로운 수집기에 대한 최신 값을 유지합니다.

```kotlin
val sharedFlow = MutableSharedFlow<Int>(
    replay = 2, // 마지막 2개의 값 재생 - 새 수집기에
    extraBufferCapacity = 8 // 백프레셔 방지를 위한 추가 버퍼 용량
)
```

5. `combine`, `map`, `filter` 및 기타 연산자 활용:

필요한 대로 데이터를 변환, 결합 또는 필터링하기 위해 Kotlin Flow 연산자를 활용하세요. 이를 통해 더 표현력 있는 효율적인 코드를 작성할 수 있습니다.

<div class="content-ad"></div>

```kotlin
val flow1 = MutableStateFlow(1)
val flow2 = MutableStateFlow(2)

val combinedFlow = flow1.combine(flow2) { value1, value2 ->
    value1 + value2
}

// Collect and print the sum of flow1 and flow2
launch {
    combinedFlow.collect { sum ->
        println("Sum: $sum")
    }
}
```

6. Errors should be properly handled:

When working with flows, make sure to handle exceptions appropriately. Utilize the catch operator to manage exceptions within the flow pipeline and the onCompletion operator to execute cleanup tasks or respond to flow completion.

```kotlin
val flow = flow {
    emit(1)
    throw RuntimeException("Error occurred")
    emit(2)
}.catch { e ->
    // Handle the exception and emit a default value
    emit(-1)
}

launch {
    flow.collect { value ->
        println("Received: $value")
    }
}
```

<div class="content-ad"></div>

7. lifecycleScope, repeatOnLifecycle 및 기타 라이프사이클 인식 연산자 사용하기:

Android나 다른 라이프사이클을 가진 플랫폼에서 작업할 때는 흐름의 라이프사이클을 자동으로 관리하기 위해 라이프사이클 인식 연산자를 활용해보세요.

```kotlin
class MyFragment : Fragment() {
    private val viewModel: MyViewModel by viewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        viewLifecycleOwner.lifecycleScope.launch {
            // 라이프사이클이 DESTROYED 상태가 될 때까지 코루틴을 일시 중단합니다.
            // repeatOnLifecycle은 라이프사이클이 STARTED 상태 (또는 그 이상)일 때마다 블록을 새 코루틴에서 실행하고,
            // STOPPED 상태일 때는 취소합니다.
            repeatOnLifecycle(Lifecycle.State.STARTED) {
                // 라이프사이클이 STARTED 상태일 때 locations에서 안전하게 수집하고,
                // STOPPED 상태일 때 수집을 중지합니다.
                viewModel.stateFlow.collect { value ->
                    // 값으로 UI 업데이트
                }
            }
            // 참고: 이 시점에서 라이프사이클은 DESTROYED 상태입니다!
        }
    }
}
```

실제 예시

<div class="content-ad"></div>

StateFlow 사용 사례: 라이브 데이터

주식 가격, 날씨 정보 또는 채팅 메시지와 같은 라이브 데이터를 표시하는 애플리케이션이 있다고 가정해 봅시다. StateFlow를 사용하면 최신 데이터를 유지하고 UI를 자동으로 업데이트할 수 있습니다.

```js
// StockViewModel.kt
class StockViewModel {
    // 주식 가격을 보유하는 개인 MutableStateFlow 속성을 생성합니다.
    private val _stockPrice = MutableStateFlow(0.0)
    // 주식 가격을 불변의 값으로 노출하는 공개 StateFlow 속성을 생성합니다.
    val stockPrice = _stockPrice.asStateFlow()

    init {
        // 주식 가격을 업데이트하기 위해 viewModelScope에서 코루틴을 시작합니다.
        viewModelScope.launch {
            updateStockPrice()
        }
    }

    private suspend fun updateStockPrice() {
        while (true) {
            delay(1000) // 매 초마다 업데이트합니다.
            val newPrice = fetchNewPrice()
            // MutableStateFlow의 값 속성을 사용하여 주식 가격을 업데이트합니다.
            _stockPrice.value = newPrice
        }
    }

    // API 또는 데이터 소스에서 새 주식 가격을 가져오는 개인 함수
    private suspend fun fetchNewPrice(): Double {
        // TODO: API 또는 데이터 소스에서 새 주식 가격을 가져옵니다.
        return 0.0
    }
}

// MainActivity.kt
class MainActivity : AppCompatActivity() {
    private val stockViewModel = StockViewModel()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // 주식 가격의 변경 사항을 관찰하기 위해 lifecycleScope에서 코루틴을 시작합니다.
        lifecycleScope.launch {
            // repeatOnLifecycle 함수를 사용하여 활동이 시작될 때만 코루틴이 활성화되도록 합니다.
            repeatOnLifecycle(Lifecycle.State.STARTED) {
                // collect 연산자를 사용하여 주식 가격의 변경 사항을 관찰합니다.
                stockViewModel.stockPrice.collect { price ->
                    // 새 주식 가격으로 UI를 업데이트합니다.
                    stockPriceTextView.text = "주식 가격: $price"
                }
            }
        }
    }
}
```

SharedFlow 사용 사례 1: 채팅 메시징 애플리케이션

<div class="content-ad"></div>

실시간 채팅 애플리케이션을 SharedFlow와 모범 사례를 사용하여 만들고 싶다고 가정해 봅시다. 여러 사용자의 채팅 메시지를 수신하는 ChatRepository, 메시지 흐름을 처리하는 ChatViewModel 및 메시지를 표시하는 Android 액티비티가 있을 것입니다.

- ChatRepository.kt: 여러 사용자에게 메시지를 보내고 받는 것을 시뮬레이션하는 ChatRepository입니다.

```kotlin
class ChatRepository {
    private val coroutineScope = CoroutineScope(Dispatchers.IO + SupervisorJob())

    // 수신되는 채팅 메시지를 보관하는 MutableSharedFlow 속성 생성
    private val _incomingMessages = MutableSharedFlow<ChatMessage>(extraBufferCapacity = 64)
    // 수신되는 채팅 메시지를 불변 값으로 노출하는 public SharedFlow 속성 생성
    val incomingMessages: _incomingMessages.asSharedFlow()

    init {
        // 수신되는 채팅 메시지 시뮬레이션을 위해 simulateIncomingMessages 함수 호출
        simulateIncomingMessages()
    }

    /**
     * 새로운 채팅 메시지를 서버에 보내고 UI에 표시하기 위해 흐름으로 emit합니다.
     */
    fun sendMessage(username: String, content: String) {
        // IO 스코프에서 코루틴을 시작하여 채팅 메시지를 _incomingMessages 흐름에 emit합니다.
        coroutineScope.launch {
            _incomingMessages.emit(ChatMessage(username, content))
        }
    }

    private fun simulateIncomingMessages() {
        coroutineScope.launch {
            while (true) {
                // 무작위 메시지 지연을 시뮬레이션하기 위해 500에서 2000 밀리초 사이의 임의의 시간 동안 기다립니다.
                delay(Random.nextLong(500, 2000)) 
                // 임의의 송신자와 내용을 갖는 새로운 채팅 메시지 생성
                val message = ChatMessage("User ${Random.nextInt(1, 6)}", "Hello, world!")
                // 채팅 메시지를 _incomingMessages 흐름으로 emit합니다.
                _incomingMessages.emit(message)
            }
        }
    }

    /**
     * [ChatRepository] 인스턴스에서 시작된 모든 코루틴을 취소합니다.
     */
    fun cancel() {
        coroutineScope.cancel()
    }
}

data class ChatMessage(val sender: String, val content: String)
```

2. ChatViewModel.kt: 메시지를 보내고 수신된 메시지를 노출하는 ChatViewModel을 생성합니다.

<div class="content-ad"></div>


class ChatViewModel(private val chatRepository: ChatRepository) : ViewModel() {

    /**
     * [SharedFlow] 형식의 수신 채팅 메시지.
     *
     * 이 flow는 수신된 채팅 메시지를 도착하는 대로 방출하며, 
     * 클라이언트가 채팅 기록을 표시하기 위해 관찰할 수 있습니다.
     */
    val incomingMessages: SharedFlow<ChatMessage> = chatRepository.incomingMessages

    fun sendMessage(username: String, content: String) {
        chatRepository.sendMessage(username, content)
    }
}


3. ChatActivity.kt

```kotlin
class ChatActivity : AppCompatActivity() {
    private val viewModel: ChatViewModel by viewModels()
    private val chatAdapter = ChatAdapter()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // UI 구성 요소 설정 및 어댑터 초기화
        setContentView(R.layout.activity_chat)

        val recyclerView = findViewById<RecyclerView>(R.id.recyclerView)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = chatAdapter

        // 도착하는 메시지를 관찰하고 목록에 표시
        lifecycleScope.launch {
            viewModel.incomingMessages.collect { message ->
                chatAdapter.addMessage(message)
                binding.recyclerView.scrollToPosition(chatAdapter.itemCount - 1)
            }
        }
    }
}

// 수신 메시지를 목록에 표시하기 위한 RecyclerView
class ChatAdapter : RecyclerView.Adapter<ChatAdapter.ViewHolder>() {
    private val messages = mutableListOf<ChatMessage>()

    fun addMessage(message: ChatMessage) {
        messages.add(message)
        notifyItemInserted(messages.size - 1)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_chat_message, parent, false)
        return ViewHolder(view)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bind(messages[position])
    }

    override fun getItemCount() = messages.size

    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        private val senderTextView: TextView = itemView.findViewById(R.id.senderTextView)
        private val contentTextView: TextView = itemView.findViewById(R.id.contentTextView)

        fun bind(chatMessage: ChatMessage) {
            senderTextView.text = chatMessage.sender
            contentTextView.text = chatMessage.content
        }
    }
}
```

ChatRepository.kt 코드 확인하러 가기:


<div class="content-ad"></div>

SharedFlow 사용 사례 2: 이벤트 버스

공유된 플로우를 사용한 더 고급 예제를 보여드리겠습니다. SharedFlow를 사용하여 여러 리스너에 이벤트를 방송하는 이벤트 버스를 생성하고자 합니다.

- EventBus.kt: 공유된 플로우를 사용하는 일반적인 EventBus 클래스를 정의합니다.

```kotlin
/**
 * 여러 리스너에 이벤트를 방송하는 공유된 플로우를 사용하는 이벤트 버스 구현
 */
class EventBus<T> {
    // 이벤트를 보유하기 위한 개인 MutableSharedFlow 속성 생성
    private val _events = MutableSharedFlow<T>(replay = 0, extraBufferCapacity = 64)
    // 이벤트를 불변 값으로 노출하는 공용 SharedFlow 속성 생성
    val events: Flow<T> = _events.asSharedFlow()

    /**
     * 이벤트를 공유된 플로우에 전송합니다.
     */
    suspend fun sendEvent(event: T) {
        _events.emit(event)
    }
}
```

<div class="content-ad"></div>

2. Event.kt: 다양한 이벤트 유형을 정의합니다.

```kotlin
sealed class Event {
    object EventA : Event()
    object EventB : Event()
    data class EventC(val value: Int) : Event()
}
```

3. EventListener.kt: 특정 이벤트 유형을 구독하는 책임을 지닌 EventListener 클래스를 생성합니다.

```kotlin
class EventListener(
    private val eventBus: EventBus<Event>,
    private val scope: CoroutineScope
) {
    init {
        // onEach 오퍼레이터를 사용하여 이벤트 플로우 구독
        eventBus.events
            .onEach { event ->
                // 서로 다른 유형의 이벤트를 처리하기 위해 when 표현식 사용
                when (event) {
                    is Event.EventA -> handleEventA()
                    is Event.EventB -> handleEventB()
                    is Event.EventC -> handleEventC(event.value)
                }
            }
            // 주어진 코루틴 범위 내에서 이벤트 리스너 실행
            // 범위가 더 이상 존재하지 않을 때 구독을 취소할 수 있음
            .launchIn(scope)
    }

    // 특정 유형의 이벤트를 처리하는 비공개 함수들
    private fun handleEventA() {
        println("EventA 수신됨")
    }

    private fun handleEventB() {
        println("EventB 수신됨")
    }

    private fun handleEventC(value: Int) {
        println("EventC 수신됨. 값: $value")
    }
}
```

<div class="content-ad"></div>

4. Main.kt: EventBus 및 EventListener을 생성한 후 EventBus를 사용하여 이벤트를 전송합니다.

```kotlin
fun main() = runBlocking {
    val eventBus = EventBus<Event>()
    // 이벤트 버스에서 이벤트 수신을 시작하도록 EventListener를 생성합니다    
    val eventListener = EventListener(eventBus, this)

    // 별도의 코루틴에서 이벤트 버스를 사용하여 이벤트를 전송합니다
    launch(Dispatchers.Default) {
        delay(1000)
        eventBus.sendEvent(Event.EventA)

        delay(1000)
        eventBus.sendEvent(Event.EventB)

        delay(1000)
        eventBus.sendEvent(Event.EventC(42))
    }

    // 리스너가 이벤트를 처리하는 데 필요한 메인 코루틴 범위를 유지합니다
    delay(5000)
}
```

eventListener 변수가 여전히 main 함수 내에서 명시적으로 사용되지 않는 것에 유의하십시오. 그러나 이 변수를 생성하면 EventListener 인스턴스가 메모리에 유지되고 가비지 수집되지 않아 이벤트를 수신하지 못하게 될 가능성이 줄어듭니다. eventListener 변수를 생성하고 EventListener 인스턴스를 할당함으로써 리스너가 활성 상태로 유지되며 eventBus로 전송된 이벤트가 예상대로 처리됩니다.

<div class="content-ad"></div>

이 예제는 SharedFlow의 더 고급 사용법을 보여줍니다. 여기서는 여러 리스너에 이벤트를 방송하기 위해 일반적인 EventBus 클래스를 작성합니다. EventListener 클래스는 특정 이벤트 유형을 수신하고 이에 따라 처리합니다. 코드는 변경 가능한 플로우를 캡슐화하고 리소스를 관리하며 다양한 이벤트 유형을 효율적으로 처리하는 등의 모베스트 프랙티스를 따릅니다.

이 게시물에서는 SharedFlow 및 StateFlow에 대해 필요한 모든 내용을 다루려고 노력했습니다. 이 기사가 여러분의 애플리케이션에서 이들을 사용하는 최상의 방법을 찾는 데 도움이 되기를 바랍니다.

SharedFlow 및 StateFlow를 어떻게 활용하는지 보려면 채팅 리포지토리를 확인해 주세요. 또한 이 기사에 대한 여러분의 의견을 알려주세요.