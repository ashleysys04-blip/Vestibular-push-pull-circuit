# Nano Letters Target Research Plan
## Hysteretic Antagonistic Silicon Devices for History-Dependent Vestibular Encoding

## 1. 연구의 핵심 아이디어

본 연구는 single-transistor latch(STL)의 **intrinsic hysteresis**를 단순한 switching 또는 spike-generation 현상으로 사용하는 것을 넘어, 두 개의 hysteretic silicon device를 antagonistic하게 구성하여 **이전 motion state를 기억하면서 방향 전환을 판단하는 stateful neuromorphic primitive**를 구현하는 것을 목표로 한다.

생물학적 vestibular system에서는 좌우 semicircular canal과 vestibular pathway가 reciprocal/push-pull 방식으로 동작하며, 한쪽 방향의 회전은 한쪽 pathway의 활동을 증가시키는 동시에 반대쪽 pathway와 상반된 response를 형성한다.

본 연구에서는 이를 단순히 두 개의 positive/negative sensor channel로 모사하는 것이 아니라, **각 device가 가진 latch-up/latch-down hysteresis를 directional memory로 사용**한다.

개념적으로,

```text
RIGHT → NEUTRAL → weak LEFT
```

와 같은 입력이 들어오더라도 RIGHT device가 즉시 reset되지 않으며,

\[
|I_{\mathrm{LEFT}}| > I_{\mathrm{reversal}}
\]

인 충분히 강한 반대 방향 입력이 들어왔을 때에만 LEFT state로 전환되도록 한다.

따라서 device의 hysteresis window는 단순한 electrical parameter가 아니라,

> **이전에 형성된 directional state를 뒤집기 위해 필요한 반대 방향 stimulus의 크기**

라는 computational meaning을 갖는다.

---

## 2. 핵심 연구 질문

본 연구에서 답하고자 하는 질문은 다음과 같다.

> **Can intrinsic hysteresis in silicon devices implement an antagonistic, history-dependent state machine for vestibular motion encoding without explicit digital memory?**

기존 neuromorphic vestibular 연구에서는 rotation sensor와 neuristor를 이용하여 angular velocity를 spike frequency로 변환하고, 이를 SNN에서 rotation-axis recognition에 이용하였다. 즉 기존 연구의 핵심 representation은 **instantaneous rotation magnitude → spike frequency**였다.

본 연구에서는 representation 자체를 다르게 설정한다.

\[
\boxed{
\text{Present motion}
+
\text{Previous directional state}
\rightarrow
\text{Current directional state}
}
\]

즉 단순한 rate coding이 아니라 **state-dependent encoding**을 구현한다.

---

## 3. Central Hypothesis

STL device의 latch-up voltage \(V_{LU}\)와 latch-down voltage \(V_{LD}\) 사이에 형성되는 hysteresis window

\[
\Delta V_H = V_{LU} - V_{LD}
\]

또는 이에 대응하는 current window를 이용하면 이전 입력의 history를 device state 자체에 저장할 수 있다.

두 개의 device를 LEFT/RIGHT channel로 구성하고 antagonistic coupling을 적용하면,

\[
S(t)=F[x(t),S(t-1)]
\]

형태의 state transition을 별도의 digital memory 없이 device physics에서 구현할 수 있을 것으로 예상한다.

여기서 가장 중요한 점은 동일한 instantaneous input에서도 이전 state에 따라 서로 다른 output이 발생할 수 있다는 것이다.

예를 들어,

### History A

```text
Strong Right → +0.2
```

에서는

\[
S = RIGHT
\]

이지만,

### History B

```text
Strong Left → +0.2
```

에서는

\[
S = LEFT
\]

가 유지될 수 있다.

즉,

\[
x_A(t_0)=x_B(t_0)
\]

임에도

\[
S_A(t_0)\neq S_B(t_0)
\]

가 된다.

이것이 단순 threshold detector 또는 instantaneous function approximation과 본 연구를 구분하는 가장 중요한 결과가 된다.

---

## 4. Proposed Novelty

본 논문의 novelty는 vestibular system 자체가 아니라 다음 세 요소의 결합에 둔다.

### Novelty 1. Hysteresis as a computational variable

기존 STL 연구에서 주로 switching/oscillation 특성으로 취급되었던 \(V_{LU}\), \(V_{LD}\), hysteresis window를 **state-retention 및 direction-reversal threshold**라는 computation parameter로 재해석한다.

### Novelty 2. Antagonistic coupling of hysteretic silicon devices

두 개의 hysteretic device를 서로 반대 방향을 encode하는 pair로 구성하여

\[
RIGHT \leftrightarrow LEFT
\]

state transition을 구현한다.

단순한 positive/negative signal separation이 아니라 **device state 간 competition**을 구현하는 것이 목표이다.

### Novelty 3. History-dependent sensory encoding

현재 input만으로 output이 결정되는 conventional encoding과 달리,

\[
Output(t)=f(Input(t),State(t-1))
\]

인 sensory encoding을 device physics 자체에서 구현한다.

Vestibular motion은 이러한 기능을 보여주기 위한 대표적인 application이다.

---

## 5. 전체 연구 구성

Nano Letters target에서는 연구를 다음 네 layer로 연결한다.

```text
Experimental device physics
        ↓
TCAD
        ↓
Antagonistic circuit
        ↓
Vestibular demonstration
```

### Experimental device physics
실제 silicon device의 latch 및 hysteresis characterization.

### TCAD
Latch mechanism 규명 및 hysteresis-window controllability.

### Antagonistic circuit
두 device를 이용한 LEFT/RIGHT state machine 구현.

### Vestibular demonstration
실제 또는 realistic rotation waveform에서 history-dependent directional encoding 검증.

중요한 점은 네 layer가 독립적인 결과가 아니라 하나의 claim으로 연결되어야 한다는 것이다.

> **A tunable device hysteresis enables an antagonistic physical state machine, which provides history-dependent vestibular encoding.**

---

# 6. Main Figure 구성

Nano Letters는 짧고 응집된 Letter 형식이므로, 계획 단계부터 **5개의 main figure**를 기준으로 논리 구조를 설계한다. 세부 characterization, simulation parameter, 추가 waveform 등은 Supporting Information으로 이동한다.

---

## Figure 1. Concept of a Hysteretic Antagonistic Vestibular Unit

**목적:** 논문의 전체 아이디어를 한 장에서 이해시키기.

### Fig. 1a — Biological inspiration

좌우 semicircular canal / bilateral vestibular pathway를 간략하게 나타내고,

- right rotation
- right excitation
- left antagonistic response

를 schematic으로 표현한다.

생물학을 과도하게 모사했다고 주장하지 않고,

> **inspired by the bilateral push-pull organization of the vestibular system**

정도로 positioning한다.

### Fig. 1b — Hysteretic device concept

Single device의 대표 hysteresis curve를 배치한다.

표시할 값:

- \(V_{LU}\)
- \(V_{LD}\)
- hysteresis window
- HRS/LRS 또는 OFF/ON state

그리고 window 위에 **state-retention region**이라는 physical interpretation을 표시한다.

### Fig. 1c — Proposed antagonistic pair

두 device를

```text
RIGHT device ↔ LEFT device
```

로 배치하고 cross-coupling 구조를 제시한다.

Input:

\[
x(t)>0 \rightarrow R
\]

\[
x(t)<0 \rightarrow L
\]

### Fig. 1d — State-transition concept

간단한 two-state diagram:

\[
LEFT \rightleftarrows RIGHT
\]

전환 화살표에는 **sufficient opposite stimulus** 조건을 표시한다.

### Figure 1 핵심 메시지

> **Hysteresis converts directional stimulation into a persistent physical state.**

---

## Figure 2. Experimental Demonstration of the Hysteretic Silicon Device

**목적:** 논문의 foundation이 simulation이 아니라 실제 device physics라는 것을 보여주기.

### Fig. 2a
Device cross-section / structure.

가능하다면 SEM/TEM 또는 실제 fabrication schematic 포함.

### Fig. 2b
기본 \(I_D-V_D\) 또는 해당 STL characteristic.

Latch-up / latch-down을 명확히 표시한다.

### Fig. 2c
Multiple sweeps 또는 반복 측정.

최소한 다음을 보여준다.

- repeatability
- \(V_{LU}\)
- \(V_{LD}\)
- window distribution

### Fig. 2d
Transient input에서 state retention.

예:

\[
0\rightarrow V_{\mathrm{high}}\rightarrow V_{\mathrm{mid}}
\]

로 입력을 낮췄음에도 latch state가 유지되는 것을 보여준다.

### Fig. 2e
Input amplitude 또는 current에 따른 switching/state response.

### Figure 2 핵심 메시지

> **The fabricated silicon device provides a reproducible hysteretic state variable rather than merely a transient nonlinear response.**

---

## Figure 3. Physical Origin and Engineering of the Hysteresis Window

**목적:** “그냥 우연히 latch되는 device를 application에 썼다”는 공격을 방어한다.

TCAD가 가장 중요한 figure이다.

### Fig. 3a — Physical mechanism

Latch 이전과 이후의 다음 물리량을 비교한다.

- impact ionization
- hole concentration
- body potential
- electric field
- carrier distribution

최종적으로,

```text
impact ionization
        ↓
body charge accumulation
        ↓
barrier modulation
        ↓
positive feedback
        ↓
latch
```

라는 mechanism을 보여준다.

### Fig. 3b
Latch-up condition에서 device 내부 spatial distribution.

### Fig. 3c
Latch-down condition에서 device 내부 spatial distribution.

### Fig. 3d — Tunability

하나 이상의 controllable parameter에 대해

\[
V_{LU},V_{LD}
\]

변화를 보여준다.

후보:

- input current
- gate bias
- body contact condition
- BOX thickness
- device geometry

### Fig. 3e — Window engineering map

가능하면 heat map 또는 phase map 형태:

\[
\Delta V_H=f(P_1,P_2)
\]

단순히 threshold가 변하는 것이 아니라,

> **The reversal threshold of the physical state machine can be engineered through device parameters.**

까지 연결한다.

---

## Figure 4. Antagonistic Two-Device State Machine

**이 논문의 핵심 figure.**

Nano Letters acceptance 가능성을 실제로 결정할 가능성이 가장 높은 결과이다.

### Fig. 4a — Circuit

두 hysteretic device와 최소한의 coupling element로 구성한 antagonistic circuit.

LTspice는 circuit design 및 parameter exploration 단계에서 사용하되, Nano Letters submission에서는 가능하면 **measured device 기반 또는 실제 hardware coupled response**를 확보한다.

### Fig. 4b — Directional input splitting

Bipolar input \(x(t)\)를

\[
I_R=\max(x,0)
\]

\[
I_L=\max(-x,0)
\]

형태로 분리한다.

### Fig. 4c — Basic push-pull operation

입력:

```text
0
→ R_strong
→ 0
→ L_weak
→ 0
→ L_strong
```

출력:

```text
N
→ R
→ R
→ R
→ R
→ L
```

을 한 waveform에서 보여준다.

### Fig. 4d — Same input, different history

본 연구에서 가장 중요한 experiment.

두 waveform의 마지막 입력은 동일하게 하고, history condition만 다르게 한다.

결과:

\[
x_A(t_0)=x_B(t_0)
\]

but

\[
S_A(t_0)\neq S_B(t_0)
\]

이를 통해 **history-dependent computation**을 직접 증명한다.

### Fig. 4e — State-transition map

- x축: opposite input amplitude
- y축: initial state 또는 stimulus duration

결과를

- retain previous state
- transition state

두 영역으로 나누어 phase diagram으로 표현한다.

이 boundary가 device hysteresis window와 quantitative하게 대응하면 매우 강한 결과가 된다.

---

## Figure 5. Vestibular Motion Encoding and Benchmark

Figure 5는 **“그래서 이 physical state machine이 왜 필요한가?”**에 대한 답이다.

### Fig. 5a — Realistic rotation input

TENG를 main sensor로 사용하지 않는다.

대신 commercial gyroscope/IMU 또는 rotation-stage 기반 signal을 사용하여 실제 angular-velocity waveform을 획득한다.

가능하면 사람의 움직임보다 **motorized rotation stage + IMU**를 사용하여 reproducible한 input을 만든다.

Input에는 다음을 포함한다.

- clockwise rotation
- counterclockwise rotation
- reversal
- small oscillatory disturbance
- random noise

### Fig. 5b — Stateful output

동일 input에 대해

- raw angular velocity
- R-channel
- L-channel
- final directional state

를 함께 표시한다.

### Fig. 5c — Noise rejection

작은 zero-crossing/noise가 존재해도 state가 계속 flip되지 않고 이전 방향을 유지한다는 것을 보인다.

### Fig. 5d — Baseline comparison

최소한 다음 중 하나와 비교한다.

- nonhysteretic threshold detector
- memoryless spiking/threshold encoder

동일 noisy input에서 conventional threshold output은 여러 차례 flip하지만 proposed device는 안정적인 state를 유지하는 것을 보여준다.

### Fig. 5e — Quantitative benchmark

가능한 metric:

- false direction transitions
- direction-detection accuracy
- reversal-detection accuracy
- minimum reversal stimulus
- noise tolerance
- state retention
- switching latency
- energy per state transition

모든 metric을 다 넣을 필요는 없다.

**2–3개의 명확한 metric**을 선택해 강조한다.

### Figure 5 핵심 메시지

> **Intrinsic device hysteresis suppresses insignificant perturbations while preserving meaningful motion-history-dependent state transitions.**

---

# 7. Double-Latch Result의 위치

Double latch는 현재 main claim으로 올리지 않는 것이 좋다.

실제 fabricated device에서 안정적으로 double latch가 확인되지 않는다면 main figure에 넣을 경우 오히려 논문의 coherence를 깨뜨릴 수 있다.

따라서 TCAD에서 구현되는 double latch는 Supporting Information에

## Extension toward multilevel antagonistic encoding

으로 포함한다.

예를 들어 향후,

```text
Strong Left
Weak Left
Neutral
Weak Right
Strong Right
```

와 같은 multi-level state representation으로 확장할 수 있음을 보여준다.

실제 experimental double latch가 reproducible하게 확보되는 경우에만 main text로 승격한다.

---

# 8. Supporting Information 구성

예상 SI:

- Figure S1. Full device fabrication/process details
- Figure S2. Additional DC characteristics
- Figure S3. Sweep-rate dependence
- Figure S4. Device-to-device variation
- Figure S5. Repeated switching / endurance
- Figure S6. Temperature dependence, 가능할 경우
- Figure S7. TCAD mesh and model parameters
- Figure S8. Impact-ionization model comparison
- Figure S9. Parameter dependence of \(V_{LU}\) and \(V_{LD}\)
- Figure S10. LTspice/behavioral device model extraction
- Figure S11. Antagonistic-circuit parameter sensitivity
- Figure S12. Additional noise waveforms
- Figure S13. Additional IMU/rotation-stage experiments
- Figure S14. Double-latch multilevel extension

---

# 9. Nano Letters Manuscript Storyline

실제 manuscript는 다음 흐름으로 작성한다.

### Paragraph 1 — Problem

Neuromorphic sensory systems에서는 sensor input을 spike/rate로 encode하는 연구가 많이 수행되어 왔으나, temporal context와 previous state를 유지하기 위해서는 별도의 memory/network dynamics가 필요한 경우가 많다.

### Paragraph 2 — Biological motivation

Vestibular system의 bilateral antagonistic organization을 소개한다.

### Paragraph 3 — Proposed concept

Intrinsic hysteresis를 가진 silicon device pair를 이용해 directional state와 history를 physical state로 encode한다는 핵심 아이디어 제시.

→ **Figure 1**

### Paragraph 4–5 — Experimental device

Device structure와 hysteresis 결과.

→ **Figure 2**

### Paragraph 6–7 — Mechanism and controllability

TCAD 및 window tuning.

→ **Figure 3**

### Paragraph 8–10 — Antagonistic computation

Push-pull circuit 및 history dependence.

→ **Figure 4**

### Paragraph 11–12 — Vestibular demonstration

Realistic motion/noise 및 baseline comparison.

→ **Figure 5**

### Final paragraph

Silicon-device hysteresis가 단순 switching mechanism을 넘어 **stateful sensory-computing primitive**로 활용될 수 있음을 결론으로 제시한다.

---

# 10. Target Manuscript Title

### Option 1 — Recommended

**Antagonistic Hysteretic Silicon Devices for History-Dependent Vestibular Encoding**

### Option 2 — Device-oriented

**Hysteretic Silicon Transistors as Antagonistic State Machines for Neuromorphic Motion Encoding**

### Option 3 — Broader Nano Letters positioning

**A Hysteretic Antagonistic State Machine Using Silicon Transistors for Neuromorphic Sensory Encoding**

세 번째 제목은 novelty를 vestibular application에만 묶지 않는다는 장점이 있다.

---

# 11. Abstract에서 주장해야 할 핵심

Abstract는 다음 네 문장 구조로 압축한다.

### Problem

Existing sensory neuromorphic devices predominantly encode instantaneous stimuli through signal amplitude or spike rate.

### Device

We demonstrate a hysteretic silicon device whose latch-up and latch-down thresholds define a controllable physical memory window.

### System

By antagonistically coupling two devices, previous directional states are retained until a sufficiently strong opposite stimulus induces state reversal.

### Demonstration

Using vestibular-like rotational signals, the system achieves history-dependent directional encoding and suppresses insignificant motion perturbations without explicit digital memory.

---

# 12. Nano Letters 수준을 위해 반드시 확보해야 할 결과

단순히 실험을 많이 하는 것보다 다음 네 결과의 확보 여부가 중요하다.

## A. Reproducible experimental hysteresis

실제 device의 latch window가 반복 가능해야 한다.

## B. Controllable window

TCAD 또는 experiment를 통해

\[
\Delta V_H
\]

를 의도적으로 조절할 수 있어야 한다.

## C. Two-device antagonistic operation

RIGHT/LEFT state가 실제로 competition하고 switching하는 결과가 필요하다.

특히 Nano Letters target이라면 가능하면 simulation-only가 아닌 **experimental two-device coupling**을 확보한다.

## D. History-dependent computation

동일 현재 input에서 서로 다른 history에 따라 서로 다른 state가 나오는 결과.

이 결과가 본 논문의 가장 중요한 novelty proof가 된다.

---

# 13. Go / No-Go 기준

## Nano Letters로 그대로 진행

다음이 모두 확보될 경우:

```text
experimental single-device hysteresis
+ tunable physics
+ experimental antagonistic pair
+ clear history dependence
+ meaningful vestibular/noise benchmark
```

## Nano Letters에 도전 가능

```text
experimental single-device
+ strong TCAD
+ measured-device-calibrated two-device circuit
+ strong functional demonstration
```

이 경우 circuit이 simulation이라는 약점은 있지만 story의 novelty가 충분히 강하면 submission을 고려한다.

## IEEE TED로 방향 전환

Push-pull circuit이 simulation에만 머물고 experimental system demonstration이 어렵다면,

```text
STL physics
+ hysteresis engineering
+ neuromorphic application
```

중심으로 TED manuscript로 전환한다.

---

# 14. 연구의 최종 한 문장

> **Rather than merely encoding the instantaneous magnitude of a sensory stimulus, two antagonistically coupled hysteretic silicon devices physically retain the previous directional state and switch only when the opposing stimulus exceeds a controllable reversal threshold.**

즉 이 연구는 **사인파를 얼마나 잘 reproduce하는가**에 대한 연구가 아니다.

**현재 입력을 계산하는 device가 아니라, 과거와 현재 입력의 관계를 physical state로 판단하는 device pair를 만드는 연구**이다.

---

# 15. 가장 먼저 해야 할 실험/시뮬레이션

가장 먼저 확인해야 할 것은 **Figure 4d의 “same input, different history”** 결과이다.

이 결과가 성립하면:

1. hysteresis가 실제 memory 역할을 한다는 것을 직접 증명할 수 있고,
2. 단순 threshold detector와의 차이를 명확히 보여줄 수 있으며,
3. push-pull circuit의 목적이 분명해지고,
4. 이후 Figure 1–5 전체가 하나의 스토리로 자연스럽게 연결된다.

따라서 현재 우선순위는:

```text
1. LTspice에서 antagonistic two-device circuit 구성
2. positive / negative directional input split
3. strong/weak opposite input sequence 검증
4. same instantaneous input / different history 검증
5. state-transition boundary 추출
6. 이후 TCAD controllability와 연결
7. 마지막으로 realistic IMU/rotation waveform 적용
```
