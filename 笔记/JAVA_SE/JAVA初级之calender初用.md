### Uitl时间三部曲之calender初用

```
package test2;
import  java.util.*;

public class test2 {
    public static void main(String[] args) {
        Scanner scanner =new Scanner(System.in);
        System.out.println("*****************功能选择******************");
        System.out.println("************1.查询某月天数*****************");
        System.out.println("************2.查询某天周几*****************");
        System.out.println("************3.查询某俩天差*****************");
        System.out.println("******************************************");
        int function;
        boolean r=true;
        while (r) {
            System.out.print("输入(序号选择功能):");
            function = scanner.nextInt();
            switch (function) {
                case 1:
                    traversal();
                    break;
                case 2:
                    queryDay();
                    break;
                case 3:
                    queryDayToDay();
                    break;
                default:System.out.println("输入出错,请输出1,2,3范围选择功能");continue;
            }
        }
    }
```
      //第一个功能：遍历某年的某月Traversal
        public static void traversal(){
            String[] days={"周日","周一","周二","周三","周四","周五","周六"};
            int year,month;
            do{
                Scanner scanner = new Scanner(System.in);
                System.out.print("输入查询年份：");
                year = scanner.nextInt();
                System.out.print("输入查询月份：");
                month = scanner.nextInt();
            }while (panD(year,month,1)==0);
    
            Calendar calendar=Calendar.getInstance(); //获得当前系统默认日历
            calendar.clear();//此方法设置此Calendar的所有日历字段值和时间值（毫秒从历元至偏移量）未定义。
            calendar.set(Calendar.YEAR,year); //设置calendar对象的year值
            calendar.set(Calendar.MONTH,month-1);//默认1月为0月，设置calendar对象的year值  
            int day = calendar.getActualMaximum(Calendar.DAY_OF_MONTH); //当前年份的这个月天数最大值
    //        System.out.println(year+"年"+month+"月"+"的最大天数是:"+day);
            int dayOfWeek=calendar.get(Calendar.DAY_OF_WEEK);
    //        System.out.println(dayOfWeek-1);
    //        System.out.println("周日 周一 周二 周三 周四 周五 周六");
            int k=0;
            for(int i=0;i<days.length;i++){
                System.out.print(days[i]+" ");
                k+=1;
                if(k==days.length){
                    System.out.println();
                }
            }
            for(int i=0;i<(dayOfWeek-1);i++){
                System.out.print("     ");
            }
            int sum=(dayOfWeek-2);
            for (int j = 1;j<=day;j++){
    
                sum+=1;
                if(sum%7==0&&sum>5){
                    System.out.println(" ");
                }
                if (j<10){
                    System.out.print(" "+j+"   ");
                } else System.out.print(j+"   ");
            }
        }


```
    //第二个功能：查询某天星期几queryDay
    public static void queryDay(){
        String[] days={"周日","周一","周二","周三","周四","周五","周六"};
        int year,month,day;
        do {
            Scanner scanner = new Scanner(System.in);
            System.out.print("输入查询年份：");
            year = scanner.nextInt();
            System.out.print("输入查询月份：");
            month = scanner.nextInt();
            System.out.print("输入查询天数：");
            day = scanner.nextInt();
        }while (panD(year,month,day)==0);
        Calendar calendar=Calendar.getInstance(); //获得当前系统默认日历
        calendar.clear();
        calendar.set(Calendar.YEAR,year);
        calendar.set(Calendar.MONTH,month-1);
        calendar.set(Calendar.DAY_OF_MONTH,day);
        int dayOfWeek=calendar.get(Calendar.DAY_OF_WEEK);
        System.out.println(year+"年"+month+"月"+day+"日是:"+days[(dayOfWeek-1)]);
    }
```
    //第三个功能：查询某俩天相隔多少天
    public static void queryDayToDay(){
        Scanner scanner =new Scanner(System.in);
        int year1,year2,mon1,mon2,day1,day2;
        do {
            System.out.print("输入第1个年份：");
            year1 = scanner.nextInt();
            System.out.print("输入第1个月份：");
            mon1 = scanner.nextInt() - 1;
            System.out.print("输入第1个天数：");
            day1 = scanner.nextInt();
        }while (panD(year1,mon1,day1)==0);
        do {
            System.out.print("输入第2个年份：");
            year2 = scanner.nextInt();
            System.out.print("输入第2个月份：");
            mon2 = scanner.nextInt() - 1;
            System.out.print("输入第2个天数：");
            day2 = scanner.nextInt();
        }while (panD(year2,mon2,day2)==0);
        Calendar calendar1=Calendar.getInstance();
        Calendar calendar2=Calendar.getInstance();
    
        calendar1.set(year1,mon1,day1);
        calendar2.set(year2,mon2,day2);
    
        if(calendar1.after(calendar2))//如果第一个日期大于第二个，则把他们值交换，否则不处理
        {
            Calendar temp=calendar1;
            calendar1=calendar2;
            calendar2=temp;
        }
    
        int days=calendar2.get(Calendar.DAY_OF_YEAR)-calendar2.get(Calendar.DAY_OF_YEAR);
    
        int y1=calendar1.get(Calendar.YEAR);
        int y2=calendar2.get(Calendar.YEAR);
        if(y1 != y2){
            calendar1= (Calendar) calendar1.clone();
            do {
                days += calendar1.getActualMaximum(Calendar.DAY_OF_YEAR);//得到当年的实际天数
                calendar1.add(Calendar.YEAR, 1);
            }while (calendar1.get(Calendar.YEAR) != y2);
        }
    
        System.out.println("2个日期相差"+days+"天!");
    }

```
//判断输入是否合法
    public static int panD(int years,int mon,int day){
        int re=1;

        if((day>0&&day<32)&&(mon>0&&mon<13)&&(years>1970&&years<2100)){
            re=1;
        }else {
            System.out.println("输入出错,请重新输入");
            re = 0;
        }
        return re;
    }
}
```