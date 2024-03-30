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