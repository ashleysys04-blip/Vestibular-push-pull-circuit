# Nano Letters Target Research Plan  
## Hysteretic Push–Pull Silicon Devices for History-Dependent Vestibular Encoding

> **Target journal:** *Nano Letters* (stretch goal)  
> **Fallback:** *IEEE Transactions on Electron Devices*  
> **Core direction:** 기존 LTspice push–pull 구조는 유지하고, STL hysteresis가 만들어내는 **history-dependent directional state**를 핵심 novelty로 확장한다.

---

## 1. 연구 개요

이 연구는 지금까지 만든 **두 개의 hysteretic silicon device 기반 push–pull vestibular circuit**을 크게 바꾸는 것이 아니다.

기존 구조의 핵심은 그대로 유지한다.

- 하나의 vestibular input을 두 반대 방향 채널이 받음
- Right channel과 Left channel이 반대 방향으로 반응
- 두 채널이 서로 영향을 주는 push–pull / antagonistic 구조
- STL의 latch 및 hysteresis 특성을 이용
- sinusoidal input을 포함한 시간 변화 신호를 LTspice에서 처리

이번에 추가되는 핵심은 **출력을 단순 waveform reproduction으로 해석하지 않고, hysteresis에 의해 이전 방향이 유지되는 stateful encoding으로 해석하는 것**이다.

즉,

$$
\text{Current output} = F\!\left(\text{current input},\ \text{previous device state}\right)
$$

를 device physics 자체에서 구현하는 것이 목표다.

---

## 2. 기존 LTspice 구조와 이번 연구 방향의 관계

### 그대로 유지되는 것

기존 push–pull 구조:

$$
x(t) > 0 \quad\Rightarrow\quad \text{Right channel dominant}
$$

$$
x(t) < 0 \quad\Rightarrow\quad \text{Left channel dominant}
$$

두 채널은 독립적인 half-wave detector가 아니라, **반대 방향으로 반응하면서 서로의 상태에 영향을 주는 antagonistic pair**로 본다.

개념적으로는 다음과 같이 표현할 수 있다.

$$
I_R^{\mathrm{eff}} = I_0 + kx(t) - gY_L
$$

$$
I_L^{\mathrm{eff}} = I_0 - kx(t) - gY_R
$$

여기서

- $x(t)$: vestibular input
- $I_0$: operating bias
- $k$: input coupling strength
- $g$: mutual inhibition / antagonistic coupling strength
- $Y_R$, $Y_L$: Right/Left device의 현재 state 또는 output

이 식은 **개념 모델**이며, 실제 LTspice 회로의 정확한 구현식과 반드시 동일할 필요는 없다.

### 새로 강조하는 것

기존에는 주로 다음을 보았다.

$$
\text{input waveform} \rightarrow \text{push–pull response / spikes}
$$

이번 논문에서는 여기에 hysteresis를 이용한 state retention을 추가한다.

$$
\text{strong Right input} \rightarrow R\text{-dominant state}
$$

이후 input이 약해지거나 약한 Left input이 들어와도 상태가 바로 뒤집히지 않고,

$$
|x_{\mathrm{opposite}}| > x_{\mathrm{reversal}}
$$

일 때만 반대 state로 전환되는지를 확인한다.

따라서 **회로를 새로 만드는 것이 아니라, 지금까지 만든 push–pull 회로에서 hysteresis가 어떤 computation을 만들어내는지 확장해서 보여주는 것**이 핵심이다.

---

## 3. 핵심 연구 질문

> **Can intrinsic hysteresis in antagonistically coupled silicon devices provide history-dependent vestibular encoding without an explicit digital memory element?**

핵심 비교는 다음과 같다.

### Memoryless encoding

$$
Y(t)=f\!\left(x(t)\right)
$$

### Proposed hysteretic encoding

$$
Y(t)=f\!\left(x(t),S(t-1)\right)
$$

$$
S(t)=g\!\left(x(t),S(t-1)\right)
$$

즉 동일한 순간 입력이라도 이전 motion history에 따라 출력이 달라질 수 있어야 한다.

---

## 4. 가장 중요한 novelty

### 4.1 Hysteresis window를 computational parameter로 사용

STL의 latch-up 및 latch-down threshold를

$$
V_{\mathrm{LU}},\qquad V_{\mathrm{LD}}
$$

라고 하면 hysteresis window는

$$
\Delta V_H = V_{\mathrm{LU}}-V_{\mathrm{LD}}
$$

로 정의할 수 있다.

이 연구에서는 $\Delta V_H$를 단순 device parameter가 아니라,

> **이전 directional state를 뒤집기 위해 필요한 반대 방향 stimulus의 크기**

로 해석한다.

### 4.2 Push–pull 구조에 intrinsic memory 추가

기존 push–pull 구조가 현재 입력 방향을 나타낸다면, hysteresis를 통해 다음과 같은 상태 유지가 가능해진다.

```text
Strong Right
    ↓
R-dominant state
    ↓
Input ≈ 0
    ↓
R state retained
    ↓
Weak Left
    ↓
R state still retained
    ↓
Strong Left
    ↓
L-dominant state
```

### 4.3 Same input, different history

가장 중요한 demonstration은 다음이다.

#### Case A

$$
\text{Strong Right} \rightarrow x(t_0)
$$

#### Case B

$$
\text{Strong Left} \rightarrow x(t_0)
$$

마지막 순간 입력은 동일하다.

$$
x_A(t_0)=x_B(t_0)
$$

하지만 device state가 다르기 때문에

$$
Y_A(t_0)\neq Y_B(t_0)
$$

가 나오는지를 확인한다.

이 결과가 나오면 단순 threshold detector나 instantaneous function fitting과 명확하게 구분된다.

---

## 5. 역할 분담: Measurement / TCAD / LTspice

### Measurement

실제 device에서 확보할 것:

- $V_{\mathrm{LU}}$
- $V_{\mathrm{LD}}$
- hysteresis window
- 반복성
- transient state retention
- 가능하면 input/current dependence
- 가능하면 two-device experimental coupling

### TCAD

TCAD의 역할은 **왜 latch가 발생하는지 + window를 어떻게 조절할 수 있는지**를 설명하는 것이다.

주요 분석:

- impact ionization
- hole accumulation
- body potential
- energy band / barrier modulation
- electric field
- latch-up / latch-down 내부 상태 비교
- gate bias, geometry, BOX/body condition 등에 따른 $V_{\mathrm{LU}}$, $V_{\mathrm{LD}}$ 변화

### LTspice

LTspice의 역할은 **현재 만든 push–pull architecture의 computation을 검증하는 것**이다.

우선순위:

1. 기존 sinusoidal push–pull 동작 정리
2. strong/weak opposite stimulus 입력
3. state retention 확인
4. same input / different history 확인
5. noise가 있는 입력에서 false reversal 확인
6. realistic vestibular waveform 적용

---

# 6. Main Figure Plan

Nano Letters를 목표로 **5개의 main figure**로 구성한다.  
Figure 구성은 논문의 claim이 한 방향으로 이어지도록 설계한다.

---

## Figure 1. Concept and Existing Push–Pull Architecture

### 목적

논문의 전체 아이디어와 **현재 LTspice에서 이미 구현한 구조**를 한 번에 보여준다.

### Fig. 1a — Biological inspiration

간단한 bilateral vestibular schematic.

포함할 것:

- head rotation
- Right vestibular pathway
- Left vestibular pathway
- 한 방향의 rotation에서 한쪽 activity 증가 / 반대쪽 감소
- “push–pull organization”만 강조

주의:

- 실제 vestibular physiology를 완전히 재현한다고 주장하지 않음
- **bio-inspired antagonistic organization** 수준으로 제한

### Fig. 1b — Single-device hysteresis

실제 STL characteristic에서

- $V_{\mathrm{LU}}$
- $V_{\mathrm{LD}}$
- ON/LRS state
- OFF/HRS state
- $\Delta V_H$

를 한 그림에 표시한다.

중앙 hysteresis region에는

> **state retention / reversal window**

라는 interpretation을 추가한다.

### Fig. 1c — 현재 push–pull LTspice architecture

**지금까지 만든 실제 회로 구조를 그대로 사용한다.**

보여줄 요소:

- vestibular input
- Right channel
- Left channel
- 두 채널의 opposite response
- mutual coupling / inhibition
- 각 channel의 hysteretic device

여기서 새로운 state-machine 회로를 따로 만들지 않는다.

### Fig. 1d — Proposed operating principle

단순한 waveform 예시:

```text
Input:
0 → Right → 0 → weak Left → strong Left

State:
N → R → R → R → L
```

핵심 메시지:

> **The existing push–pull circuit becomes history-dependent because each branch possesses intrinsic hysteresis.**

---

## Figure 2. Experimental Hysteresis and State Retention

### 목적

push–pull system의 memory가 회로에 억지로 추가된 것이 아니라 **실제 device property에서 시작된다는 것**을 증명한다.

### Fig. 2a — Device structure

- device schematic 또는 cross section
- 사용 가능한 경우 SEM/TEM
- gate/source/drain/body 영역 표시

### Fig. 2b — Full latch characteristic

한 번의 up/down sweep에서

- latch-up
- latch-down
- $V_{\mathrm{LU}}$
- $V_{\mathrm{LD}}$

를 표시한다.

### Fig. 2c — Reproducibility

여러 cycle 또는 여러 device에서 hysteresis를 비교한다.

가능하면 다음 중 하나를 제시한다.

- overlaid curves
- $V_{\mathrm{LU}}$ distribution
- $V_{\mathrm{LD}}$ distribution
- $\Delta V_H$ distribution

### Fig. 2d — State retention under reduced input

예:

$$
V_{\mathrm{in}}: 0 \rightarrow V_{\mathrm{high}} \rightarrow V_{\mathrm{mid}}
$$

$V_{\mathrm{mid}}$가 latch-up threshold보다 낮더라도 device가 이전 state를 유지하는 것을 보여준다.

이 결과가 push–pull system의 history dependence와 직접 연결된다.

### Fig. 2e — Reversal threshold

반대 방향 또는 reset stimulus의 amplitude를 변화시키며

- retain
- reset / switch

경계를 추출한다.

핵심 메시지:

> **The device physically retains a state over a finite input window and requires a finite reversal stimulus to erase it.**

---

## Figure 3. Device Physics and Hysteresis Engineering by TCAD

### 목적

hysteresis window가 우연한 현상이 아니라 **설명 가능하고 조절 가능한 physical parameter**임을 보여준다.

### Fig. 3a — Before latch-up

예:

- impact ionization rate
- hole concentration
- body potential

중 가장 mechanism을 잘 보여주는 quantity.

### Fig. 3b — After latch-up

Fig. 3a와 동일 quantity를 사용하여 직접 비교한다.

보여줄 mechanism:

```text
Impact ionization
      ↓
Hole accumulation
      ↓
Body-potential increase
      ↓
Barrier modulation
      ↓
Positive feedback
      ↓
Latch
```

### Fig. 3c — Latch-down state

input 감소 시 positive feedback이 약해지고 원래 state로 복귀하는 과정을 보여준다.

### Fig. 3d — Threshold controllability

변수 하나 또는 두 개를 선정한다.

후보:

- gate bias
- body condition
- BOX thickness
- channel/body geometry
- input current

plot:

$$
V_{\mathrm{LU}}(p),\qquad V_{\mathrm{LD}}(p)
$$

### Fig. 3e — Hysteresis-window map

가장 좋은 형태는

$$
\Delta V_H=f(p_1,p_2)
$$

의 2D map 또는 parameter sweep.

이 결과를 circuit language로 다시 해석한다.

$$
\Delta V_H \uparrow \quad\Rightarrow\quad \text{stronger state persistence / larger reversal threshold}
$$

핵심 메시지:

> **The memory strength of the push–pull system can be engineered at the device level.**

---

## Figure 4. Push–Pull Dynamics and History-Dependent Computation

### 목적

**현재까지 LTspice로 만든 push–pull 회로가 논문의 핵심 computation을 실제로 수행함을 보여주는 figure.**

새로운 회로를 만드는 figure가 아니다.

### Fig. 4a — Full LTspice circuit

실제 사용한 circuit schematic.

표시:

- input node
- Right branch
- Left branch
- coupling/inhibition path
- device model
- output definition

### Fig. 4b — 기존 sinusoidal push–pull result

지금까지 해온 결과를 baseline으로 사용한다.

동시에 표시:

- $x(t)$
- Right output
- Left output
- 필요하면 final differential output

확인할 것:

$$
x(t)>0 \Rightarrow R>L
$$

$$
x(t)<0 \Rightarrow L>R
$$

즉 기존 연구를 버리지 않고 **정상적인 antagonistic response의 baseline**으로 사용한다.

### Fig. 4c — Strong / weak reversal sequence

입력:

```text
0
→ strong Right
→ 0
→ weak Left
→ 0
→ strong Left
```

관찰:

```text
N
→ R
→ R
→ R
→ R
→ L
```

여기서 핵심은 weak opposite input에서는 state가 뒤집히지 않는 것이다.

### Fig. 4d — Same instantaneous input, different history

가장 중요한 panel.

#### Sequence A

```text
strong Right
→ common test input
```

#### Sequence B

```text
strong Left
→ common test input
```

test input은 완전히 동일하게 설정한다.

$$
x_A(t_0)=x_B(t_0)
$$

그런데 output은

$$
R_A(t_0)-L_A(t_0) \neq R_B(t_0)-L_B(t_0)
$$

가 되는지를 확인한다.

이 결과를 본 논문의 **history dependence proof**로 사용한다.

### Fig. 4e — State-transition diagram

x축 예시:

$$
|x_{\mathrm{opposite}}|
$$

y축 예시:

- pulse width
- previous-state strength
- coupling strength $g$

output:

- R retained
- L retained
- state reversal

영역을 phase map으로 표시한다.

가능하면 이 transition boundary를 Figure 3의 $\Delta V_H$와 연결한다.

---

## Figure 5. Vestibular-Like Motion Demonstration and Benchmark

### 목적

Fig. 4의 computation이 단순히 synthetic pulse에서만 성립하는 것이 아니라 **실제 motion-like input에서 기능적 의미가 있음을 보여준다.**

### Fig. 5a — Motion input

우선순위:

1. commercial IMU / gyroscope의 실제 rotation waveform
2. motorized rotation stage에서 얻은 angular velocity
3. realistic synthetic waveform

TENG는 main novelty로 사용하지 않는다.

입력에는 다음을 포함한다.

- clockwise motion
- counterclockwise motion
- direction reversal
- zero-crossing noise
- small vibration

### Fig. 5b — Push–pull channel outputs

동시에 표시:

- angular velocity input
- Right channel
- Left channel
- final dominant state

### Fig. 5c — Noise / small-motion rejection

zero 근처에서 noise를 추가한다.

비교:

**memoryless threshold**

```text
R ↔ L ↔ R ↔ L
```

**hysteretic push–pull**

```text
R → R → R → R
```

처럼 false state transition이 감소하는지를 보여준다.

### Fig. 5d — Meaningful reversal

반대로 충분히 큰 실제 direction reversal에서는 state가 제대로 전환되어야 한다.

즉 hysteresis가 단순히 output을 고정시키는 것이 아니라,

> **small perturbation은 무시하지만 meaningful reversal은 검출**

한다는 것을 보여준다.

### Fig. 5e — Quantitative benchmark

2–3개의 metric만 선정한다.

추천:

- false direction-transition rate
- minimum effective reversal amplitude
- reversal detection accuracy
- switching latency
- noise tolerance

가능하다면 추가:

- energy per transition

핵심 메시지:

> **Intrinsic hysteresis stabilizes directional encoding against insignificant perturbations while preserving real direction reversals.**

---

# 7. Double-Latch의 위치

Double latch는 현재 main storyline과 분리한다.

실제 device에서 안정적으로 재현되기 전까지는 Supporting Information 또는 outlook으로 둔다.

가능한 extension:

```text
Strong Left
Weak Left
Neutral
Weak Right
Strong Right
```

즉 binary directional state를 multi-level directional state로 확장하는 방향이다.

main claim은 우선 **single hysteresis + push–pull + history dependence**로 유지한다.

---

# 8. Supporting Information 후보

- Figure S1. Detailed device structure / fabrication
- Figure S2. Additional latch sweeps
- Figure S3. Sweep-rate dependence
- Figure S4. Device-to-device variation
- Figure S5. Repeated switching / endurance
- Figure S6. Temperature dependence
- Figure S7. TCAD mesh and model parameters
- Figure S8. Additional carrier / field distributions
- Figure S9. Additional $V_{\mathrm{LU}}$ / $V_{\mathrm{LD}}$ parameter sweeps
- Figure S10. LTspice device model
- Figure S11. Coupling-strength dependence
- Figure S12. Additional history sequences
- Figure S13. Additional noisy motion inputs
- Figure S14. Double-latch multilevel extension

---

# 9. Nano Letters용 핵심 claim

> **Two antagonistically coupled hysteretic silicon devices retain the previous directional state and reverse only when the opposing stimulus exceeds a device-defined threshold, enabling history-dependent vestibular encoding without explicit digital memory.**

---

# 10. 연구 우선순위

현재 가장 먼저 해야 할 일은 **기존 LTspice push–pull 회로를 유지한 채 Fig. 4c와 Fig. 4d가 성립하는지 확인하는 것**이다.

```text
1. 기존 sinusoidal push–pull result 정리
2. strong Right → weak Left → strong Left sequence 적용
3. weak opposite input에서 state retention 확인
4. same input / different history test
5. reversal threshold sweep
6. hysteresis window와 reversal threshold 연결
7. noisy vestibular input 적용
8. IMU / realistic motion signal 적용
```

이 결과가 성립하면 지금까지 만든 회로를 버릴 필요 없이, 기존 결과를 **stateful hysteretic push–pull computation**으로 자연스럽게 확장할 수 있다.
