---
title: "구글 엔지니어가 알려주는 2024 소프트웨어 엔지니어 인터뷰 완벽 대비 가이드"
description: ""
coverImage: "/blocktong.github.io/assets/no-image.jpg"
date: 2024-07-07 20:24
ogImage: 
  url: /blocktong.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Google Engineer’s Guide for Cracking Software Engineering Interviews"
link: "https://medium.com/@shivam-bunge/google-engineers-guide-for-cracking-software-engineering-interviews-e4c3d1f745ff"
---


# SOLID 원칙:

1. 단일 책임 원칙: 각 클래스는 하나의 역할만 가져야 합니다.
2. 개방/폐쇄 원칙: 클래스는 수정하지 않고 확장할 수 있어야 합니다.
3. 리스코프 치환 원칙: 하위 클래스는 부모 클래스로 대체할 수 있어야 합니다.
4. 인터페이스 분리 원칙: 일반적인 인터페이스보다 구체적이고 작은 인터페이스를 선호해야 합니다.
5. 의존성 역전 원칙: 구체적인 구현 대신 추상화에 의존해야 합니다.

# 리트코드 문제 해결 패턴:

1. 백트래킹 패턴: [백트래킹 문제에 대한 일반적인 접근 방식 (자바): subsets, permutations, combination sum, palindrome partitioning](https://leetcode.com/problems/permutations/solutions/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)/)
2. 비트 조작 패턴: [비트 조작 패턴에 대한 모든 유형과 활용 방법](https://leetcode.com/discuss/study-guide/4282051/all-types-of-patterns-for-bits-manipulations-and-how-to-use-it)
3. 동적 프로그래밍 패턴 1: [동적 프로그래밍 패턴](https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns)
4. DFS + BFS 패턴 (2): [DFS와 BFS 문제 해결 패턴 (파트 2)](https://medium.com/leetcode-patterns/leetcode-pattern-2-dfs-bfs-25-of-the-problems-part-2-a5b269597f52)
5. 트리 패턴: [반복적 또는 재귀적인 DFS 및 BFS 트리 순회 및 레벨 순서 출력](https://leetcode.com/discuss/study-guide/937307/Iterative-or-Recursive-or-DFS-and-BFS-Tree-Traversal-or-In-Pre-Post-and-LevelOrder-or-Views)
6. 슬라이딩 윈도우 패턴: [최빈 원소의 빈도](https://leetcode.com/problems/frequency-of-the-most-frequent-element/solutions/1175088/C++-Maximum-Sliding-Window-Cheatsheet-Template/)
7. 두 개의 포인터 패턴: [100일 동안 모든 두 포인터 문제 해결](https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days)
8. DFS + BFS 패턴 (1): [DFS와 BFS 문제 해결 패턴 (파트 1)](https://medium.com/leetcode-patterns/leetcode-pattern-1-bfs-dfs-25-of-the-problems-part-1-519450a84353)
9. 그래프 패턴: [초보자를 위한 그래프 문제, 패턴 및 샘플 솔루션](https://leetcode.com/discuss/study-guide/655708/Graph-For-Beginners-Problems-or-Pattern-or-Sample-Solutions)
10. 단조 스택 패턴: [모노토닉 스택 기반 문제 해결을 위한 포괄적인 가이드 및 템플릿](https://leetcode.com/discuss/study-guide/2347639/A-comprehensive-guide-and-template-for-monotonic-stack-based-problems)
11. 문자열 문제 패턴: [중요한 문자열 문제 패턴](https://leetcode.com/discuss/study-guide/2001789/Collections-of-Important-String-questions-Pattern)
12. 백트래킹 패턴: [백트래킹 (파트 3)](https://medium.com/leetcode-patterns/leetcode-pattern-3-backtracking-5d9e5a03dc26)
13. 동적 프로그래밍 패턴 2: [동적 프로그래밍 패턴](https://leetcode.com/discuss/study-guide/1437879/Dynamic-Programming-Patterns)
14. 이진 탐색 패턴: [파이썬 강력한 이진 탐색 템플릿](https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)
15. 문자열 질문 패턴: [중요한 문자열 질문 패턴 모음](https://leetcode.com/discuss/study-guide/2001789/Collections-of-Important-String-questions-Pattern)
16. 14 코딩 인터뷰 패턴: [코딩 인터뷰 문제 해결을 위한 14 가지 패턴](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)

<div class="content-ad"></div>

# 시스템 디자인 주요 개념:

1. 확장성: [Scalability](https://blog.algomaster.io/p/scalability)
2. 지연시간 대 처리량 비교: [Latency vs Throughput](https://aws.amazon.com/compare/the-difference-between-throughput-and-latency/)
3. CAP 이론: [CAP Theorem](https://www.bmc.com/blogs/cap-theorem/)
4. ACID 트랜잭션: [ACID Transactions](https://redis.io/glossary/acid-transactions/)
5. 속도 제한: [Rate Limiting](https://www.imperva.com/learn/application-security/rate-limiting/)
6. API 설계: [API Design](https://blog.wahab2.com/api-architecture-best-practices-for-designing-rest-apis-bf907025f5f?gi=d6ec53528d6c)
7. 강력 일관성 대 최종 일관성: [Strong vs Eventual Consistency](https://hackernoon.com/eventual-vs-strong-consistency-in-distributed-databases-282fdad37cf7)
8. 분산 추적: [Distributed Tracing](https://www.dynatrace.com/news/blog/what-is-distributed-tracing/)
9. 동기식 대 비동기식 통신: [Synchronous vs Asynchronous Communications](https://www.techtarget.com/searchapparchitecture/tip/Synchronous-vs-asynchronous-communication-The-differences)
10. 일괄 처리 대 스트림 처리: [Batch Processing vs Stream Processing](https://atlan.com/batch-processing-vs-stream-processing/)
11. 내고장성: [Fault Tolerance](https://www.cockroachlabs.com/blog/what-is-fault-tolerance/)

# ACID 속성:

1. 원자성: 트랜잭션은 분할할 수 없는 단위이며 완전히 성공하거나 완전히 실패해야 합니다.
2. 일관성: 각 트랜잭션은 데이터베이스의 일관성을 유지해야 합니다.
3. 격리성: 트랜잭션은 서로 영향을 주지 않고 독립적으로 실행되어야 합니다.
4. 지속성: 트랜잭션이 완료되면 시스템 장애를 견디더라도 지속되어야 합니다.