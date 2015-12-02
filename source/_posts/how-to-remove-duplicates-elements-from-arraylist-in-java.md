title: How to remove duplicates elements from ArrayList in Java
tags:
  - Duplicate from Array
id: 210
categories:
  - Java
date: 2015-05-15 13:24:29
---

How to Remove Duplicates from Array without using Java Collection API

Read more: [http://javarevisited.blogspot.com/2014/01/how-to-remove-duplicates-from-array-java-without-collection-API.html#ixzz3aD7lSsgh](http://javarevisited.blogspot.com/2014/01/how-to-remove-duplicates-from-array-java-without-collection-API.html#ixzz3aD7lSsgh)
<pre>import java.util.Arrays;

public class RemoveDupFromArray{
    private static void main(String args[]){
**         int**[][] test = **new** **int**[][]{
            {**1**, **1**, **2**, **2**, **3**, **4**, **5**},
            {**1**, **1**, **1**, **1**, **1**, **1**, **1**},
            {**1**, **2**, **3**, **4**, **5**, **6**, **7**},
            {**1**, **2**, **1**, **1**, **1**, **1**, **1**},};

         for (int[] input : test){
             System.out.println("Array with Duplicates :"+Arrays.toString(input));
             System.out.println("Array without Duplicates :"+ Arrays.toString(removeDuplicates(input)));
         }
    }
   public static int[] removeDuplicates(int[] input){

       Arrays.sort(input);
       int[] result =  new int[input.length];
       int pre = input[0];
       result[0] = pre;
       for(int i=1;i&lt;input.length;i++){
        int ch = input[i];
        if(pre != ch){
            result[i] = ch
        }
        pre = ch;
    }
      return result;

   }
}</pre>
Use Collection API
<pre>import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedHashSet;
import java.util.List;

public class RemoveDuplicatesFromArray{

public static void main(String[] args){
List&lt;String&gt; duplicatesList = (List&lt;String&gt;) Arrays.asList("Android" , "Android", "iOS", "Windows mobile");

System.out.println("size:"+duplicatesList.size());
System.out.println("String:"+duplicatesList);

LinkedHashSet&lt;String&gt; linkHash = new LinkedHashSet&lt;String&gt;(duplicatesList);

List&lt;String&gt; removDup = new ArrayList&lt;String&gt;(linkHash);

System.out.println("size:"+removDup.size());
System.out.println("String:"+removDup);

}

}
</pre>