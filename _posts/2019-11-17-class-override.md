---
layout: post
title: 파이썬 클래스 오버라이딩
tags: [python, class]
---

베리오그램 분석을 통해 태양광 발전기 이용률의 공간적 상관관계를 구하기 위해 scikit-gstat 모듈을 사용했다.

Variogram이랑 OrdinaryKriging 클래스는 정말 상관관계를 분석하기 쉽게 만들어져있었지만

내가 사용하는 태양광 데이터 사이즈가 워낙 크다보니 크리깅 속도가 너무 느려짐

무엇보다 1년 8760시간에 해당하는 데이터를 전부 크리깅을 통해 구하려고 하니 정신이 아찔해짐

그래서 대표일 하나 골라서 분석하고, 가중치만 뽑아내고 싶었는데 가중치만 반환하는 함수가 없더라

결국 말로만 듣던 클래스 오버라이딩을 하게 되었다.

```python
from skgstat import OrdinaryKriging

class NewKriging(OrdinaryKriging):
    
    def _krige(self, p):
    
        '''생략'''
        Z = I[:-1].dot(values)
        
        # return
        self.weights = I[:-1]
        self.location = in_range
        
        return Z, sigma     
```

원래 코드가 너무 길어서 생략 했지만, 원래 있던 행렬 I에서 가중치에 해당하는 부분만 떼와서

weights로 저장했다. 부를 때는 아래처럼 불러오도록 했다.

<pre>
<code>ok = NewKriging(Variogram_obj, min_point, max_point, mode)
target_weight = ok.weights </code>
</pre>

나는 그냥 필요한 부분만 고쳐쓰는 수준으로 했는데도 코드 뜯어보느라 꽤 고생했다.

좀 더 능숙해지면 쉽게쉽게 할 수 있겠지