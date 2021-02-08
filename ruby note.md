### introduction

#### print

```
print "Hello HackerRank!!"
```

#### 判断

```
return number.even?
number.even?
```

#### Object Method Parameters

```
a.range?(b, c)
return a.range?(b, c)
a.range? b, c
return a.range? b, c
```

### data structure

#### Ruby Hash

##### initialize

create empty hash

```
empty_hash = Hash.new 
```

- Initialize an empty Hash with the variable name `default_hash` and the default value of every key set to `1`.

```
default_hash = Hash.new(1)

default_hash = Hash.new
default_hash.default = 1
```

Initialize a hash with the variable name `hackerrank` and having the key-value pairs

```
hackerrank = {"simmy" => 100, "vivmbbs" => 200}

hackerrank = Hash.new
hackerrank["simmy"] = 100
hackerrank["vivmbbs"] = 200
```

#### Ruby Array

##### initialize

empty array

```
array = Array.new

array = []
```

- Initialize an array with exactly one `nil` element in it with the variable name `array_1`

**Hint**

```
array_1 = Array.new(1)
```

or

```
array_1 = [nil]
```

- Initialize an array with exactly two elements with value `10` in it using the variable name `array_2`.

**Hint**

```
array_2 = Array.new(2, 10)
```

or

```
array_2 = [10, 10]
```

##### index

The positions are `0` indexed. Objects of the array can be accessed using the `[]` method which may take various arguments, as explained below.

```
arr = [9, 5, 1, 2, 3, 4, 0, -1]
```

- A number which is the position of element

```
>>arr[4]
  => 3
```

or

```
>>arr.at(4)
  => 3 
```

- A range indicating the start and the end position

```
>>arr[1..3] # .. indicates both indices are inclusive. 
  => [5,1,2]
>>arr[1...3] # ... indicates the last index is excluded.
  => [5,1]
```

- Start index and the length of the range

```
>>arr[1,4]
  => [5, 1, 2, 3]
```

To access the elements from the end of the list, we can use negative indices.

For the array,

```
arr = [9, 5, 1, 2, 3, 4, 0, -1]
 > arr[-1]
 => -1
```

- The first element of the array can be accessed using

```
 > arr.first
 => 9
```

- The last element of the array can be accessed using

```
 > arr.last
 => -1
```

- The first `n` elements of the array can be accessed using

```
 arr.take(3)
 => [9, 5, 1]
```

- Everything but the first `n` elements of the array can be accessed using

```
 arr.drop(3)
 => [2, 3, 4, 0, -1]
```

##### add

- `push` allows one to add an element to the end of the list.

```
 >x = [1,2]
 >x.push(3)
 => [1,2,3]
```

- `insert` allows one to add one or more elements starting from a given index (shifting elements after the given index in the process).

```
 >x = [1,2]
 >x.insert(1, 5, 6, 7)
 => [1, 5, 6, 7, 2]
```

- `unshift` allows one or more elements to be added at the beginning of the list.

```
 >x = [1,2,3]
 >x.unshift(10, 20, 30)
 => [10, 20, 30, 1, 2, 3]
```



### control structure

#### each

Ruby offers control structures that let you iterate through its collections. One such control structure is `each`.

As you already know, HackerRank conducts many contests, and for every user who participates in a contest we update their score once the contest ends. You will be given a method called `scoring` with an array passed as an argument. Elements of the array are of the class `User`.

`User` class has a method `update_score`.

Your task is to iterate through each of the elements in the array using `each` and call the method `update_score` on every element.

**Hint**

```
array.each do |user|
    # call update_score on `user` here
end
```

```
def scoring(array)
    array.each do |user|
        user.update_score
    end
end
```

#### unless

Like the previous challenge, you are given a method `scoring` with an array passed as an argument. Each element of the array is of class `User`.

`User` has two public methods, `is_admin?` and `update_score`. Your task in this challenge is to use the control structure `unless` and update all elements of the array who are not `admins`.

**Hint**

```
unless user.is_admin?
    user.update_score
end
```

or

```
user.update_score unless user.is_admin? 
```

The above code is a Ruby one liner.

**Explanation**

```
unless` is the logical equivalent of `if not
```

```
def scoring(array)
    array.each do |i|
      unless i.is_admin?
          i.update_score
      end
    end
end
```

#### infinite loop

An infinite loop in Ruby is of the form

```
loop do
end
```

Use an infinite loop and call the method `coder.practice` within it and break if `coder.oh_one?` is true.

`break if` conditions in Ruby are of the form

```
if <condition>
    break
end
```

or a one-liner

```
break if <condition>  
```

```
loop do
    coder.practice
    break if coder.oh_one?
end
```

#### while not / until

Call the method `coder.practice` until `coder.oh_one?` becomes `true`.

Use the `until` control structure.

`until` is the logical equivalent of `while not`.

**Hint**

```
while not <condition>
    <code>
end
```

or

```
until <condition>
    <code>
end
```

or the beautiful one-liner

```
<code> until <condition>  
```

```
coder.practice until coder.oh_one?
```

#### case

You have been given a function where an object which may or may not be of the above mentioned type is sent as an argument. You have to use the `case` control structure in Ruby to identify the class to which the object belongs and print the following output:

- if `Hacker`, output "It's a Hacker!"
- if `Submission`, output "It's a Submission!"
- if `TestCase`, output "It's a TestCase!"
- if `Contest`, output "It's a Contest!"
- for any other object, output "It's an unknown model"

**Note**

- use `case` (switch statement of Ruby)
- use `puts` for printing
- Ruby Docs on [case](http://ruby-doc.org/docs/keywords/1.9/Object.html#method-i-case)

```
def identify_class(obj)
    case obj
    when Hacker
        puts "It's a Hacker!"
    when Submission
        puts "It's a Submission!"
    when TestCase
        puts "It's a TestCase!"
    when Contest
        puts "It's a Contest!"
    else
        puts "It's an unknown model"
    end
end
```

