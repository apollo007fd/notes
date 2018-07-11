[牛客网链接](https://www.nowcoder.com/practice/3194a4f4cf814f63919d0790578d51f3?tpId=13&tqId=11197&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)



题意分析：

输入一个句子，单词之间由" "分隔，单词与之后的标点符号表示一个单词。

输出所有单词翻转后的句子。

比如输入："I am a student."

输出："student. a am I"



测试用例猜测：

""、" "、若干个连续" "、句子前后有" "、



解题思路：

先split，再用栈翻转；



```java
public String ReverseSentence(String str) {
		if(str.trim().equals(""))
			return str;
		String[] strs = str.split(" ");
		Stack<String> stack = new Stack<String>();
		for(String s: strs){
			if(!s.equals(""))
				stack.push(s);
		}
		StringBuilder res = new StringBuilder(stack.pop());
		while(!stack.isEmpty()){
			res.append(" ");
			res.append(stack.pop());
		}
		return new String(res);
    }
```

