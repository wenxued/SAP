### Junit4 常用的几个annotation

`@Before`:初始化方法，每个测试都要执行一遍。

`@After`:释放资源 对于每一个测试方法都要执行一次

`@Test`：测试方法，在这里可以测试期望异常和超时时间

`@Test(expected=expected=ArithmeticException.class)`检查被测方法抛出`ArithmeticException`异常

`@Ignore`:忽略测试方法

`@BeforeClass`：针对有测试，只执行一次，且必须为`static void`

`@AfterClass`：针对有测试，只执行一次，且必须为`static void

**一个JUnit4的单元测试用例执行顺序为:**
`@BeforeClass -> @Before -> @Test -> @After -> @AfterClass; `

**每一个测试方法的调用顺序为:**
`@Before -> @Test -> @After;`



## jenkins











### 反转整数

思路：求余法

先对10取余的得到余数，将上次的数字之和乘以10之和加上余数，再除以10，重复步骤，直至为0.

**陷阱** ：所求的反转数字，可能在求解过程中已经超过了`int` 型的数据范围，导致舍弃了超出范围，剩下的范围继续相加，但是此时数字已经改变，不是我们所期待的结果。所以我们每次都要对循环中数字加以判断，一旦超出范围，返回为0.

 ` sum= sum*10 +temp;` 这句容易溢出

**第一种思路**：偷梁换柱——long类型代替int类型进行判断，最后返回值强转（long -> int）

```java

import java.util.Scanner;

public class Solution7 {

    public static void main(String[] args){
        //读取整数
        Scanner scanner =new Scanner(System.in);
        int x= scanner.nextInt();
        Solution7 solution7 =new Solution7();
        int out =solution7.reverse(x);
        System.out.println(out);
    }

    public int reverse(int x){
        int temp =0;
        long sum =0;
        while(x!=0){
            temp = x% 10;
            sum =sum *10 +temp;
            if(sum > Integer.MAX_VALUE || sum < Integer.MIN_VALUE){
                return 0;
            }
            x=x/10;
        }
        return (int) sum;
    }

}

```

`nextInt()`根据分隔符(回车，空格等)只取出输入的流中分割的第一部分并解析成`Int`。

**时间复杂度、空间复杂度都为O(1)。**

**第二种思路**：**提前**发现，提前处理，在累计反转数字的步骤前进行**提前预判**。

```java
    import java.util.Scanner;

public class Solution7 {

    public static void main(String[] args){
        //读取整数
        Scanner scanner =new Scanner(System.in);
        int x= scanner.nextInt();
        Solution7 solution7 =new Solution7();
        int out =solution7.reverse(x);
        System.out.println(out);
    }

    public int reverse(int x){
            int temp;
            int sum =0;
            while(x !=0){
                temp = x%10;
                if(sum < Integer.MIN_VALUE /10 || sum > Integer.MAX_VALUE /10 )
                    return 0;
                sum= sum*10 +temp;
                x= x/10;
            }
            return sum;
    }

}  
```

未写完 

```java
import java.lang.String;

import java.util.ArrayList;
import java.util.Scanner;
public class meiTuan3 {
    public static void main(String [] args){
        Scanner sc =new Scanner(System.in);
        String number =sc.nextLine();
        String [] numbers = number.split(" ");
        int length = numbers.length;
        int [] number1= new int[length];
        int i =0;
        for( String s :numbers){
             number1[i] =Integer.valueOf(s);
             i++;
        }
        meiTuan3 m3 = new meiTuan3();


        m3.rightPositiveLeftNegative(number1);

    }
    public  int[] rightPositiveLeftNegative(int [] number){
        int begin= 0;
        int end =number.length-1;
        int temp;
        while(begin<end){
            if(number[begin]>0)
                 begin++;
            if(number[end] <0)
                end--;
            if(number[begin] <0 && number[end]>0)
            {
                temp = number[begin];
                number[begin] =number[end];
                number[end]=temp;
                begin++;
                end--;
            }

        }
        for(int i=0;i<number.length;i++ )
            System.out.print(number[i]+" ");
     return number;
    }
}

```



























从`ArrayList`中获取数据



