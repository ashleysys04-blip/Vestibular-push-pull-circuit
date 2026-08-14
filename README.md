# SOI STL을 이용한 Vestibular Push-Pull 및 Commissural Inhibition 회로

## 0. Abstract

뇌의 vestibular system은 좌우 labyrinth의 상반된 응답을 결합하는 **push-pull** 구조와, brainstem에서의 commissural inhibition(좌뇌와 우뇌가 서로 교차되면서 억제하는 메커니즘)을 통해 **dynamic range를 확장**하고 **common-mode noise를 상쇄**한다. 기존 neuromorphic vestibular 구현(Corradi et al., 2014, INI에서 나온 논문)에서는 다수의 comparator·timer 회로를 써야 한다.

본 연구는 SOI 소자의 STL특성인 positive feedback과 hysteresis window를 생물학적 상황에서의 Na+ channel의 regenerative depolarization과 refractory period라고 해석하려 한다.

그래서, 이를 vestibular push-pull + commissural inhibition 회로에 적용하여 회로 complexity와 필요한 에너지를 개선할 수 있는지를 LTspice 시뮬레이션으로 검증하려고 한다. 실제 소자의 측정값 혹은 TCAD 시뮬레이션의 결과로부터 소자의 charateristics를 얻어내 LTspice 시뮬레이션의 소자 안에 넣어주려고 한다.

---

## 1. Background

### 1.1 Biological background

Vestibular afferent는 자극이 없어도 높은 **resting spiking rate**를 유지한다. 이 baseline 덕분에 머리 회전은 한쪽의 발화 **증가**와 반대쪽의 발화 **감소**로 **양방향으로 encoding** 된다. 그러나 억제 방향에서는 발화율이 0에서 하한에 걸려(**inhibitory cutoff**, Ewald's second law로 알려진 비대칭성) 단측 신호만으로는 큰 자극에서 non-linearity가 발생한다. → push-pull 구조는 이를 compensate 할 수 있다.

Malinvaud et al.(2010): 개구리 whole brain 표본에서 commissural 경로가 feedforward (전용 경로) push-pull 회로를 구성한다는 사실을 보임. 자극 시 second-order vestibular neuron (**2°VN**)에서 관찰된 응답의 약 70%가 IPSP, 30%가 EPSP. (반대쪽 신경 자극 시 2°VN에서 70%는 억제 30%는 흥분 반응) 이때 IPSP는 glycinergic이 아닌 **순수 GABAergic**이었다. 즉 commissural inhibition은 단순한 "반대편 끄기"가 아니라, 억제와 흥분이 공존하는 구조적으로 선택적인 경로다. 근데 이걸 구현하긴 어려울 것 같다. 그래서 일단 억제만 하는 회로를 만들고 한계점에 30%의 EPSP를 쓴다던지 말던지.. 아니면 double latch로 해보던지 말던지..

Angelaki & Cullen(2008): vestibular 신호가 **velocity storage** 메커니즘을 통해 말초 afferent의 짧은 time constant보다 훨씬 긴 time constant로 연장되어 안구 운동 명령으로 변환된다는 사실. spike train → leaky integration 메커니즘에 대응 가능

### 1.2 Device Physical Background

대충 SOI 설명이랑 4개 피드백 그림이랑 그에 대응하는 EBD 그림...

| Biological | Device Physical | Circuit Analytical |
| --- | --- | --- |
| Na+ influx에 의한 depolarization | II로 생성된 hole의 body 축적 → V_t 감소, I_D latch up | All-or-none latch-up (comparator 불필요) |
| Firing threshold | Latch-up trigger |  |
| Refractory period | hysteresis window | Timer 회로 불필요 |
| Commissural IPSP |  | push pull stage |
| Velocity storage | RC leaky integrator | Spike → 연속 rate 복원 |

노벨티는: 2014년 뉴런 회로에서 가장 소자 수를 많이 잡아먹는 두 기능 (threshold 판별, refractory)이 한 소자로 구현된다는 사실... 이미 있으려나

### 1.3 Research Question

- **RQ1.** STL의 hysteresis window가 별도 comparator/timer 없이 biological refractory period와 rate coding을 대체할 수 있는가? 그때 neuron당 소자 수와 spike당 에너지는 subthreshold CMOS 대비 얼마나 감소하는가?
- **RQ2.** Latch의 all-or-none 특성이 cross-inhibition의 noise margin과 CMRR을 원래 회로 대비 얼마나 개선하는가?

## 마지막. References
- Malinvaud, D., Vassias, I., Reichenberger, I., Rössert, C., & Straka, H. (2010). Functional organization of vestibular commissural connections in frog. Journal of Neuroscience, 30(9), 3310–3325. doi:10.1523/JNEUROSCI.5318-09.2010
- Corradi, F., Zambrano, D., Raglianti, M., Passetti, G., Laschi, C., & Indiveri, G. (2014). Towards a neuromorphic vestibular system. IEEE Transactions on Biomedical Circuits and Systems, 8(5), 669–680. doi:10.1109/TBCAS.2014.2358493 (초기 버전: IEEE ISCAS 2014)
- Angelaki, D. E., & Cullen, K. E. (2008). Vestibular system: the many facets of a multimodal sense. Annual Review of Neuroscience, 31, 125–150.
- STL에다가 constant하지 않은 전류를 넣은 논문들
    - fig 2: Woo, S., Kim, S. Neural oscillation of single silicon nanowire neuron device with no external bias voltage. Sci Rep 12, 3516 (2022). https://doi.org/10.1038/s41598-022-07374-2
    - fig 7: I. M. Sheriff and R. Sakthivel, "The Artificial Neuron: Built From Nanosheet Transistors to Achieve Ultra Low Power Consumption," in IEEE Access, vol. 12, pp. 11653-11663, 2024, doi: 10.1109/ACCESS.2024.3350268.
    - fig 3: D. Lim, K. Cho and S. Kim, "Single Silicon Neuron Device Enabling Neuronal Oscillation and Stochastic Dynamics," in IEEE Electron Device Letters, vol. 42, no. 5, pp. 649-652, May 2021, doi: 10.1109/LED.2021.3063954.


