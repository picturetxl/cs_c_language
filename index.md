## C语言 造轮子

### 计算两个日期的天数
```cpp
int day_diff(int year_start, int month_start, int day_start, int year_end, int month_end, int day_end)
{
	int y2, m2, d2;
	int y1, m1, d1;
	
	m1 = (month_start + 9) % 12;
	y1 = year_start - m1/10;
	d1 = 365*y1 + y1/4 - y1/100 + y1/400 + (m1*306 + 5)/10 + (day_start - 1);
 
	m2 = (month_end + 9) % 12;
	y2 = year_end - m2/10;
	d2 = 365*y2 + y2/4 - y2/100 + y2/400 + (m2*306 + 5)/10 + (day_end - 1);
	
	return (d2 - d1);
}

```

### 获取当前日期

```cpp
//    printf("%d\n",p->tm_sec); /*获取当前秒*/
//    printf("%d\n",p->tm_min); /*获取当前分*/
//    printf("%d\n",8+p->tm_hour);/*获取当前时,这里获取西方的时间,刚好相差八个小时*/
//    printf("%d\n",p->tm_mday);/*获取当前月份日数,范围是1-31*/
//    printf("%d\n",1+p->tm_mon);/*获取当前月份,范围是0-11,所以要加1*/
//    printf("%d\n",1900+p->tm_year);/*获取当前年份,从1900开始，所以要加1900*/
//    printf("%d\n",p->tm_yday); /*从今年1月1日算起至今的天数，范围为0-365*/

char * get_current_date()
{
    time_t timep;
    struct tm *p;
    time (&timep);
    p=gmtime(&timep);
    
    char *result=(char * )malloc(sizeof(char)*8);
    
    sprintf(result,"%d%02d%02d",p->tm_year+1900,p->tm_mon+1,p->tm_mday);
	
    return result;
}
```
### 计算一个值中值为1的位的个数
```cpp
int count_one_bits(unsigned int)
{
	int ones;
	for(ones=0;value!=0;value=value>>2)
	{
		if(value%2!=0)
		{
			ones += 1;
		}
	}
}
```

