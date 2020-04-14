# java中的时间

### Date        

```java
Date();//返回当前时间
Date(long millisec);//返回 距离1970/1/1的毫秒数对应的时间

Date date2=new Date();
Date date3=new Date();

date2.compareTo(date3);//比较，大于返回1，小雨返回-1，等于返回0
date2.getTime();//返回距离1970/1/1的毫秒数
date2.before(date3);//date2在date3之前，返回true
date2.after(date3);//date2在date3之后，返回true
```

### SimpleDateFormat

```java
SimpleDateFormat format = new SimpleDateFormat("yyyy:MM:dd HH");
Date date = new Date();
format.format(date);//format将Date转字符串
        
String strDate="2019:10:1 11";
Date date2=format.parse(strDate);//parse将字符串转Date类型
```

### Calendar

#### getInstance/get/set/add

```java
Calendar c1 = Calendar.getInstance();
// 获得年份
int year = c1.get(Calendar.YEAR);
// 获得月份
int month = c1.get(Calendar.MONTH) + 1;//月份默认是从0开始算
// 获得日期
int date = c1.get(Calendar.DATE);
// 获得小时
int hour = c1.get(Calendar.HOUR_OF_DAY);
// 获得分钟
int minute = c1.get(Calendar.MINUTE);
// 获得秒
int second = c1.get(Calendar.SECOND);
// 获得星期几（注意（这个与Date类是不同的）：1代表星期日、2代表星期1、3代表星期二，以此类推）
int day = c1.get(Calendar.DAY_OF_WEEK);

//把c1的对象设置成2008，
c1.set(Calendar.YEAR,2008);

//表示c1的基础上加10天，-10表示减10天,可以方便进行运算
c1.add(Calendar.DATE, 10);

//计算接下来一百天的日期
Calendar current=Calendar.getInstance();
SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy-MM-dd");

for (int i = 0; i < 100; i++) {
current.add(Calendar.DAY_OF_MONTH,1);
System.out.println(simpleDateFormat.format(current.getTime()));
}

```

#### 和Date转化

```java
/**
* 通过setTime和getTime转化
*/
Date d3=c1.getTime();
Date d4=new Date();
Calendar c4=Calendar.getInstance();
c4.setTime(d4);
System.out.println(d3.before(d4));//false
```

