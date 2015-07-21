title: "OC数组排序"
date: 2015-07-21 13:47:56
tags: 数组排序
---

> 大体上，OC中常用的数组排序有以下几种方法：
> sortedArrayUsingSelector:   
> sortedArrayUsingComparator:   
> sortedArrayUsingDescriptors:

###简单排序（sortedArrayUsingSelector:）

1. 简单排序

	如果只是对字符串的排序，可以利用sortedArrayUsingSelector:方法就可以了，代码如下：
	
	```
	//简单排序
	void sortArray1(){
    NSArray *array = [NSArray arrayWithObjects:@"abc",@"456",@"123",@"789",@"ef", nil];
    NSArray *sortedArray = [array sortedArrayUsingSelector:@selector(compare:)];
    NSLog(@"排序后:%@",sortedArray);
	}
	```
	除了字符串自带的compare:方法，也可以自己写compare:方法，进行对象的比较；如下：
	   
	首先是新建了Person类，实现方法如下：
	
	```
	#import "Person.h"
	@implementation Person
	//直接实现静态方法，获取带有name和age的Person对象
	+(Person *)personWithAge:(int) age withName:(NSString *)name{
    Person *person = [[Person alloc] init];
    person.age = age;
    person.name = name;
    return person;
	}
 
	//自定义排序方法
	-(NSComparisonResult)comparePerson:(Person *)person{
  //默认按年龄排序
    NSComparisonResult result = [[NSNumber numberWithInt:person.age] compare:[NSNumber numberWithInt:self.age]];//注意:基本数据类型要进行数据转换
  //如果年龄一样，就按照名字排序
    if (result == NSOrderedSame) {
        result = [self.name compare:person.name];
    }
    return result;
	}
	@end
	```
	主函数代码如下：
	```
void sortArray2(){
    Person *p1 = [Person personWithAge:23 withName:@"zhangsan"];
    Person *p2 = [Person personWithAge:21 withName:@"lisi"];
    Person *p3 = [Person personWithAge:24 withName:@"wangwu"];
    Person *p4 = [Person personWithAge:24 withName:@"liwu"];
    Person *p5 = [Person personWithAge:20 withName:@"liwu"];
    NSArray *array = [NSArray arrayWithObjects:p1,p2,p3,p4,p5, nil];
    NSArray *sortedArray = [array sortedArrayUsingSelector:@selector(comparePerson:)];
    NSLog(@"排序后:%@",sortedArray);
}
	```
2. 利用block语法（sortedArrayUsingComparator:）

	苹果官方提供了block语法，比较方便。其中数组排序可以用sortedArrayUsingComparator:方法，代码如下：
	
	```
void sortArray3(){
    NSArray *array = [NSArray arrayWithObjects:@"1bc",@"4b6",@"123",@"789",@"3ef", nil];
    NSArray *sortedArray = [array sortedArrayUsingComparator:^NSComparisonResult(id obj1, id obj2) {
 
   //这里的代码可以参照上面compare:默认的排序方法，也可以把自定义的方法写在这里，给对象排序
        NSComparisonResult result = [obj1 compare:obj2];
        return result;
    }];
    NSLog(@"排序后:%@",sortedArray);
}
	```
3. 高级排序（sortedArrayUsingDescriptors:）

	Person类里面有另外一个类的变量，比如说Person类除了name，age变量，还有一辆车Car类型，Car类里面有个name属性。对Person对象进行排序，有这样的要求：按照Car的name排序，如果是同一辆车，也就是Car的name相同，name再按照年龄进行排序，如果年龄也相同，最后按照Person的name进行排序。
	
	上面这样就要使用第三种方法，利用排序描述其，不过说，看API介绍。如下：
	首先写个Car类，实现类Car.m代码如下：
	
	```
#import "Car.h"
@implementation Car
 
+(Car *)initWithName:(NSString *)name{
    Car *car = [Car alloc] init];
    car.name = name;
    return car;
}
 
@end
	```
	然后改写Person类，实现类Person.m代码如下：
	
	```
#import "Person.h"
#import "Car.h"
@implementation Person
 
+(Person *)personWithAge:(int)age withName:(NSString *)name withCar:(Car *)car{
    Person *person = [[Person alloc] init];
    person.age = age;
    person.name = name;
    person.car = car;
    return person;
}
 
//这里重写description方法，用于最后测试排序结果显示
-(NSString *)description{
    return [NSString stringWithFormat:@"age is %zi , name is %@, car is %@",_age,_name,_car.name];
}
 
@end
	```
	主函数代码如下：
	
	```
void sortArray4(){
        //首先来3辆车，分别是奥迪、劳斯莱斯、宝马
        Car *car1 = [Car initWithName:@"Audio"];
        Car *car2 = [Car initWithName:@"Rolls-Royce"];
        Car *car3 = [Car initWithName:@"BMW"];
         
        //再来5个Person，每人送辆车，分别为car2、car1、car1、car3、car2
        Person *p1 = [Person personWithAge:23 withName:@"zhangsan" withCar:car2];
        Person *p2 = [Person personWithAge:21 withName:@"zhangsan" withCar:car1];
        Person *p3 = [Person personWithAge:24 withName:@"lisi" withCar:car1];
        Person *p4 = [Person personWithAge:23 withName:@"wangwu" withCar:car3];
        Person *p5 = [Person personWithAge:23 withName:@"wangwu" withCar:car2];
 
     
        //加入数组
        NSArray *array = [NSArray arrayWithObjects:p1,p2,p3,p4,p5, nil];
         
        //构建排序描述器
        NSSortDescriptor *carNameDesc = [NSSortDescriptor sortDescriptorWithKey:@"car.name" ascending:YES];
        NSSortDescriptor *personNameDesc = [NSSortDescriptor sortDescriptorWithKey:@"name" ascending:YES];
        NSSortDescriptor *personAgeDesc = [NSSortDescriptor sortDescriptorWithKey:@"age" ascending:YES];
         
        //把排序描述器放进数组里，放入的顺序就是你想要排序的顺序
        //我这里是：首先按照年龄排序，然后是车的名字，最后是按照人的名字
        NSArray *descriptorArray = [NSArray arrayWithObjects:personAgeDesc,carNameDesc,personNameDesc, nil];
         
        NSArray *sortedArray = [array sortedArrayUsingDescriptors: descriptorArray];
        NSLog(@"%@",sortedArray);
}
	```
	结果如下：
	![image](/images/数组排序/20150721.png)