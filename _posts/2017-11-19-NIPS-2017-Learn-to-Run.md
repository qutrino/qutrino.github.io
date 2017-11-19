---
layout: post
title: "NIPS 2017:Learn to Run 참가기"
---

![test](https://dnczkxd1gcfu5.cloudfront.net/images/challenges/image_file/8/8.Screen_Shot_2017-04-10_at_1.23.56_PM.png){:height="30%" width="30%"}

딥러닝 및 강화학습에 친해지고자 [NIPS 2017 Competition Track](https://nips.cc/Conferences/2017/CompetitionTrack) 중의 하나인  [NIPS 2017: Learn to Run](https://www.crowdai.org/challenges/nips-2017-learning-to-run)에 도전해 보았습니다. 처음 참여해본 IT분야의 competition이라 부족한 점이 많기도 하였지만, 여러가지 배운 점들을 공유하고자 합니다.

# NIPS 2017:Learn to Run

### 개요
[opensim](http://opensim.stanford.edu/)이라는 신체 시뮬레이터를 기반으로 openai gym과 유사한 강화학습 환경 osim-rl을 주최측에서 제공하고, 그 환경 내에서 강화학습을 이용, 인간신체의 근육자극을 조절하여 장애물을 넘으면서 달리게 만드는것이 목표인 대회입니다. 구체적인 환경의 정의는 아래와 같습니다.
  - Reward : x축 방향으로 골반이 이동한 거리 - 인대 힘(ligament force)의 사용량
  - Observation: 신체 각부위의 x, y좌표 및 속도, 각속도, 다음 장애물의 위치정보: 총합 41 차원 
  - Action: 근육자극 정도. (18차원)

### 대회 기간 및 참가팀 
- 1라운드: 2017.06.24 ~ 2017.11.05
- 2차(최종)라운: 2017.11.05 ~ 2017.11.13
- 참가팀: 584팀

### 기타
- 높이 0.65m 이하로 골반이 내려가면 에피소드가 끝남
- 1000 time step(= 10초)가 지나면 에피소드가 끝남
- 1라운드에서는 3개의 장애물, 2라운드에서는 10개의 장애물이 있는 환경에서 평가함.
- 1라운드에서 15m이상인 팀만이 2라운드로 진출

# 최종 참가 결과
- 1라운드: 28위 ( 3회 평균 28.14m )
- 2라운드(최종): 19위 (10회 평균 24.80m ) 
- 아래는 2라운드 5번의 submission 기회 중 첫번째 submission이 로컬에서의 시뮬레이션 결과임. 실제로 평가된 시뮬레이션 영상들은 NIPS 2017 conference와 함께 공개될 예정. 
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<video autoplay="autoplay" loop="loop" width="798" height="209">
  <source src="/assets/video/submit_1.mp4" type="video/mp4">
</video>

# 그 밖의 시도
- batch norm. 은 시도해 보지않음. (RL에서 잘 된다는 보고가 많지 않아서.)
- layer norm. 을 적용해 보았으나, 좋은 결과를 얻지 못했음
- [Prioritized Experience Replay](https://arxiv.org/abs/1511.05952),  openai baseline code를 이용하여 적용해 보았으나, 좋은 결과를 얻지 못했음. 리플레이 메모리가 크다보니 prioritized experience replay 의 실행 시간이 너무 많이 걸림.


# 기초공부
대회 이전에 공부해 본 부분이라 대회참여와는 상관없긴 하지만, 도움이 되실까 공유합니다.
 

# 총평


# 접근방법
 
# 워크플로우 

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page: About]({{ site.baseurl }}/about).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# [](#header-1)Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## [](#header-2)Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
