---
layout: post
title: ����caffe��lmdb��дͼ������
category: ����
tags: ���ѧϰ
keywords: source code
description: 
---

# ���

lmdb��һ�������������ݿ⣬caffe����Ҫ����ʹ��lmdbģ��������ͼ�����ݼ��ı��档��˵����Ϊlmdb�ж�ȡ�ٶȿ죬֧�ֶ��̡߳�����̲������������������ŵ㣬ע�⵽������ݿ���ʵ��û���κ�ѹ����������ã�����Ŀ��ֻ��Ϊ�˿��ٵ������ʹ�ȡ���������ݶ������һ�������ݽṹ�Ӷ�ʹ�����΢����

��ʵ�����������lmdb���÷����޷�ֱ��Ӧ����ͼ���ļ��Ĵ���ġ�����caffe�ǽ�ͼ�������Դ����������͵���ʽ����lmdb�еģ�������Ǳ�����caffe���������Ͳ�����ɶ�ȡ��ʹ�á�

# ���������ļ�

```Python
#coding:utf-8
import lmdb
import numpy as np
import cv2
import caffe
from caffe.proto import caffe_pb2 
 
#basic setting
lmdb_file = 'lmdb_data'#�������ɵ������ļ�
batch_size = 200       #lmdb�������ݽ��е����Ȼ����һ����д��Ӷ����Ч�ʣ���˶���һ��batch_size����ÿ��д�������
 
# create the leveldb file
lmdb_env = lmdb.open(lmdb_file, map_size=int(1e12))#����һ�������ļ����������ռ�
lmdb_txn = lmdb_env.begin(write=True)              #�����ݿ�ľ��
datum = caffe_pb2.Datum()                          #����caffe�ж������ݵ���Ҫ����
 
for x in range(1000):
    x+=1
    img=cv2.imread('A/'+str(x)+'.png').convert('RGB')     #��A/�ļ��������ζ�ȡͼ��
 
    # save in datum
    data = img.astype('int').transpose(2,0,1)      #ͼ�����ע����Ҫ����ά��
    #data = np.array([img.convert('L').astype('int')]) #������������ά��
    label = x                                      #ͼ��ı�ǩ��Ϊ�˷���洢�����������������
    datum = caffe.io.array_to_datum(data, label)   #�������Լ���ǩ����Ϊһ��������
 
    keystr = '{:0>8d}'.format(x-1)                 #lmdb��ÿһ�����ݶ����ɼ�ֵ�Թ��ɵģ��������һ���õ���˳�����еĶ���Ψһ��key
    lmdb_txn.put( keystr, datum.SerializeToString())#���þ����д���ڴ档
 
    # write batch
    if x % batch_size == 0:                        #ÿ���ۼƵ�һ����������������commit����д��Ӳ�̡�
        lmdb_txn.commit()
        lmdb_txn = lmdb_env.begin(write=True)      #commit֮��֮ǰ��txn�Ͳ������ˣ��������¿�һ����
        print 'batch {} writen'.format(x)
 
lmdb_env.close()                                   #�������ס�ͷ���Դ�������´��õ�ʱ��򲻿���
```

# ��ȡ�����ļ�

```Python
#coding:utf-8
 
import caffe
import lmdb
import numpy as np
import cv2
from caffe.proto import caffe_pb2
 
lmdb_env = lmdb.open('lmdb_data')#�������ļ�
lmdb_txn = lmdb_env.begin()      #���ɾ��
lmdb_cursor = lmdb_txn.cursor()  #���ɵ�����ָ��
datum = caffe_pb2.Datum()        #caffe�������������
 
for key, value in lmdb_cursor:   #ѭ����ȡ����
    datum.ParseFromString(value) #��value�ж�ȡdatum����
 
    label = datum.label          #��ȡ��ǩ�Լ�ͼ������
    data = caffe.io.datum_to_array(datum)
    print data.shape
    print datum.channels
    image =data.transpose(1,2,0)
    cv2.imshow('cv2.png', image) #��ʾ
    cv2.waitKey(0)
 
cv2.destroyAllWindows()
lmdb_env.close()
```