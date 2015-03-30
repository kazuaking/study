

# ruby study

## Hello world

### ruby

```bash
$ vi hello.rb
```


```ruby
puts 'hello world'
```


```bash
$ ruby hello.rb
hello world
```

or

```bash
ruby -e "puts 'hello world'"
```

### java

```bash
$ vi hello.java
```

```java
class hello{
  public static void main(String args[]){
    System.out.println("hello world");
  }
}
```

```bash
$ javac hello.java
$ java hello.class
```

### php

```bash
$ vi hello.php
```

```php
<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <?php echo 'Hello World'; ?> 
 </body>
</html>
```

```bash
$ open http://localhost/hello.php
```

or


``` bash
$ php -r "echo 'hello world';"
```


## Object

### 数値

```ruby
123   #=> 123
123.class   #=> Fixnum
123.12.class   #=> Float
9999999999999999999.class  #=> Bignum
```
* 精度によって型がかわります。

```ruby
a = 1
++a #=> 1 #効果無し
a-- #=> syntax error, unexpected keyword_end
```
* インクリメント/デクリメントはありません
```ruby
a = 1
a += 1 #=> 2
a -= 10 #=> -8
```
* rubyでは変数はオブジェクトの扱いではなく、オブジェクトのポインタを示すラベルという扱いになっている
* インクリメントを行おうとすると１が格納された変数の値だけでなく、プログラム内で扱っている１という数字へのポインタが２を向くように
```ruby
1.__id__ #=> 3
2.__id__ #=> 5
3.__id__ #=> 7
a = 1
a.__id__ #=> 3
a = 2
a.__id__ #=> 5
a = 3
a.__id__ #=> 7
```
* 上記のようなrubyの設計上の理由で使えないようになっている
* 参考: http://abe-log.cocolog-nifty.com/blog/2012/03/ruby-62d2.html

### 文字列
```ruby
"123"   #=> "123"
"123".class   #=> String
:hoge   #=> :hoge
:hoge.class   #=> Symbol
"fuga".to_sym  #=> :fuga
```
* Stringは定義するたびにメモリを使いますが、Symbolは一度定義するとメモリが固定され、使い回されます。同じオブジェクトが使われることになるのでhashのキーによく使います。

##### シンボルに関しての注意

```ruby
Symbol.all_symbols.size #=> 40328
:hoge #=> :hoge
Symbol.all_symbols.size #=> 40329
```
* Symbolは永久に消えません。Ruby2.2.0からGCの対象になったようですがそれ以前のRubyの場合GCの対象ではありません。むやみなto_symは危険です。
 * 参考：　http://fiveteesixone.lackland.io/2015/01/21/symbol-gc-ruby-2-2/


### 疑似変数
```ruby
true.class   #=> TrueClass
false.class   #=> FalseClass
nil   #=> nil
nil.class   #=> NilClass
```
* true/false/nil( null )は疑似変数と呼ばれ特定のオブジェクトへのポインタをさしていて、ユーザーは変更出来ません。

※.classはObjectクラスに定義されています。 http://docs.ruby-lang.org/ja/2.2.0/class/Object.html


### 変数

##### 参照渡しです。

```ruby
a = 'hoge'
b = nil

a.__id__ #=> 351208
b.__id__ #=> 8
b = a

b.__id__ #=> 351208
```
* rubyでは変数はオブジェクトの扱いではなく、オブジェクトのポインタを示すラベルという扱いになっている

##### メソッドがレシーバを返すものには注意が必要です。
```ruby
a = 'bar'
b = a

a.insert(0, 'foo')  # レシーバが返される
a #=> "foobar"
b #=> "foobar"
```



### Array
```ruby
# 宣言
hoges = []   #=> []
hoges = [1, 2, 4, 6]    #=>[1, 2, 4, 6]
# 追加
hoges << 8   #=>[1, 2, 4, 6, 8]
fugas << 3   # 変数が無いと怒られます。
#=> NameError: undefined local variable or method `hugas' for main:Object

# 取得
hoges[0]   #=> 2
# 削除
hoges.delete(8)  #=> 8
hoges   #=>[1, 2, 4, 6]
hoges.delete_at(0)  #=> 1
hoges   #=>[2, 4, 6]
# 操作のメソッドはだいたいそろってる

```

### hash
```ruby
# 宣言
hoges = {}   #=> {}
hoges = {hoge: 1, fuga: 2}   #=> {:hoge=>1, :fuga=>2}
# 追加
hoges[:piyp] = 5 #=> {:hoge=>1, :fuga=>2, :piyp=>5}
fugas[:piyo] = 5 # 変数が無いと怒られます。
#=> NameError: undefined local variable or method `fugas' for main:Object

# 取得
hoges[:hoge]   #=> 1
# 削除
hoges.delete(:fuga) #=> 8
hoges   #=> {:hoge=>1, :piyp=>5}
# 操作のメソッドはだいたいそろってる
```
* ruby 1.9 から、hashはシンボルを使用する場合,  '{hoge: 1}'で書けます。
シンボル以外は  {‘hoge’ => 1} という書き方になります。
※通称ハッシュロケット





### if

```ruby
if 'hoge'.length > 3
  puts 'fuga'
elsif 'hoge'.length > 2
  puts 'piyo'
else
  puts 'buu'
end
# 表示 'fuga'
# => nil
```
* ifの書き方はこんな感じ


```ruby
foo = if 'hoge'.length > 3
        'fuga'
      elsif 'hoge'.length > 2
        'piyo'
      else
        'boo'
      end
# => 'fuga'
puts foo
# 表示 'fuga'
```

* ifはブロック内の最後の行の返り値を返します。(一時変数が減らせるのでよくつかいます）

```ruby
puts 'hoge'.length > 3 ? 'fuga' : 'puyo'
# 表示 'fuga'
# => nil
```
* `if/else`のような単純な条件で単一の戻り値の場合、三項演算子が推奨されています。

### 比較演算子

詳しくはこちら
http://docs.ruby-lang.org/ja/1.9.3/doc/symref.html

### loop

#### for 

```rb
# for
list = [1, 2, 3, 5, 8]
for item in list
  puts item
end
1
2
3
5
8
#=> [1, 2, 3, 5, 8]  # listが返る
```

* forはこんな感じ（使った記憶無し）

#### while

```ruby
cnt = 0
while cnt < 3
  puts 'foobar'
  cnt += 1
end
# 表示:
#   foobar
#   foobar
#   foobar
#=> nil
```
* whileもある（使った記憶無し）

#### each/map

* `for` `while`はほとんどの場合、eachでかけるので使うことは少ないです（書いた覚えがありません）

```rb
list.each do |item|
  puts item
end
1
2
3
5
8
#=> [1, 2, 3, 5, 8]
```
* こんなかんじ

```rb
list.each { |item| puts item }
1
2
3
5
8
#=> [1, 2, 3, 5, 8]
```

* ブロック`do...end`は`{...}で代用出来ます。１行ですみます。(改行も可能ですが、RubyStyleGuideでは`{}`の場合１行で改行が必要な場合は、`do...end`が推奨されています。見た目の問題です。

```rb
list.map do |item|
  item.to_s
end
#=> ["1", "2", "3", "5", "8"]
```

* mapはブロック内の最後の戻り値を配列として返します。（よく使います）

```rb
list.map { |item| item.to_s }
#=> ["1", "2", "3", "5", "8"]
```
* これも一行で記述可能です。

```rb
list.map(&:to_s)
#=> ["1", "2", "3", "5", "8"]
```

* ブロックのレシーバが単一のメソッドを呼び出す場合、上記のように書きます。（わかってる人風のコードなのでナウいです）


```rb
5.times.map { :a } #=> [:a, :a, :a, :a, :a] # Enumeratorクラス
(5...10).map { |a| a } #=> [5, 6, 7, 8, 9]  # Rangeクラス
```
* eachでも可




