category: ����
tags: ���ѧϰ
keywords: ����
description: 
---

# CNN�����󵼼��Ƶ�

��������������CNNϵͳ��Ŀ�꺯������������$\left( {{x_i},{y_i}} \right)$��m����CNN���繲��L�㣬�м�������ɸ�������pooling�㣬���һ������Ϊ$f\left( {{x_i}} \right)$����ϵͳ��loss���ʽΪ(��Ȩֵ�����˳ͷ���һ����඼���ý�������ʽ)��

$$Loss =  - \frac{1}{m}\sum\nolimits_{i = 1}^m {y_i^'} \log f\left( {{x_i}} \right) + \lambda \sum\nolimits_{k = 1}^L {sum\left( {{{\left\| {{W_k}} \right\|}^2}} \right)}$$

## ����һ������������������

����ֻ���Ǹ�һ����������$\left( {x,y} \right)$�����Σ�loss����������Ĺ�ʽ�������ý���������ʾ�ģ���ʱ������Ȩֵ�����������ǩ����one-hot���룬CNN��������һ�����softmaxȫ����(�����ʱ�����һ����softmax)������$\left( {x,y} \right)$����CNN���������յ������$f\left( {{x}} \right)$��ʾ�����Ӧ��������lossֵΪ:
![1](/public/img/posts/CNN���򴫲�/1.png)

����$f\left( {{x}} \right)$���±�$c$�ĺ������ʽ��
$$f{\left( x \right)_c} = p\left( {y = c|x} \right)$$