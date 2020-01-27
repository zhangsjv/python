# 第一章 用pythonic方式思考
* 1. **一门语言的编程习惯是由用户来确定的** 挺好玩。好多东西的发展都是由用户来推动的，当然科技的力量也必不可少。在这个过程中可能会增加很多在设计之初根本就没有想过的事情。比如计算机的发明是为了计算，然后现在最广泛的用途好像是传播信息。当然现在的计算机的计算能力比之前强了不知道多少倍了，可是为什么它的发展并没有仅限于计算呢？是从什么时候开始它的能力有了“基因突变”呢？

* 2. 对编写代码来说，可读性比简短更重要。python可以只通过一行代码完成很多事情，但是如果这一行包括很多括号来帮助理顺逻辑，那么应该舍弃这种写法。
* 3. 如果需要重复操作，那就把那种操作写成函数。
* 4. 列表的切割问题：
    * a=[0,1,2,3,4,5,6,7,8,9]
    * a[2:7]=[100,200,300]
    * 变化之后 a=[0,1,100,200,300,8,9]
    * 当切割与赋值同时进行的时候，只对切割范围内赋值，其他位置上的值不变。如果赋值个数小于切割范围，就相当于列表缩小了；如果赋值个数大于切割范围，就等于把列表扩展了
    
* 5. 列表表达式：
    * matrix=[[1,2],[3,4]]
    * a=[x for row in matrix for x in row]----> a=[1,2,3,4]
    * b=[[x^2 for x in row] for row in matrix]----> b=[[1,4],[9,16]]
    * c=[x for x in a if x>1 and <4]----> c=[2,3]
    
* 6. 生成器表达式：
    * it=(len(x) for x in open('myfile.txt'))
    * 与列表表达式的区别为圆括号；当使用it的时候才会调用其表达式进行运算，不像列表会把所有的数据都读出来。所以这种方式可以适用于大数据量的情况。

* 7. 同时打印列表元素与元素下标
   * `string=['a','b','c','d','e','f']`
   *  for i,s in enumerate(string):
      *   print(i,s)
   * 结果会是 0,'a';1,'b';2,'c'....
   * for i,s in enumerate(string,1):
     * print(i,s)
   * 结果会是 1,'a';2,'b';3,'c'....

* 8. zip的使用：
   * zip 可以把两个及以上的列表，生成器组合成一个生成器，该生成器将所有列表的元素按顺序组合成元组返回
   * a=[1,2,4]  b=[4,5,6]
   * for i,j in zip(a,b)
   *  print(i,j)---->(1,4);(2,5);(3,6)
   * **注意： zip的使用要求每个列表的的长度一致   len(a)==len(b)
* 9. try/except/else/finally的使用
   * finally 语句在任何条件下都要执行的，所以可以用来写一些必须执行的语句，比如关闭文件。当然，前提是文件是打开的，也就是说，打开文件的语句要在try的前面
   * **_不太明白else的作用。既然try执行成功时else的内容才能执行，为什么不把else的内容放到try里边呢？_**
   
* 10. 在设计函数的时候需要考虑，如果将来函数的返回值需要做if语句的判定条件，那么需要考虑当返回值是0时该怎么处理。因为返回值0有极大的可能是函数的正常返回值，但是但还进行条件判定的时候会判定为Falser

* 11. 使用生成器函数代替列表类型返回值。
   * def index_word_iter(text):
      * if text:
         * yield 0
      * for index, letter in enumerate(text)
         * if letter== ' ':
            * yield index+1
   * 如果text存在，先返回其单词第一个字母下标，也就是0；然后循环访问text（可能一个单词，可能一行单词，可能一个文本）的每个字符，如果遇到空格，则记录当前位置的下一个位置，因为那是下一个单词的首字母字符下标。 这个会返回一个迭代器，只有在访问这个迭代器的时候才会读取，所以不会占用太大内存。
   * **注意： 如果把迭代器赋值给某个变化，那么这个变量只能使用一次。比如：
      * a=index_word_it
      * for i in a: # 第一次调用，可以正确的打印出内容
         * print(i)
      
      * for i in a: # 第二次调用，打印内容为空，因为a的内容已经没了
         * print(i)
         
      * 可以使用 for i in index_word_it(text): 来代替

* 12.  ‵*arg是什么参数
   * 表示可变参数。如果函数中有这个参数，表示从这个位置开始可以有任意个数的参数。如果想把列表中的元素当作参数，需要在列表前加上`*
   * def log(message,*value): 
        * if not value: 
            * print(message) 
        * else: 
            * value_str=','.join(str(x) for x in value) 
            * print("{},{}".format(message,value_str)) 
   * 这样可以令用户的调用者个函数时更方便。
   
* 13. 关键字参数
   * def rem(number,divisor):
      * return number%divisor
      
   * 在调用这个函数的时候可以使用以下几种方式：
      * rem(20,7)
      * rem(20,divisor=7)
      * rem(number=20,divisor=7)
      * rem(divisor=7,number=20)
   * 以上number=，divisor=，都叫做关键参数。
   * 必须保证位置参数在关键参数之前：rem(number=20,7)这是不合法的
   * 必须保证每个参数只指定以此，比如 rem（7，number=20）这是不合法的，因为7已经占据了参数number的位置，表明在number已经有值了，再使用number关键字参数就不行了，在这种情况下必须把divsor=也加上表明这个参数时传递给divisor的。
   * 需要记住这个和默认参数的区别：
      * def rem(number=20,divisor=7):
         * return number%divisor
      * 再定义函数的时候使用 number=20，divisor=7，表示在调用函数时如果没有使用参数，那么就使用默认的参数20和7

* 14. 动态默认值参数
   * def log(message,when=None):
      * print('{},{}'.format(message,when=datetime.now()))
      
   * 这样会在每次调用函数的时候打印出调用函数所在的时间
   
* 15. 如何保证每次调用函数时必须使用关键字参数以明晰其意图
   * def rem(number,divisor,* ignore_overflow=False):
      * ...
   * 在调用这个函数的时候，如果想更改* 号之后的参数必须使用关键字参数。 rem(20,7,ignore_overflow=True)是合法的；rem(20,7,True)是不合法的 
   
   
   
   
   
   
   
   
   
   
   
   
            
                                                
      
      
   
