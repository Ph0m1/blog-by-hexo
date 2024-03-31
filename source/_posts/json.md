---
title: 使用C++实现JSON解析
date: 2024-03-30 18:27:30
---

# JSON格式介绍

   JSON(JavaScript Object Notation)，是一种序列化的格式，最大的优点在于可读性极强，以及可以直接嵌入到 JS 代码中，所以广泛运用于 web 数据的收发

   JSON 格式有以下基本类型：

   - null 类型：值为null，表示为空
   - bool 类型：值为 true 和 false
   - number 类型：值为 int、double 
   - string 类型：形如 "LinuxGroup"

   除此之外，还有以下复合类型：

   - list 类型（也称 array 类型）
      ``` json
      ["abc",3.2,323,"def"] 
      ```
   - dict 类型（也称 object 类型）
      ```json
      {
            "id": 32,
            "name": "hhh"
      }
      ```

# 解析JSON字符串

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;www.iodraw.com\&quot; modified=\&quot;2024-03-30T14:50:57.742Z\&quot; agent=\&quot;5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36\&quot; etag=\&quot;3vroEbg5rb9oIm9rTXd7\&quot; version=\&quot;13.3.6\&quot; type=\&quot;device\&quot;&gt;&lt;diagram id=\&quot;daFsWAjA5bF-lV7y9AyR\&quot; name=\&quot;第 1 页\&quot;&gt;7Vlbc9soFP41zOw+bEdIQpdH2Va6bdM2M37o7lOHSsRSVxYejGM7v35BAt1QbmvHTpqdyUzg4wDi+w6HAwbOdLl7z/Aq+0xTUgDbSnfAmQHbhoHtiX8S2SvEsqwaWbA8VVgLzPNbog0VuslTsu4ZckoLnq/6YELLkiS8h2HG6LZvdk2L/qwrvCAGME9wYaLf8pRnNRrYfov/SfJFpmeGXli3LLE2VitZZzil2w7kxMCZMkp5XVrupqSQ7Gle6n4Xd7Q2H8ZIyUc60B8/JR+2VeAfQpTKoO6iR7hNLqf2LfvuOZcfP32I4qsoyv4Imy9rZlzzvWaDpIIcVaWMZ3RBS1zELTphdFOmRM5giVprc0npSoBQgD8J53ulNN5wKqCMLwvVek1LrhqhJ+r1N8uJ71y3gtZ0wxJyz9rU+jlmC8LvsQs6HLwndEk424t+jBSY5zf978DKvxaNneoaMYb3HYMVzUu+7ox8JQFhoPaKHSo/UTvFRqiv/wP2EPXkF4X6C3Sts5QWqhTWVeUx4+42SpTepOf1ELLL+V9qOFn+W9nJ8mzXaZjtVeXYPnWorxjiQr8vrqODiB6i9mHVq1X9qU7nwqc53dDesax77Y11WAc56SO9Up05N7jYkG7ge3OBDKIDvbMj1H/QIXiROgi22b6JF7IiA4b1DulqGzOq2nMFjcfo55/jJHKh09+0HgLPvmltw1c+flUZzMBlWoeQ6m6znJP5Cld8b0US+phNeEMYJ7v7ZTRZ32k6+vSIoFbXt21+CHWWm3VyQ9+6W6jDNppvkjf/+gXECEQzEPog9kE0BZEHYhdMAjAx9+G5SUXB4KAYIxWelFT0+kkdnL5Qr6lLqn1KUkdOBMneRP7FHghcEFxIYiewQhAIYxBMXhyxEL40Yt1fk9gm732IWO/ZwkA4zqzY+ZbkMRDhAEmKQwQmVsX1DESxwazghPfpW3NG/yFTWlAmkJKWMtu5zotiAOEiX5SimggOicAnkuE8wUWkGpZ5mlap0phe/fTpGSRr3jf0cRiYkgUjig1vNEdTTC/hTsXELoherWKMcpH6UTlKKNszyvJbYYOLXpp6mKTeILydXVLHkJTT70KOvFz89vsrUe4IwriD1NNxTWHckwpjnjtXmK0ldZH4u2B0Oa9UejsaoWE8PLtGj3gOSTbspjn2SZlG8uVcclzg9TpPjvOwYVLWoQSNUKKxAx/TPHcQzqwB1fV93XhMM1+z0AMDHelVzhnk7e4JHs1s8w75trwEDg695qnzqV4ydDdjoGO93Q7n8U/hJmM3OCQvFyKJEgWRWcmLhkCC6o4sClMQijuyuCBfyNRL3pqDsavHL3sgOHb4DvWUQuaJAD3/lEfC2KXmf/HGxBtusjHt4HG0E9X21+B6w7Y/qjvxvw==&lt;/diagram&gt;&lt;/mxfile&gt;&quot;}"></div>
<script type="text/javascript" src="https://www.iodraw.com/diagram/js/viewer.min.js"></script>

# 创建JObject类
   我们需要把json的类型对应到计算机语言的类型。

   由于json的数据在我们看来都是字符串，那么有如下对应关系：

   - "null"对应我们构造的null类型
   - “true","false"对应内部的bool类型即可
   - number类型数据对应int、double类型
   - string类型数据对应string即可
   - list类型对应C++中的vector
   - dict类型对应C++中的map或unordered_map

   在C++中我们通过std::variant来进行，还需要一个枚举tag来表示当前对象内存储的数据类型。
   
   当然如果做的更绝的话，可以通过一个void* + 申请堆内存来解决，然后再强转为对应类型来操作。
   
   对应的代码如下：(中间的类方法就暂时省略了)

   ```c++
   enum TYPE
   {
    T_NULL,
    T_BOOL,
    T_INT,
    T_DOUBLE,
    T_STR,
    T_LIST,
    T_DICT
   };

   using null_t = string;
   using int_t = int32_t;
   using bool_t = bool;
   using double_t = double;
   using str_t = string;
   using list_t = vector<JObject>;
   using dict_t = map<string, JObject>;//底层实现是红黑树
   
   class JObject
   {
   public:
       using value_t = variant<bool_t, int_t, double_t, str_t, list_t, dict_t>;
       ...
   private:
       TYPE m_type;
       value_t m_value;
   };


   ```

# 创建Parser类

   我们有了JObject，可以把所有的JSON数据接收起来，现在要做的就是扫描JSON字符串，对其中的数据进行读取处理，然后转化为JObject。

   关键代码如下：

   ```c++
   JObject Parser::parse()
   {
       char token = get_next_token();
       if (token == 'n')
       {
           return parse_null();
       }
       if (token == 't' || token == 'f')
       {
           return parse_bool();
       }
       if (token == '-' || std::isdigit(token))
       {
           return parse_number();
       }
       if (token == '\"')
       {
           return parse_string();
       }
       if (token == '[')
       {
           return parse_list();
       }
       if (token == '{')
       {
           return parse_dict();
       }
   
       throw std::logic_error("unexpected character in parse json");
   }

   ```
   ```
   以上就是整个字符串的解析过程，每次通过get_next_token这个方法得到整个字符串的下一个token，根据token决定解析对应的数据类型。
   ```

## get_next_token方法
   
   跳过空白符号，以及跳过注释（标准的JSON格式不支持注释，为了vscode的JSON格式配置文件解析）

   ```c++
   
   char Parser::get_next_token()
   {
       while (std::isspace(m_str[m_idx])) m_idx++;
       if (m_idx >= m_str.size())
           throw std::logic_error("unexpected character in parse json");
       //如果是注释，记得跳过
       skip_comment();
       return m_str[m_idx];
   }
   ```
## parse_null和parse_bool

   - parse_null
   
   ```c++
   bool Parser::parse_null()
   {
      if (m_str.compare(m_idx, 4, "null") == 0)
      {
        m_idx += 4;
        return {};
      }
      throw std::logic_error("parse null error");
   }
   ```

   - parse_bool
   
   ```c++
   bool Parser::parse_bool()
   {
       if (m_str.compare(m_idx, 4, "true") == 0)
       {
           m_idx += 4;
           return "true";
       }
       if (m_str.compare(m_idx, 5, "false") == 0)
       {
           m_idx += 5;
           return "false";
       }
       throw std::logic_error("parse bool error");
   }

   ```

## parse_number

   ```c++
   JObject Parser::parse_number()
   {
       auto pos = m_idx;
       //整数部分
       if (m_str[m_idx] == '-')
       {
           m_idx++;
       }
       if (isdigit(m_str[m_idx]))
           while (isdigit(m_str[m_idx]))
               m_idx++;
       else
       {
           throw std::logic_error("invalid character in number");
       }

       if (m_str[m_idx] != '.')
       {
           return (int) strtol(m_str.c_str() + pos, nullptr, 10);
       }

       //小数部分
       if (m_str[m_idx] == '.')
       {
           m_idx++;
           if (!std::isdigit(m_str[m_idx]))
           {
               throw std::logic_error("at least one digit required in parse float part!");
           }
           while (std::isdigit(m_str[m_idx]))
               m_idx++;
       }
       return strtof64(m_str.c_str() + pos, nullptr);
   }

   ```

## parse_list

   ```c++
   JObject Parser::parse_list()
   {
       JObject arr((list_t()));//得到list类型的JObject
       m_idx++;
       char ch = get_next_token();
       if (ch == ']')
       {
           m_idx++;
           return arr;
       }

       while (true)
       {
           arr.push_back(parse());
           ch = get_next_token();
           if (ch == ']')
           {
               m_idx++;
               break;
           }

           if (ch != ',') //如果不是逗号
           {
               throw std::logic_error("expected ',' in parse list");
           }
           //跳过逗号
           m_idx++;
       }
       return arr;
   }
   ```

## parse_dict
   
   ```c++
   JObject Parser::parse_dict()
   {
       JObject dict((dict_t()));//得到dict类型的JObject
       m_idx++;
       char ch = get_next_token();
       if (ch == '}')
       {
           m_idx++;
           return dict;
       }
       while (true)
       {
           //解析key
           string key = std::move(parse().Value<string>());
           ch = get_next_token();
           if (ch != ':')
           {
               throw std::logic_error("expected ':' in parse dict");
           }
           m_idx++;

           //解析value
           dict[key] = parse();
           ch = get_next_token();
           if (ch == '}')
           {
               m_idx++;
               break; //解析完毕
           }
           if (ch != ',')//没有结束，此时必须为逗号
           {
               throw std::logic_error("expected ',' in parse dict");
           }
           //跳过逗号
           m_idx++;
       }
       return dict;
   }

   ```

# 完善Object类

   很明显，我们需要为JObject类提供一个方法，此方法可以让调用者直接访问到std::variant里面对应的数据，并且我们也需要提供一个方法能让JObject快速初始化为对应的类型。

## 初始化接口


   ```c++
   void Null()
   {
       m_type = T_NULL;
       m_value = "null";
   }

   void Int(int_t value)
   {
       m_value = value;
       m_type = T_INT;
   }

   void Bool(bool_t value)
   {
       m_value = value;
       m_type = T_BOOL;
   }

   void Double(double_t value)
   {
       m_type = T_DOUBLE;
       m_value = value;
   }

   void Str(string_view value)
   {
       m_value = string(value);
       m_type = T_STR;
   }

   void List(list_t value)
   {
       m_value = std::move(value);
       m_type = T_LIST;
   }

   void Dict(dict_t value)
   {
       m_value = std::move(value);
       m_type = T_DICT;
   }

   ```

   为了方便平时的赋值时候的隐式转化，我们应该再添加对应的构造函数，如下：（隐式转化在C++里有个坑，只能为类提供一种方向的隐式转化，比如提供了int把转为JObject的隐式转化后，就不能再提供把JObject转为int的隐式转化了，这两种必须要有一个是explicit，否则会报错）

   ```c++
   JObject()//默认为null类型
   {
       m_type = T_NULL;
       m_value = "null";
   }

   JObject(int_t value)
   {
       Int(value);
   }

   JObject(bool_t value)
   {
       Bool(value);
   }

   JObject(double_t value)
   {
       Double(value);
   }

   JObject(str_t const &value)
   {
       Str(value);
   }

   JObject(list_t value)
   {
       List(std::move(value));
   }

   JObject(dict_t value)
   {
       Dict(std::move(value));
   }

   ```

## 请求值接口

    设计思路：通过提供一个Value方法，该方法为泛型，其内部有调用value()方法得到对应的数据指针，而Value方法则负责将指针强转

    value方法如下，用于调用get_if得到对应的数据指针，关于怎么获取std::variant的数据，有get和get_if两种方式，get得到的是对象的引用，如果获取不到，则抛出异常，get_if获取对象的指针，如果获取不到则返回nullptr

   ```c++
   void *JObject::value()
   {
       switch (m_type)
       {
           case T_NULL:
               return get_if<str_t>(&m_value);
           case T_BOOL:
               return get_if<bool_t>(&m_value);
           case T_INT:
               return get_if<int_t>(&m_value);
           case T_DOUBLE:
               return get_if<double_t>(&m_value);
           case T_LIST:
               return get_if<list_t>(&m_value);
           case T_DICT:
               return get_if<dict_t>(&m_value);
           case T_STR:
               return std::get_if<str_t>(&m_value);
           default:
               return nullptr;
       }
   }  
   ```   

   - Value方法：
   ```c++
   #define THROW_GET_ERROR(erron) throw std::logic_error("type error in get "#erron" value!")

   template<class V>
       V &Value()
   {
       //添加安全检查
       if constexpr(IS_TYPE(V, str_t))
       {
           if (m_type != T_STR)
               THROW_GET_ERROR(string);
       } else if constexpr(IS_TYPE(V, bool_t))
       {
           if (m_type != T_BOOL)
               THROW_GET_ERROR(BOOL);
       } else if constexpr(IS_TYPE(V, int_t))
       {
           if (m_type != T_INT)
               THROW_GET_ERROR(INT);
       } else if constexpr(IS_TYPE(V, double_t))
       {
           if (m_type != T_DOUBLE)
               THROW_GET_ERROR(DOUBLE);
       } else if constexpr(IS_TYPE(V, list_t))
       {
           if (m_type != T_LIST)
               THROW_GET_ERROR(LIST);
       } else if constexpr(IS_TYPE(V, dict_t))
       {
           if (m_type != T_DICT)
               THROW_GET_ERROR(DICT);
       }

       void *v = value();
       if (v == nullptr)
           throw std::logic_error("unknown type in JObject::Value()");
       return *((V *) v); //强转
   }

   ```

## 重载方法

   当JObject为dict类型时，我们可以直接用下标运算符进行key-value的赋值（得益于隐式转化和运算符重载）
   
   ```c++
   JObject &operator[](string const &key)
   {
       if (m_type == T_DICT)
       {
           auto &dict = Value<dict_t>();
           return dict[key];
       }
       throw std::logic_error("not dict type! JObject::opertor[]()");
   }
   ```
   
   同样如果为list对象，我们也准备了push_back等方法

   ```c++
   void push_back(JObject item)
   {
       if (m_type == T_LIST)
       {
           auto &list = Value<list_t>();
           list.push_back(std::move(item));
           return;
       }
       throw std::logic_error("not a list type! JObjcct::push_back()");
   }
   ```

# 完善Parser类

   我们之前是完成了整个字符串到JObject的解析过程，但是每次都需要创建一个Parser类，然后调用方法，这样的过程未免有些繁琐，我们可以对外提供FromString的静态方法，然后充分利用一个对象便可完成整个解析过程。

   ```c++
   JObject Parser::FromString(string_view content)
   {
       static Parser instance;
       instance.init(content);
       return instance.parse();
   }
   ```

# 完成序列化和反序列化过程

   我们前面所做的工作其实已经把这个过程相当于完成了，当然还差一个把JObject变为JSON字符串的方法，现在添加上就是，也不是很难，按照相似的逻辑反推一遍就行。如下给JObject添加一个to_string方法：

   ```c++
   string JObject::to_string()
   {
       void *value = this->value();
       std::ostringstream os;
       switch (m_type)
       {
           case T_NULL:
               os << "null";
               break;
           case T_BOOL:
               if (GET_VALUE(bool))
                   os << "true";
               else os << "false";
               break;
           case T_INT:
               os << GET_VALUE(int);
               break;
           case T_DOUBLE:
               os << GET_VALUE(double);
               break;
           case T_STR:
               os << '\"' << GET_VALUE(string) << '\"';
               break;
           case T_LIST:
           {
               list_t &list = GET_VALUE(list_t);
               os << '[';
               for (auto i = 0; i < list.size(); i++)
               {
                   if (i != list.size() - 1)
                   {
                       os << ((list[i]).to_string());
                       os << ',';
                   } else os << ((list[i]).to_string());
               }
               os << ']';
               break;
           }
           case T_DICT:
           {
               dict_t &dict = GET_VALUE(dict_t);
               os << '{';
               for (auto it = dict.begin(); it != dict.end(); ++it)
               {
                   if (it != dict.begin()) //为了保证最后的json格式正确
                       os << ',';
                   os << '\"' << it->first << "\":" << it->second.to_string();
               }
               os << '}';
               break;
           }
           default:
               return "";
       }
       return os.str();
   }

   ```

   有关JObject的方法也都补充的差不多了，那么我们现在要考虑的是如何通过JObject这个中间对象将我们自定义的任何一个类给序列化和反序列化？

## 序列化接口设计

   在Parser类中添加一个ToJSON的静态方法，用来对任意类型进行序列化，这个方法使用模板。

   ```c++
   template<class T>
       static string ToJSON(T const &src)
   {
       //如果是基本类型
       if constexpr(IS_TYPE(T, int_t))
       {
           JObject object(src);
           return object.to_string();
       } else if constexpr(IS_TYPE(T, bool_t))
       {
           JObject object(src);
           return object.to_string();
       } else if constexpr(IS_TYPE(T, double_t))
       {
           JObject object(src);
           return object.to_string();
       } else if constexpr(IS_TYPE(T, str_t))
       {
           JObject object(src);
           return object.to_string();
       }
       //如果是自定义类型调用方法完成dict的赋值，然后to_string即可
       json::JObject obj((json::dict_t()));
       src.FUNC_TO_NAME(obj);
       return obj.to_string();
   }

   ```

   如上代码如果是基本类型则直接初始化JObject调用to_string方法，如果是自定义类型，则需要在该类型中嵌入一个方法，这个方法名字我们用FUNC_TO_NAME这个宏代替。

   也就是说，如果是自定义的类型，那么肯定就是对应JSON数据的dict类型，所以只需要你对该类型定义对应的方法，该方法需要将值传递给JObject，为了简化这个过程我们用宏来替代。
   ```c++
   #define FUNC_TO_NAME _to_json
   #define FUNC_FROM_NAME _from_json

   #define START_TO_JSON                           \
       void FUNC_TO_NAME(json::JObject &obj) const \
       {
   #define to(key) obj[key]
   // push一个自定义类型的成员
   #define to_struct(key, struct_member)    \
       json::JObject tmp((json::dict_t())); \
       struct_member.FUNC_TO_NAME(tmp);     \
       obj[key] = tmp
   #define END_TO_JSON }

   #define START_FROM_JSON                     \
       void FUNC_FROM_NAME(json::JObject &obj) \
       {
   #define from(key, type) obj[key].Value<type>()
   #define from_struct(key, struct_member) struct_member.FUNC_FROM_NAME(obj[key])
   #define END_FROM_JSON }

   ```

   基本使用如下：

   ```c++
   struct Base
   {
       int pp;
       string qq;

       START_FROM_JSON //生成反序列化相关的方法
           pp = from("pp", int);
           qq = from("qq", string);
       END_FROM_JSON

       START_TO_JSON //生成序列化相关的代码
           to("pp") = pp;
           to("qq") = qq;
       END_TO_JSON
   };

   struct Mytest
   {
       int id;
       std::string name;
       Base q;

       START_TO_JSON	//序列化相关代码
           to_struct("base", q);
           to("id") = id;
           to("name") = name;
       END_TO_JSON

       START_FROM_JSON //反序列化相关代码
           id = from("id", int);
           name = from("name", string);
           from_struct("base", q);
       END_FROM_JSON
   };
   ```

   实际上上面代码等效于下面的代码，以Base类为例：

   ```c++
   struct Base
   {
       int pp;
       string qq;
   
       void _from_json(json::JObject& obj){ //反序列化
           pp = obj.Value<int>();
           qq = obj.Value<string>();
       }
       
       void _to_json(json::JObject& obj){//序列化代码
           obj["pp"] = pp;
           obj["qq"] = qq;
       }
   };
   
   ```