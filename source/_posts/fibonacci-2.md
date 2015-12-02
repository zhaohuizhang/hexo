title: Fibonacci
tags:
  - java
id: 237
categories:
  - Java
date: 2015-06-15 08:55:39
---

<pre>public class Fibonacci{

 public static int fibonacci(int n){

 if(n==1 || n==2){

 return 1;
 }
 return fibonacci(n-1)+fibonacci(n-2);
 }
 public static int fibonacci2(int n){

 if(n==1 || n==2){

 return 1;
 }

 int n1 = 1, n2 = 1, fibo = 1;
 for(int i=3; i&lt;=n; i++){

 fibo = n1 + n2;
 n1 = n2;
 n2 = fibo;

 }
 return fibo;
 }

 public static void main(String args[]){

 for(int i=1; i&lt;=10; i++){
 System.out.print(Fibonacci.fibonacci2(i) +" ");
 }

 }
}</pre>