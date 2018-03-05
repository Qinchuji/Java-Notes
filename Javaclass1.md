```java
import java.util.Scanner;
public class BaseSort {     
  public void sort(int[] array){         
    System.out.println("排序算法");     
  }
}
//选择排序
public class SelectSort extends BaseSort{
  @Override
  public void sort(int[] array){
      for (int i = 0; i < array.length - 1; i++)    
      {
         int k = i;
         for (int j = i + 1; j < array.length; j++) {    
             if (array[j] < array[i])
                 i = j;
         }
      if (k != i) {
          int temp = array[i];
          array[i] = array[k];
          array[k] = temp;
      }
      System.out.println("第" + (i + 1) + "趟排序");
      for (int m = 0; m < array.length; m++) {
          System.out.print(array[m] + " ");
      }
      System.out.println();
      }
   }
}
//插入排序
public class InsertSort extends BaseSort{
  @Override
  public void sort(int[] array)
  for (int i = 1; i < iDataNum; i++)  
    {  
        int j = i - 1;  
        int temp = pDataArray[i];
        while (j >= 0 && pDataArray[j] > temp)
        {  
            pDataArray[j+1] = pDataArray[j];  
            j--;  
        }  

        if (j != i - 1)
            pDataArray[j+1] = temp;
    }
    System.out.print("插入排序结果为：");
        for (int num:
             array) {
            System.out.println(num + " ");
        }

}
//快速排序
public class QuickSort extends BaseSort{

  @Override
  public void sort ( int[] array){
  super.sort(array);

  sort(array, 0, array.length-1);
  System.out.print("插入排序结果为：");
      for (int num:
           array) {
          System.out.println(num + " ");
      }
}


  int sort(int[] array, int left, int right){
      if(left < right){
          int key = array[left];
          int low = left;
          int high = right;
          while(low < high){
              while(low < high && array[high] > key){
                  high--;
              }
              array[low] = array[high];
              while(low < high && array[low] < key){
                  low++;
              }
              array[high] = array[low];
          }
          array[low] = key;
          sort(array,left,low-1);
          sort(array,low+1,right);
      }
  }
}
//策略类
public class Factory{     
  private BaseSort sort;     
  //依赖注入     
  public void setSort(BaseSort sort){         
    this.sort = sort;    
  }     
public void doSort(int []a){         
  sort.sort(a);     
  }

}

public class Test{     
  public static void main(String[] args) {     
    Scanner input = new Scanner(System.in);
    int[] a = new int[10];
      //自己用键盘输入 int a[10]
    for (int i = 0:
         10) {
           a[i] = input.nextInt();
    }
     
    Factory factory = new Factory();
         
    BaseSort select_sort = new SelectSort();     
    factory.setSort(select_sort);     
    factory.doSort(array)
    BaseSort insert_sort = new InsertSort();     
    factory.setSort(insert_sort);     
    factory.doSort(array)
    BaseSort quick_sort = new QuickSort();     
    factory.setSort(quick_sort);     
    factory.doSort(array)          
  }
}
```
