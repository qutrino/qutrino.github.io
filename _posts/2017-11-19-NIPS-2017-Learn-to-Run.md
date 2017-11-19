---
layout: post
title: "NIPS 2017:Learn to Run 참가기"
---

![test](https://dnczkxd1gcfu5.cloudfront.net/images/challenges/image_file/8/8.Screen_Shot_2017-04-10_at_1.23.56_PM.png){:height="30%" width="30%"}

*딥러닝 및 강화학습에 친해지고자 [NIPS 2017 Competition Track](https://nips.cc/Conferences/2017/CompetitionTrack) 중의 하나인  [NIPS 2017: Learn to Run](https://www.crowdai.org/challenges/nips-2017-learning-to-run)에 도전해 보았습니다. 처음 참여해본 IT분야의 competition이라 부족한 점이 많기도 하지만, 과정을 공유하여 새롭게 도전하시는 분들께 도움이 되었으면 합니다.*

# NIPS 2017:Learn to Run

### 개요
*[opensim](http://opensim.stanford.edu/)이라는 신체 시뮬레이터를 기반으로 openai gym과 유사한 강화학습 환경 osim-rl을 주최측에서 제공하고, 그 환경 내에서 강화학습을 이용, 인간신체의 근육자극을 조절하여 장애물을 넘으면서 달리게 만드는것이 목표인 대회입니다. 구체적인 환경의 정의는 아래와 같습니다.*
  - Reward : x축 방향으로 골반이 이동한 거리 - 인대 힘(ligament force)의 사용량
  - Observation: 신체 각부위의 x, y좌표 및 속도, 각속도, 다음 장애물의 위치정보: 총합 41 차원 
  - Action: 근육자극 정도. (18차원)

### 대회 기간 및 참가팀 
- 1차 라운드: 2017.06.24 ~ 2017.11.05
- 2차(최종)라운드: 2017.11.05 ~ 2017.11.13
- 참가팀: 584팀

### 기타 규칙
- 높이 0.65m 이하로 골반이 내려가면 에피소드가 끝남.
- 1000 time step(= 10초)가 지나면 에피소드가 끝남.
- 1라운드에서는 3개의 장애물로 평가하고, 3회 시도의 평균을 취하며, 매일 5번의 submission 기회가 있음.
- 2라운드에서는 10개의 장애물이 있는 환경에서 평가하고, 10회 시도의 평균을 취하며, 딱 5번의 submission 기회가 있음.
- 1라운드에서의 기록이 15m이상인 팀만이 2라운드로 진출

# 최종 참가 결과
- 1라운드: 28위 ( 3회 평균 28.14m )
- 2라운드(최종): 19위 (10회 평균 24.80m ) 
- 아래는 2라운드 5번의 submission 기회 중 첫번째 submission의 로컬 시뮬레이션 결과임. 실제로 평가된 시뮬레이션 영상들은 NIPS 2017 conference와 함께 공개될 예정이라고 함.
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<video autoplay="autoplay" loop="loop" controls="controls" width="798" height="209">
  <source src="/assets/video/nips_submit_1.mp4" type="video/mp4">
</video>

# 참가 동기 및 시기
- 딥러닝의 전반적인 내용 및 강화학습을 스터디하던 중 지인의 소개로 이 competition을 뒤늦게 알게 되어, 강화학습 분야의 깊은(?) 이해를 위하여 참여 결정.
- 1라운드 마감시한이 3~40일 정도 남은 상황에서 실질적인 참여를 시작하게 되었음.

# 초기 접근 방향

- 강화학습에 대한 교과서적인 지식밖에 없었으므로, 공식페이지의 문서 및 예제코드 습득.
- 늦은 출발을 만회하기 위하여, 공식 페이지의 discussion channel, 공식 github repo의 issue, gitter 대화내용 등은 대부분 읽음.
- 참가자 중 상위 랭커 중 한명인 [Qin Yongliang](https://github.com/ctmakro)가 자신의 모형 및 [코드](https://github.com/ctmakro/stanford-osrl)를 공개해 놓고 있는 상황이라 아주 큰 도움이 되었음. 실제 본인(나)의 구현도 Qin Yongliang의 구현에 크게 벗어나지 않고 새로운 것을 더하지 못한 상황에서 대회가 마무리 되었음.

- 그나마 이 문제와 가장 비슷하다고 할 수 있는 [mujuco](http://www.mujoco.org/)-humanoid 의 결과를 위주로 논문 서베이.
- 적용 알고리즘을 고르기 위하여 [https://gym.openai.com/envs/Humanoid-v1/](https://gym.openai.com/envs/Humanoid-v1/) 등의 공개된 정보 습득

# 문제해결
## 적용 알고리즘 결정
- continuous action에 쓰이는 RL 알고리즘 중, DDPG 알고리즘 정도까지밖에 스터디를 못한 상황이었고, 논문 서베이를 해봐도 최근의 알고리즘 A3C, TRPO 등이 DDPG알고리즘을 압도하는 상황은 아닌것으로 보여서, DDPG로 먼저 시작하기로 결정.
- [https://github.com/MorvanZhou/Reinforcement-learning-with-tensorflow](https://github.com/MorvanZhou/Reinforcement-learning-with-tensorflow)의 코드를 기본으로 하여 수정.

## 느린 시뮬레이션 환경
- 실제 세상에서의 10ms에 해당하는 1 timestep을 계산하는데, 5초가 넘게 걸릴때도 있을 정도로 느린 env 시뮬레이션 환경이기 때문에 시뮬레이션 환경의 병렬화가 꼭 필요한 상황이었음. 시뮬레이션이 항상 느린것은 아니고, 근육에 부하가 많이 걸린다거나 장애물과 컨택한다거나 하는 상황에서 많은 시간이 걸렸음.
- 이전에 잠깐 MPI(Message Passing Interface)를 사용해본적이 있고, 최근 openai baseline 코드에서도 mpi4py를 사용한 구현을 
본적이 있어서, mpi4py를 이용한 시뮬레이션 병렬화를 구현하였음.
 
	| MPI rank         | 서버    | 기능 |
	|:-------------|:------------------|:------|
	| 0            | 로컬 PC| 모델 구현 및 러닝, 다른 노드에  action값을 돌려주고 simulation 결과를 얻어서 이용함.  | 
	| 1            | 로컬 PC   | env simul. 화면에 표시되게 하여 모니터링 용도도 겸함.  |
	| 2  ~ 최대 200        | 5~8개의 aws c4.8xlarge 서버    | env simul. without visualization   |

## Preprocessing of observation
- 앞서 언급한 [Qin Yongliang](https://github.com/ctmakro)의 글과 코드를 많이 참조하였음.
- observation은 신체부위의 위치 및 속도, 각속도, 다음 장애물의 위치 등을 포함하여, 41차원의 vector.
- 장애물의 위치만 빼고 다 절대좌표이기 때문에, x방향은 골반을 중심으로 한 상대좌표로 **치환**하였고, y방향은 골반을 중심으로 한 상대좌표를 **추가**하였음.
- 속도 정보가 들어오지 않는 신체 부위는 이전 정보를 이용하여 속도 계산하여 추가하였음. (가속도는 추가하지 않음.)
- 장애물의 경우, 자기 앞에 있는 하나의 장애물 정보만 들어오기 때문에, 아직 연관이 있는 이전 장애물 정보와 새로운 장애물 정보를 둘다 유지하도록 함.
  
## Hyperparameter 및 network 수정

- actor learning rate, critic learning rate, network update rate tau
  - 병렬화를 감안하여도 시뮬레이션 환경이 빠른 편이 아니었기 때문에, 하이퍼 파라미터를 하나씩 바꿔보면서 나아지는 방형으로 조금씩 수정하였음.

- noise
  - 여러가지 시도를 해보다, Ornstein Uhlenbeck Process를 사용하게 되었고, 노이즈 크기를 바꾸는 수정을 가장 많이 하였음.

- network 
  - 간단한 network에서 점점 레이어를 늘려나감. 많은 변화를 주지는 못하였음.

## 코드
[https://github.com/qutrino/nips-2017-learn-to-run](https://github.com/qutrino/nips-2017-learn-to-run)


# 결과 제출 

- 위의 파라미터 및 네트워크 변경의 결과로 약 15,000 episode에 35~39m를 달리는 최고 기록은 나왔으나, 1/3정도는 중간에 넘어졌기 때문에 30m대의 평균기록은 얻지 못하였음. 학습시간을 더 길게 해도 기록이 나아지지 않고 나빠지는 상태가 이어짐.
- 학습 중간에 주기적으로 weight를 저장하여, 그 저장된 weight를 가지고, noise가 없는 환경에서 테스트 후 가장 좋은 결과를 준 5개를 submission하였음.(2라운드)   

# 그 밖의 시도
- batch norm. 은 시도해 보지않음. (RL에서 잘 된다는 보고가 많지 않아서.)
- layer norm. 을 적용해 보았으나, 좋은 결과를 얻지 못했음
- [Prioritized Experience Replay](https://arxiv.org/abs/1511.05952),  openai baseline code를 이용하여 적용해 보았으나, 좋은 결과를 얻지 못했음. 리플레이 메모리가 크다보니 prioritized experience replay 의 실행 시간이 너무 많이 걸림.


# 감상
*시간적으로도 여유가 많지 않은 상태였고, 병렬머신 자원을 맘편하게 쓸수 있는 환경도 아니라, 여러가지 시도를 못 해본게 아쉬움으로 남습니다. TRPO나 PPO등의 새로운 알고리즘을 사용하여 큰 변화를 주고 싶은 생각도 있었지만, 공부할 여유가 없었기도 하고, 또 수많은 하이퍼 파라미터들을 탐색할 엄두가 나지 않아 실행해보지 못하였습니다. 하지만 처음 참가한 competition 치고는 너무 많은것을 배우게 된것 같습니다.*


# 학습과정 동영상
*1 ~ 4000 에피소드까지의 학습과정입니다. 91명이 동시에 연습중인데 그 중 한명만을 보여주는 화면이라 학습이 빨라 보입니다.*

<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<video autoplay="autoplay" loop="loop" controls="controls" width="574" height="255">
  <source src="/assets/video/nips_learn.mp4" type="video/mp4">
</video>



