---
layout: post
title: "KISTI cpu 클러스터에 keras + mpi4py 설치"
---

* KISTI cpu 클러스터에 keras ( theano backend ) 와 mpi4py 설치방법입니다. 보안상이유로 자세한 버전등은 xxx로 표시하였습니다. 서버의 OS/커널버전/glibc 버전이 낮아, 로컬에서 glibc 버전업을해서 pytorch 등을 설치해보긴 하였는데, mpi4py와 동시에 설치하기가 쉽지 않아서 keras (theano backend)를 사용하기로 하였습니다. 


## Remote 클러스터의 .bashrc 편집
다음 라인 추가. 가능한한 높은 버전 선택하였음.
{% highlight bash %}
module load compiler/gcc-XXX mpi/mvapich2-XXX applic/python-3.6
{% endhighlight %}



## Local PC에서 python 패키지 다운로드 후 전송
conda 등으로 리모트 컴퓨터와 유사한 환경구성
{% highlight bash %}
conda create --name test36 python=3.6
source activate test36
{% endhighlight %}

pip download 명령으로 원하는 패키지 저장
{% highlight bash %}
(test36) pip download mpi4py -d ./mpi4py
(test36) pip download Theano -d ./theano
(test36) pip download kears -d ./keras
{% endhighlight %}

Remote 컴퓨터 (클러스터)로 복사
{% highlight bash %}
scp -r ./mpipy ./theano ./keras XX@YY:~/ZZZ
{% endhighlight %}

## Remote 클러스터에 패키지 설치 
다운 받은 각 디렉토리로 들어가서 패키지를 개인계정에 설치
{% highlight bash %}
pip install --user mpi4py-3.0.0.tar.gz  -f ./ --no-index
pip install --user Theano-1.0.2.tar.gz  -f ./ --no-index
pip install --user Keras-2.2.0-py2.py3-none-any.whl  -f ./ --no-index
{% endhighlight %}

## keras 설정 수정
~/.keras/keras.json 파일 수정하여 theano backend 이용.
{% highlight javascript  %}
{
    "floatx": "float32",
    "epsilon": 1e-07,
    "backend": "theano",
    "image_data_format": "channels_last"
} 
{% endhighlight %}


