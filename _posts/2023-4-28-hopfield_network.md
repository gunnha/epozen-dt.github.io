---
title: "Hopfield Network"
last_modified_at: 2023-04-28
author: Gun-ha, KANG
---

> 이번 포스팅에서는 **Hopfield Network(1982')**에 대해 공유해보려고 합니다.


# **Hopfield Network**

> **개요**  

본 포스팅에서는 1982년 물리학자 존 홉필드 제안한 논문인   
"신경망과 물리 시스템에서 비롯되는 집단적 계산능력" 에서는 뉴런의 기본 속성과 단순한 지역 회로들이 어떻게 복잡한 계산 능력과 집합적 특성을 가지게 되는지를 설명합니다.


## **Method**

> **The general content-addressable memory of as physical system**

해당 논문에서는 시스템의 안정상태를 저장하여 이를 나중에 연상할 수 있는 물리 시스템을 이용한 일종의 content-addressable memory에 대해 설명합니다. 이러한 메모리 시스템은 저장된 패턴을 기반으로 주어진 쿼리를 연상할 수 있는 능력을 가집니다. 이는 주어진 내용(content)이 주소(address)의 역할을 수행한다는 의미로, 저장된 패턴을 찾아내기 위해 패턴의 내용을 이용합니다.

이 논문에서는 이러한 메모리 시스템을 뉴런들의 상태를 나타내는 확률 분포 함수로 모델링하고, 홉필드 네트워크를 사용하여 이를 구현하였습니다. 각 상태(패턴)은 게슈탈트적인 특성을 가지기에 개별적인 뉴런들의 상태가 아닌 전체 패턴을 하나의 덩어리를 봅니다.

_※게슈탈트: 예를들면 정지된 장면들을 연속적으로 제시하면 이러한 단순 조합으로는 설명할 수 없는 영화와 같은 움직임이 나타난다와 동일, "전체는 부분의 단순한 합이 아니다"_


> **The model system**

논문에서는 이진 뉴런 모델과 해당 뉴런의 기본속성에 대해서 설명합니다. 해당 모델에서는 이산적인 바이너리 값들을 다룹니다. 즉, 각 뉴런은 0 또는 1의 값을 가지며, 이 값들은 뉴런의 활동상태를 나타냅니다.

홉필드에서는 뉴런이 발화하는 것과 같이 상태를 전이시키는데 필요한 임계값을 설정해주며, 이 값은 가중치의 합이 일정 값 이상이 되어야 발화할 수 있는 값을 의미합니다.


> **The information storage algorithm** 

홉필드 네트워크를 이용하여 정보를 저장하는 방법을 설명합니다.  

* **수식 2**  
  + $$T_{i j} =\sum_s\left(2 V_i^s-1\right)\left(2 V_j^s-1\right)$$  
  + 각 뉴런 $i$와 $j$ 사이의의 연결을 나타내는 가중치 행렬 $T_{ij}$을 계산하는 식   
  + 뉴런 간의 시냅스 연결강도 $T$ 로 표현, 이때 $T$ 는 대칭행렬    
  + 뉴런 $i$와 $j$ 사이의 연결이 활성화되면 양수값, 비활성화되면 음수값 

<p>

* **수식 3**
  + $$\sum_j T_{i j} V_j^{s^{\prime}}=\sum_s\left(2 V_i^s-1\right)\left[\sum_j V_j{s^{\prime}}\left(2 V_j^s-1\right)\right]=H_j^{s^{\prime}}$$  
  + 입력 패턴 $s'$와 가중치 행렬을 이용하여 $j$번째 뉴런의 활성화 값을 계산하는 식  
  + 입력 패턴 $s'$와 저장된 정보 간의 내적 연산을 이용합니다. 내적 연산을 통해 구한 값은 $H_j^{s'}$   
  + $[\sum_j V_j^{s'}(2V_j^s-1)]$는 해당 뉴런이 입력 패턴 $s'$와 유사한 정보를 저장하고 있는 정도를 나타내는 값  
  + $H_j^{s'}$ 값이 입력 패턴 $s'$와 비슷하다면, 해당 패턴과 유사한 정보를 저장하고 있다는 것을 의미   

<p>

* **수식 4**
  + $$\sum_j T_{i j} V_j^{s^{\prime}}\equiv\left\langle H_i^{s^{\prime}}\right\rangle\approx\left(2 V_i^{s^{\prime}}-1\right) N / 2$$  
  + $H_i^{s'}$은 뉴런 $i$가 입력 패턴 $s'$와 유사한 패턴을 출력하도록 하는 값  
  + 평균 내서 $\left\langle H_i^{s^{\prime}}\right\rangle$로 나타냄  
  + $N$은 뉴런의 갯수로, 활성화된 뉴런의 경우 $(2 V_i^{s'}-1)$ 값이 큰 양수가 되며, 이 값을 $N/2$와 곱함으로써 더욱 큰 값을 얻으며, 음수인 경우에는 더욱 작은 값을 만듬


> **The biological interpretation of the model**

모델의 생물학적 해석에 대한 내용을 설명합니다. Hebbian property 는 Hebbian rule 에서 연결된 뉴런들이 동시에 활성화되면 해당 시냅스 강도가 강화되는 현상으로,  Hebbian property 가 생물학적 신경망에서도 관찰됩니다. 홉필드에서도 마찬가지로 규칙을 통해 시냅스 강도를 업데이트하고, 이를 통해 뉴런 간의 연결이 동적으로 조정됨을 말합니다.

* **수식 6**
  + $$\Delta T_{\forall}=\left[V_t(t) V_f(t)\right]{\text {average }}$$  
  + 시간$t$에서 전체 네트워크에 대한 시냅스 강도의 변화량을 나타내는 식   


> **Studies of the collective behaviors of the model** 

모델의 집합적인 행동을 연구하는 내용을 설명합니다. 특히 에너지 함수를 이용한 시스템의 상태 표현에 대한 설명을 주로 다룹니다. 

* **수식 7**
  + $$E = -1/2 \sum_{i \neq j} \sum T_{i j} V_iV_j$$    
  + 모델의 에너지 함수 식  
  + 각 뉴런간의 연결강도와 상태에 따라 에너지가 결정됨  
  + 에너지가 최소가 될 때, 네트워크가 안정상태를 띔  

<p>

* **수식 8**
  + $$\Delta E=-\Delta V_i \sum_{j \neq i^{\prime}} T_j V_j$$  
  + 뉴런의 상태 변화에 따른 에너지 변화를 나타내며, 이를 통해 이러한 상태 변화가 네트워크의 에너지에 어떤 영향을 주는지를 계산 
  + $\Delta$i, 뉴런 $i$의 상태가 변화가 일어났을 때의 그와 연결된 뉴런 $j$  


<p>


추가적으로 모델의 성능평가를 위한 지표로 사용 중인 수식들입니다.

* **수식 9**
  + $$\ln M=-\sum p_i \ln p_i$$
  + 엔트로피를 계산하는 식
  + 엔트로피란 시스템 내에서 가능한 상태들이 갖는 불확실성을 나타내는 척도  
  + $M$은 모든 가능한 상태의 수, $pi$는 i번째 상태의 확률  
  + 입력 패턴을 올바르게 분류할수록 낮은 값  

<p>

* **수식 10**
  + $$P=\frac{1}{\sqrt{2 \pi \sigma^2}} \int_{N / 2}^{\infty} e^{-x^2 / 2\sigma^2}$$
  + 정규 분포를 따르는 확률변수의 특정 범위 내의 확률을 계산하는 식  
  + 모델이 얼마나 잘 작동하는지 평가  
  + 입력패턴에 대해 잘 작동한다면, 정규분포의 확률분포를 따르는 출력 패턴이 생성됨  

<p>


뉴런 간의 연결 강도 업데이트를 위한 수식들입니다.


* **수식 11**  
  + $$\Delta T_{ij} = (2X_i-1)(2X_j-1), \quad i,j < k \leq N$$ 
  + $i,j$ 뉴런 사이의 전체의 연결 강도를 업데이트하기 위한 식  
  + $k$개의 뉴런만 사용하여 전체 네트워크의 연결 강도를 업데이트  
  + $X_i$와 $X_j$는 뉴런 $i$와 $j$의 현재 상태  
  + 뉴런 사이의 연결 강도가 서로 다르면, 갱신  

<p>

* **수식 12**
  + $$\sum_{j=1}^k c_{i j} x_j$$
  + $i$ 뉴런의 출력을 계산하기 위한 가중합을 나타내는 식
  + $c_{ij}$는 뉴런 $i$와 $j$ 사이의 가중치, $x_j$는 뉴런 $j$의 출력 상태

<p>

* **수식 13**
  + $$\Delta T_{i j}=A \sum_s\left(2 V_i^{s+1}-1\right)\left(2V_j^s-1\right)$$
  + 두 특정 $i,j$ 뉴런 사이의 연결 강도를 업데이트하기 위한 식 
  + $A$는 상수, $s$는 입력 패턴  
  + 이전 상태와 현재 상태의 차이를 이용하여 각 뉴런 사이의 연결 강도가 갱신  



## **정리 및 결론**


홉필드 네트워크는 각 뉴런이 기본속성을 가지고 있으며, 이런 작은 단위들이 모여서 집단적 계산 특성이 자발적으로 나타나는 모델로, 이러한 특성들이 생물학적 혹은 물리적 시스템에서도 발견되었고 이러한 부분이 모델링을 위한 기반이 되었다는 내용으로 이해하시면 되겠습니다.


> **참고 자료**  

* Neural Networks and Physical Systems with Emergent Collective Computational Abilities

---

<details>
  <summary><b>Contact</b></summary>

<b>Author. </b>KangGunha

<b>Email. </b>zxcvbnm9931@epozen.com

</details>