title: java 打印素数
id: 218
categories:
  - Java
date: 2015-05-20 08:08:22
tags:
---

<pre> public static void main(String args[]){

 for(int i=2; i&lt;100; i++){

 int factor = 0;
 for(int j=1; j&lt;(i+2)/2; j++){

 if((i%j)==0){

 factor++;
 }
 }
 if(factor &lt; 2)
 System.out.println(i+" is a prime");

 }
 }</pre>