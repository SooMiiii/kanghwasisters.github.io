---
title: "[강화시스터즈 1기/09주차/프로젝트] 슬롯머신 코드 리뷰, 지뢰찾기 agent, env 구현"
tags: [24-1]
pages: papers
style: border  # fill / border 
color: dark
description: " "
---

# 9주차-1 세션

## 요약
저번 주에 구현한 원숭이 슬롯머신 코드를 깃허브에 올린 후 코드 리뷰를 진행했습니다.   
팀끼리 모여 지뢰찾기 agent, env를 구현했습니다. 

## 👩‍💻 코드 리뷰
### 요약
코드를 운영진이 취합해, agent 코드 구현에서 실수한 점, 잘한 점, 좋은 아이디어를 조원들과 함께 나누었습니다. 
### 깃허브 링크
Preview✨  
[https://github.com/KanghwaSisters/Slot_Machine_DQN.git](https://github.com/KanghwaSisters/Slot_Machine_DQN.git)
- `ReadMe`에 진행한 코드 리뷰 내용이 저장되어 있습니다.  

## 👩‍💻 팀 프로젝트 
- **팀구성**   

| 지뢰마스터즈 | Agent | Environment |
| --- |-----|-------------|
| 1팀 | 주민서 | 김도희         |
| 2팀 | 이정연 | 손주현         |

| AI 폭탄 제거 부대 | Agent | Environment |
| --- |-------|-------------|
| 1팀  | 변지은   | 이승연         |
| 2팀 | 이은나   | 김정은         |

### 요약
각 팀이 모여 일주일 동안 구현한 내용을 나누고, 개선점에 대해 이야기했습니다.  
가장 빠른 속도의 팀은 env-agent를 물려 train이 돌아가는 것을 확인했습니다. 

# 9주차-2 세션

## 요약 
팀끼리 모여 지뢰찾기 agent, env를 구현했습니다.  
reward에 대한 의견을 나누었습니다.  
지뢰마스터즈의 경우, 1-2팀 전부 env-agent 연결에 들어갔으며, AI 폭탄 제거 부대는 agent, env를 다듬고 있습니다. 

## 👩‍💻 팀 프로젝트 
- **팀구성**   

| 지뢰마스터즈 | Agent | Environment |
| --- |-----|-------------|
| 1팀 | 주민서 | 김도희         |
| 2팀 | 이정연 | 손주현         |

| AI 폭탄 제거 부대 | Agent | Environment |
| --- |-------|-------------|
| 1팀  | 변지은   | 이승연         |
| 2팀 | 이은나   | 김정은         |

### 팀 별 Reward
- **지뢰마스터즈-01팀** : {win : 10,  0을 눌러서 여러 개가 동시에 까지는 경우 : 2, lose : -1, 죽지 않고 진전 : 1 }  
- **지뢰마스터즈-02팀** : {win : 2,  0을 눌러서 여러 개가 동시에 까지는 경우 : 2, lose : -1, 죽지 않고 진전 : 1 }  
- **AI 폭탄 제거 부대** : {win : 100, lose : -1, 죽지 않고 진전 : 1 }  
  - **02팀** : `self.clicks` >= `n_tiles`  -> done

### 개선 방안 토의 
1. 너무 극단적인 리워드를 주면 값이 뭉개져 학습이 잘 되지 않을 것이다.   
2. win에는 큰 보상을 주는데 lose에는 전반적으로 작은 보상을 준다. 이는 지뢰를 밟는 것이 잘못된 선택인 것을 학습하지 못할 가능성이 높다. 
3. 0을 눌러서 여러 개가 까지는 상황은 플레이어 입장에서는 좋은 일이지만, 순전히 운의 영역이다. 0이 있을 법한 위치를 누르는 것은 불가능하다. 따라서 이런 식의 보상은 학습에 있어 오히려 혼란을 야기시킬 수 있기에, 진전과 동일하게 취급하는 것이 좋다. 
4. AI 폭탄 제거 부대 02팀의 경우, 한 에피소드에서 총 클릭 횟수가 게임의 타일 수와 동일해지면 보상 없이 게임을 강제로 종료시켰다. 
   - 사고 흐름 : 전체 타일 수만큼 클릭이 되었다는 거는 타일이 전부 다 까졌다는 의미다.
   1. 같은 타일을 또 누르는 경우가 있기 때문에 총 클릭 수를 기준으로 하는 건 논리적 오류다. 
   2. 지뢰를 누르지 않고 지뢰가 아닌 나머지 타일들을 전부 다 까는 것을 목표로 하기에 전체 타일 수라는 기준은 옳지 않다. 
   3. step에서 발생하기 때문에 reward 값을 갖고 있어야 하는데, 리워드를 설정하지 않았고, 지뢰찾기 특성상 이 사건에 대한 리워드를 주기가 애매하다.


