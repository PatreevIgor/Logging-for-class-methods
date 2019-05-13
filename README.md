class SomeClass
  def method_without_args
    'method_without_args'
  end

  def method_with_args(a,b)
    args = []; method(__method__).parameters.map {|_, name| args << binding.local_variable_get(name)}
    puts ("Call method: #{__callee__}, with args: #{args}")

    "method_with_args(#{a},#{b})"
  end

  [:method_without_args].each do |m|
    alias_method "original_#{m.to_s}".to_sym, m

    define_method m do
      puts("Call method: #{__callee__}")
      send("original_#{m.to_s}".to_sym)
    end
  end
end

SomeClass.new.method_without_args
SomeClass.new.method_with_args('1','2')
