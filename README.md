# Method Locator

Method Locator is a Ruby gem that allows you to easily determine the method lookup path of a particular
object / class / module, as well as find all places (represented as UnboundMethod instances) where
a callable method is defined on an object. This is very useful in an environment such as Rails where
you are unsure where a method may be defined or overridden.

## Installation

Method Locator is available as a RubyGem:

    gem install method_locator

## Examples

```ruby
module M1
  def foo
    puts "foo from M1"
  end
end

module M2
  def foo
    puts "foo from M2"
  end
end

module M3
  def blah
    puts "blah from M3"
  end
end

module M4
  def self.hello
    puts "hello from M4"
  end
end

class A
  def foo
    puts "foo from A"
  end

  def self.blah
    puts "blah from A"
  end
end

class B < A
  include M1
  include M2

  def foo
    puts "foo from B"
  end

  def self.blah
    puts "blah from B"
  end
end

B.extend(M3)

b = B.new

def b.foo
  puts "foo from b's singleton class!"
end

puts "all method owners of b#foo"
b.methods_for(:foo).each { |m| puts "#{m.name}, owner: #{m.owner}" }

puts "all method owners of B.blah"
B.methods_for(:blah).each { |m| puts "#{m.name}, owner: #{m.owner}" }

puts "all method owners of B.new"
B.methods_for(:new).each { |m| puts "#{m.name}, owner: #{m.owner}" }

puts "all method owners of M4.hello"
M4.methods_for(:hello).each { |m| puts "#{m.name}, owner: #{m.owner}" }

puts "method lookup path for b"
p b.method_lookup_path

puts "method lookup path for B"
p B.method_lookup_path

puts "method lookup path for M2"
p M2.method_lookup_path


# OUTPUT

all method owners of b#foo
foo, owner: #<Class:#<B:0x007fcc74031d38>>
foo, owner: B
foo, owner: M2
foo, owner: M1
foo, owner: A

all method owners of B.blah
blah, owner: #<Class:B>
blah, owner: M3
blah, owner: #<Class:A>

all method owners of B.new
new, owner: Class

all method owners of M4.hello
hello, owner: #<Class:M4>

method lookup path for b
[#<Class:#<B:0x007fcc74031d38>>, B, M2, M1, A, Object, MethodLocator, Kernel, BasicObject]

method lookup path for B
[#<Class:B>, M3, #<Class:A>, #<Class:Object>, #<Class:BasicObject>, Class, Module, Object, MethodLocator, Kernel, BasicObject]

method lookup path for M2
[#<Class:M2>, Module, Object, MethodLocator, Kernel, BasicObject]
```

## Note on Patches/Pull Requests

* Fork the project.
* Make your feature addition or bug fix.
* Add tests for it. This is important so I don't break it in a future version unintentionally.
* Commit, do not mess with rakefile, version, or history. (if you want to have your own version, that is fine but bump version in a commit by itself I can ignore when I pull)
* Send me a pull request. Bonus points for topic branches.

## Authors

* Ryan LeCompte

## Copyright

Copyright (c) 2011 Ryan LeCompte. See LICENSE for
further details.