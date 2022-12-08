---
published: True
title:  "[tensorflow] InvalidArgumentError:    Invalid input_h shape: [1,64,1024] [1,41,1024]"
categories: tensorflow
tag: [kera, tensorflow, Error]
---

## InvalidArgumentError

```
InvalidArgumentError:    Invalid input_h shape: [1,64,1024] [1,41,1024]
```
모델 훈련 도중 위와 같은 에러를 만났다.


```py
model = tf.keras.Sequential()
model.add(tf.keras.layers.Embedding(
    vocab_size, 
    embedding_dim,
    batch_input_shape=[batch_size, None]))
model.add(tf.keras.layers.LSTM(
    rnn_units, 
    return_sequences=True, 
    stateful=True, recurrent_initializer='glorot_uniform'))
model.add(tf.keras.layers.Dense(vocab_size))
```
모델을 구성할때 LSTM을 사용 했는데  
stateful 파라미터가 True 라면  
트레이닝 데이터가 배치 사이즈에 딱맞아 떨어져야 한다.  
맞아 떨어지지 않는다면 위의 경우처럼 에러가 난다.  

배치사이즈가 64인데 남은 트레이닝 데이터는 41이라 안맞음.

## 해결방법

1. 배치사이즈로 트레이닝 데이터를 나누었을 때 딱 맞아 떨어지게 해준다.

트레이닝 데이터 길이 % 배치 사이즈 = 0

2. drop_remainder=True

이 방법이 간단한데 배치를 만들때 아래처럼  
drop_remainder를 True 로 해주면 맞춰서 잘라준다.(남는건 버린다)

```py
train_data = train_data.batch(
    batch_size, drop_remainder=True)
```


### 참고

<https://stackoverflow.com/questions/64159777/stateful-lstm-tensorflow-invalid-input-h-shape-error>  

<https://keras.io/api/layers/recurrent_layers/lstm/>