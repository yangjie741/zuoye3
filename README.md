# zuoye3
## 8.4
```cpp
#include<iostream>
#include<fstream>
#include<sstream>
#include<string>
#include<vector>
using namespace std;

int main(int argc, char**argv)
{
string infile = "1.txt";
vector<string> vec;//声明一个vector
ifstream in(infile);
if (in)
{
    string buf;
    while (getline(in,buf))
    {
        vec.push_back(buf);
    }
}
else
{
    cerr<<"cannot open this file: "<<infile<<endl;
}
for (int i = 0;i < vec.size();++i)
{
    cout<<vec[i]<<endl;
}

return 0;
}
```
## 8.7
fstream out(argv[2])  
## 8.9
```cpp
#include<iostream>
#include<fstream>
#include<sstream>
#include<string>
#include<vector>
using namespace std;

istream& func(istream& is)
{
string buf;
while (is >> buf) 
    std::cout << buf << std::endl;
is.clear();
return is;
}

int main(int argc, char**argv)
string infile = "1.txt";
vector<string> vec;
ifstream in(infile);
istringstream iss("hello");
func(iss);
if (in)
{
    string buf;
    while (in >> buf)
    {
        vec.push_back(buf);
    }
}
else
{
    cerr<<"cannot open this file: "<<infile<<endl;
}
for (int i = 0;i < vec.size();++i)
{
    cout<<vec[i]<<endl;
}

return 0;
}
```
## 16.11
在List类内，使用模版名不需要再加参数列表，而ListItem使用时必须加上<>模版参数列表

## 16.19
```cpp

#ifndef HAVE_H
#define HAVE_H
 
template <typename T> void Have(T &t)
{
	for (size_t i = 0; i < t.size(); ++i)
	{
		cout<<t[i]<<endl;
	}
}
#endif HAVE_H
```
```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
#include "Have.h"
using namespace std;
 
int main(int argc,char** argv)
{
	vector<int> vec1;
	vec1.push_back(2);
	vec1.push_back(3);
	Have(vec1);
	system("pause");
	return 0;
}
```

## 16.41
```cpp
#ifndef REU_TYPE_H
#define REU_TYPE_H
 
template <typename T> auto sum(const T&a,const T&b) ->decltype(a+b)//将函数的返回类型指定为a+b的类型
{
	return a+b;
}
#endif REU_TYPE_H
```
```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
#include "Reu_type.h"
using namespace std;
 
 
int main(int argc,char** argv)
{
	int a = 566669;
	int b = 595955;
	cout<<sum(a,b);
	cin.get();
	return 0;
}
```
## 16.62
```cpp
#include <iostream>  
#include <string>  
class Sales_data  
{  
    std::string bookNo;  
    unsigned units_sold;  
    double revenue;  
    double avg_price()const { return units_sold ? revenue / units_sold : 0; }  
public:  
    Sales_data(const std::string &s=std::string(), unsigned n = 0, double p = 0) :bookNo(s), units_sold(n), revenue(p) {}  
    Sales_data(std::istream &is);  
    std::string isbn()const { return bookNo; }  
    Sales_data &operator+=(const Sales_data &s);  
    friend std::hash<Sales_data>;  
    friend std::ostream &operator<<(std::ostream &os, const Sales_data &s);  
    friend std::istream &operator>>(std::istream &is, Sales_data &s);  
    friend bool operator==(const Sales_data &ls, const Sales_data &rs);  
    friend Sales_data operator+(const Sales_data &ls, const Sales_data &rs);  
    friend std::ostream &print(std::ostream &os, const Sales_data &s);  
    friend std::istream &read(std::istream &is, Sales_data &s);  
};  
bool operator!=(const Sales_data &ls, const Sales_data &rs);  
Sales_data add(const Sales_data &ls, const Sales_data &rs);  
  
namespace std  
{  
    template<> struct hash<Sales_data>  
    {  
        typedef size_t result_type;  
        typedef Sales_data argument_type;  
        size_t operator()(const Sales_data &s)const {  
            return hash<string>()(s.bookNo) ^ hash<unsigned>()(s.units_sold) ^ hash<double>()(s.revenue);  
        }  
    };  
}
```
```cpp

#include "Sales_data.h"  
  
Sales_data::Sales_data(std::istream &is)  
{  
    is >> *this;  
}  
  
Sales_data &Sales_data::operator+=(const Sales_data &s)  
{  
    units_sold += s.units_sold;  
    revenue += s.revenue;  
    return *this;  
}  
  
std::ostream &operator<<(std::ostream &os, const Sales_data &s)  
{  
    os << s.isbn() << " " << s.units_sold << " " << s.revenue << " " << s.avg_price();  
    return os;  
}  
  
std::istream &operator>>(std::istream &is, Sales_data &s)  
{  
    double price;  
    is >> s.bookNo >> s.units_sold >> price;  
    if (is)  
        s.revenue = s.units_sold*price;  
    else  
        s = Sales_data();  
    return is;  
}  
  
bool operator==(const Sales_data &ls, const Sales_data &rs)  
{  
    return ls.bookNo == rs.bookNo&&ls.units_sold == rs.units_sold&&ls.revenue == rs.revenue;  
}  
bool operator!=(const Sales_data &ls, const Sales_data &rs)  
{  
    return !(ls == rs);  
}  
  
Sales_data operator+(const Sales_data &ls, const Sales_data &rs)  
{  
    Sales_data temp = ls;  
    temp += rs;  
    return temp;  
}  
  
Sales_data add(const Sales_data &ls, const Sales_data &rs)  
{  
    Sales_data temp = ls;  
    temp += rs;  
    return temp;  
}  
  
std::ostream &print(std::ostream &os, const Sales_data &s)  
{  
    os << s.isbn() << " " << s.units_sold << " " << s.revenue << " " << s.avg_price();  
    return os;  
}  
  
std::istream &read(std::istream &is, Sales_data &s)  
{  
    double price;  
    is >> s.bookNo >> s.units_sold >> price;  
    s.revenue = s.units_sold*price;  
    return is;  
}
```
```cpp
#include <memory>  
#include <unordered_set>  
#include "Sales_data.h"  
  
int main()  
{  
    //! test for ex16.62  
    std::unordered_multiset<Sales_data> mset;  
    Sales_data sd("Bible", 10, 0.98);  
  
    mset.emplace(sd);  
    mset.emplace("C++ Primer", 5, 9.99);  
  
    for (const auto &item : mset)  
        std::cout << "the hash code of " << item.isbn()  
        << ":\n0x" << std::hex << std::hash<Sales_data>()(item)  
        << "\n";  
    system("pause");  
    return 0;  
} 
```
## 12.1
b1和b2都包含4个元素。
## 12.10
正确
## 12.15
直接将删除器函数修改即可  
```cpp
[](connection *p){ disconnect *p; }
```
## 12.17
(a)：不合法，ix不是new返回的指针  
(b)：不合法，不是new返回的指针  
(c)：合法，unique_ptr必须采用直接初始化  
(d)：不合法，不是new返回的指针  
(e)：合法  
(f)：不合法，必须使用new返回的指针进行初始化，赋值和拷贝的操作也不包含get()方法。
## 12.18
release()函数的作用就是放弃对指针指向对象的控制权，但shared_ptr是多对一的关系，其他的智能指针仍然可以删除这个对象，所以这个函数对shared_ptr没意义
## 12.19
```cpp

class StrBlob
{
public:
	friend class StrBlobPtr;//声明friend
	StrBlobPtr begin();
	StrBlobPtr end();
	StrBlob();//默认构造函数
	StrBlob(initializer_list<string> il):data(make_shared<vector<string>>(il)){}///C++11新特性
	StrBlob(string il):data(make_shared<vector<string>> (il)){}//另一构造函数
	typedef vector<string>::size_type size_type;//定义类型别名，方便使用
 
	//定义函数，返回大小
	size_type size() const
	{
		return data->size();
	}
	//判断vector<string>是否为空
	bool empty()
	{
		return data->empty();
	}
	//向vector<string>中加入元素
	void pushback(const string &s)
	{
		data->push_back(s);
	}
	//访问函数，应首先调用check()
	string& front()
	{
		check(0,"front on empty StrBlob");
		return data->front();
	}
	string& back()
	{
		check(0,"back on empty StrBlob");
		return data->back();
	}
	void popback()
	{
		check(0,"pop_back on empty StrBlob");
		data->pop_back();
	}
 
private:
	shared_ptr<vector<string>> data;//指向vector<string>的智能指针
	void check(size_type i,const string &msg) const//若访问元素的大小大于data的size,输出错误信息
	{
		if (i > data->size())
		{
			throw out_of_range(msg);//抛出该out_of_range异常，表示不在范围之内
		}
	}
};
 
class StrBlobPtr
{
public:
	StrBlobPtr():curr(0){}//构造函数，将curr设定为0
	StrBlobPtr(StrBlob &a, size_t sz = 0):wptr(a.data),curr(sz){}//构造函数，将StrBlob的智能指针与此类中的weak_ptr绑定
	string& deref() const
	{
		auto p =check(curr,"deference past end");
		return (*p)[curr];
	}
	StrBlobPtr& incr()
	{
		auto p =check(curr,"deference past end");
		++curr;
		return *this;
	}
private:
	shared_ptr<vector<string>> check(size_t i,const string& msg) const//检查函数，返回一个vector<string>的智能指针
	{
		auto ret = wptr.lock();//检查对象是否还存在
		if(!ret)
		{
			throw runtime_error("未绑定");
		}
		if (i >= ret->size())
		{
			throw out_of_range(msg);
		}
		return ret;
	}
	weak_ptr<vector<string>> wptr;//定义弱智能指针
	size_t curr;//设立游标，表示下标
 
};
 
StrBlobPtr StrBlob::begin()
{
	return StrBlobPtr(*this);
}
StrBlobPtr StrBlob::end()
{
	return StrBlobPtr(*this, data->size());
}
```
## 12.30
```cpp

#include "Chapter12.h"
#include <iostream>
using namespace std;
 
void runQueries(std::ifstream& infile)
{
	TextQuery tq(infile);
	while (true) {
	cout << "enter word to look for, or q to quit: ";
	string s;
	if (!(cin >> s) || s == "q") break;
	print(cout, tq.query(s)) << endl;
	}
}
 
int main()
{
	ifstream file("1.txt");
	runQueries(file);
}
```
```cpp
#ifndef Cccc
#define Cccc
 
#include <string>
#include <fstream>
#include <map>
#include <set>
#include <vector>
#include <iostream>
#include <sstream>
using namespace std;
 
class QueryResult;
class TextQuery {
public:
	using LineNo = vector<string>::size_type;
	TextQuery(std::ifstream&);
	QueryResult query(const string&) const;
 
private:
	shared_ptr<vector<string>> input;
	std::map<string, shared_ptr<std::set<LineNo>>> result;
};
 
class QueryResult {
public:
	friend std::ostream& print(std::ostream&, const QueryResult&);
 
public:
	QueryResult(const string& s, shared_ptr<std::set<TextQuery::LineNo>> set,
		shared_ptr<vector<string>> v)
		: word(s), nos(set), input(v)
	{
	}
 
private:
	string word;
	shared_ptr<std::set<TextQuery::LineNo>> nos;
	shared_ptr<vector<string>> input;
};
 
std::ostream& print(std::ostream&, const QueryResult&);
 
TextQuery::TextQuery(std::ifstream& ifs) : input(new vector<string>)
{
	LineNo lineNo{0};
	for (string line; std::getline(ifs, line); ++lineNo) {
		input->push_back(line);
		std::istringstream line_stream(line);
		for (string text, word; line_stream >> text; word.clear()) {
			// avoid read a word followed by punctuation(such as: word, )
			std::remove_copy_if(text.begin(), text.end(),
				std::back_inserter(word), ispunct);
			// use reference avoid count of shared_ptr add.
			auto& nos = result[word];
			if (!nos) nos.reset(new std::set<LineNo>);
			nos->insert(lineNo);
		}
	}
}
 
QueryResult TextQuery::query(const string& str) const
{
	// use static just allocate once.
	static shared_ptr<std::set<LineNo>> nodate(new std::set<LineNo>);
	auto found = result.find(str);
	if (found == result.end())
		return QueryResult(str, nodate, input);
	else
		return QueryResult(str, found->second, input);
}
 
std::ostream& print(std::ostream& out, const QueryResult& qr)
{
	out << qr.word << " occurs " << qr.nos->size()
		<< (qr.nos->size() > 1 ? " times" : " time") << std::endl;
	for (auto i : *qr.nos)
		out << "\t(line " << i + 1 << ") " << qr.input->at(i) << std::endl;
	return out;
}
#endif
```
