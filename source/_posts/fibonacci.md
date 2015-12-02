title: Fibonacci
id: 202
categories:
  - fibonacci
date: 2015-05-15 11:59:44
tags:
---

```
import java.util.Scanner;

/**
*@author napu.zhang
*/
public class Fibonacci{

    public static int fibonacciRecusion(int num){
        if(num == 1 || num ==2){
            return 1;
        }
        return fibonacciRecusion(num-1)+fibonacciRecusion(num-2);
    }

    public static int fibonacciLoop(int num){

        if(num == 1||num ==2){
            return 1;
        }
        int fibo1=1,fibo2=1,fibonacci=1;
        for(int i=3;i&lt;=num;i++){
            fibonacci = fibo1 + fibo2;
            fibo1 = fibo2;
            fibo2 = fibonacci;
        }
        return fibonacci;
    }

    public static void main(String args[]){
        System.out.println("Enter num")
        int number = new Scanner(System.In).nextInt();
        System.out.println("Fibonacci series upto "+number+"numbers:");
        for(int i=1;i&lt;=number; i++){
            System.out.print(fibonacciRecusion(i)+"" );
        }
    }
}
```
