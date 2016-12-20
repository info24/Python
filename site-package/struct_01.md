#!/usr/bin/python
# -*- coding: UTF-8 -*-
__author__ = 'Administrator'

#需要有一种机制将某些特定的结构体类型打包成二进制流的字符串然后再网络传输，
# 而接收端也应该可以通过某种机制进行解包还原出原始的结构体数据。python中的struct模块就提供了这样的机制，

#python struct的pack和unpack
#http://www.cnblogs.com/coser/archive/2011/12/17/2291160.html

import struct
import binascii
import ctypes

print "1、基本的pack和unpack"
#原始的结构体数据
values = (1, 'abc', 2.7)
s = struct.Struct('I3sf') #I 表示int，3s表示三个字符长度的字符串，f 表示 float。
packed_data = s.pack(*values)
unpacked_data = s.unpack(packed_data)

print 'Original values:', values
print 'Format string :', s.format
print 'Uses :', s.size, 'bytes'
print '打包数据Packed Value :', binascii.hexlify(packed_data)
print '解包数据Unpacked Type :', type(unpacked_data), ' Value:', unpacked_data

print "2、字节顺序"
values = (1, 'abc', 2.7)
s = struct.Struct('I3sf')
prebuffer = ctypes.create_string_buffer(s.size)
print 'Before :',binascii.hexlify(prebuffer)
s.pack_into(prebuffer,0,*values)
print 'After pack:',binascii.hexlify(prebuffer)
unpacked = s.unpack_from(prebuffer,0)
print 'After unpack:',unpacked

print "3、利用buffer，使用pack_into和unpack_from方法"


values1 = (1, 'abc', 2.7)
values2 = ('defg',101)
s1 = struct.Struct('I3sf')
s2 = struct.Struct('4sI')

prebuffer = ctypes.create_string_buffer(s1.size+s2.size)
print 'Before :',binascii.hexlify(prebuffer)
s1.pack_into(prebuffer,0,*values1)
s2.pack_into(prebuffer,s1.size,*values2)
print 'After pack:',binascii.hexlify(prebuffer)
print s1.unpack_from(prebuffer,0)
print s2.unpack_from(prebuffer,s1.size)
print(type(prebuffer))

print("4、测试其他")

values1 = (1, 'abc', 2.7)
values2 = ('defg',101)
s1 = struct.Struct('I3sf')
s2 = struct.Struct('4sI')

prebuffer = ctypes.create_string_buffer(s1.size+s2.size)
print (s1.size+s2.size)
print 'Before :',binascii.hexlify(prebuffer)
s1.pack_into(prebuffer,0,*values1)
s2.pack_into(prebuffer,s1.size,*values2)

text=binascii.hexlify(prebuffer);
print 'After pack:',text
buffer=binascii.unhexlify(text);
print s1.unpack_from(buffer,0)
print s2.unpack_from(buffer,s1.size)
