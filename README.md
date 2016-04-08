# 20160408
学习一下  来自rubychina  https://ruby-china.org/topics/28387  
map心得
有个数组

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
我们想生成一个把 numbers 中每个值映射到 String 的 Array , 通常我们这么做

 numbers.map { |number| number.to_s }
简写就是这样

numbers.map(&:to_s)
现在有一个名字的数组

names = %w[女朋友 小二 小三 小四 小五 小六 小七 小八 小九 小十]
我们想根据 numbers 在 names 中的位置找到对应的名字， 通常我们这么做

numbers.map { |number| names[number - 1] }  # 程序员从 0 开始数数
花括号好烦不想写花括号

 numbers.map(&names.method(:[]))
诶不对没有减 1， 没关系可以这样

numbers.map { |number| number - 1 }.map(&names.method(:[]))
我去还是有花括号， 那这样

minus_one = -> (number) do 
  number - 1
end

numbers.map(&minus_one).map(&names.method(:[]))
do ... end 和花括号没有区别啊

numbers.map(&1.method(:-)).map(&0.method(:-)).map(names.method(:[]))
至于如何 map 一个 Object 上的方法 并给这个方法传递一个值作为参数，不存在多态的时候可以这样

class Object
  def self.pure_instance_method(name)
    ->(*args) { 
      ->(obj) {
        self.instance_method(name).bind(obj)[*args]
      }
    }
  end
end

names.map(&String.pure_instance_method(:[])[-1]) # 所有名字的最后一个字
存在多态或者不知道数组中元素类型的时候我就不会了， 这里抛砖引个玉 ~
