### 페이지 교체 알고리즘

---

**페이지 부재(Page Fault)**가 발생하여 새로운 페이지를 적재하기 위해 기존 페이지 중 하나를 대체하는 방법을 결정하는 알고리즘

<aside>
💡 페이지 부재(Page Fault)

</aside>

가상 메모리에서 발생하는 문제로 프로세스가 요청한 페이지가 현재 메모리에 존재하지 않는 경우

- 현재 필요한 페이지만 올려도 메모리는 결국 차게 됨
- 올라와있던 페이지가 사용 후에도 자리만 차지하고 있을 수 있음
1. **FIFO 알고리즘 (First-In-First-Out)**

---

**가장 먼저 메모리에 적재된 페이지를 대체하는 방식**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/21766bd8-4ae2-4592-a025-3bd3bd9a5803/Untitled.png)

- 가장 간단한 방식이고 오버헤드가 적음
- 향후 참조 가능성을 고려하지 않고 프레임에 올라온 순서대로 내쫓을 대상 선정
- **Beleady 모순 현상**이 발생할 수 있음

<aside>
💡 Beleady 모순 현상

</aside>

페이지 프레임 수를 늘려도 페이지 부재가 더 자주 발생하는 경우

1. **LRU 알고리즘 (Least Recently Used)**

---

**가장 최근에 사용되지 않은 페이지를 대체하는 방식**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea30cf37-1214-4012-9d6f-ac6d32074035/Untitled.png)

- 가장 오랫동안 참조되지 않은 것이 교체대상
- 정확성 면에서 가장 뛰어난 알고리즘
- 구현과 관리가 복잡하고 오버헤드가 큼
1. **LFU 알고리즘 (Least Frequently Used)**

---

**가장 적게 사용된 페이지를 대체하는 방식**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c98f0ed9-c001-41e5-ad41-f31f53dc5788/Untitled.png)

- 가장 참조 횟수가 적은 페이지를 교체
- 자주 사용되는 페이지는 유지 적게 사용되는 페이지는 대체
- 페이지 부재율 감소
- 페이지 사용 빈도를 추적하기 위해 오버헤드가 발생
- 사용패턴이 동적으로 변하는 경우에 대처하기 어려움

[[OS] 페이지 교체 알고리즘 (FIFO, LRU, LFU, NRU, NUR)](https://zangzangs.tistory.com/143)

[프레임과 페이지](https://velog.io/@seorim0801/프레임과-페이지)
