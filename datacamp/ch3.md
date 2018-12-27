
JSON 格式常用于 Web
的数据传输

一般有两种格式，参考[DataCamp](https://campus.datacamp.com/courses/working-with-web-data-in-r/handling-json-and-xml?ex=1)

1.  objects: `{"title" : "A New Hope", "year" : "1977"}`
2.  arrays: `[1977, 1980]`

`http_type`帮助知道反馈的值是不是json格式。

``` eval
[{ 
   "first_name": "Jason",
   "last_name": "Bourne",
   "occupation": "Spy"
 },
{
  "first_name": "Jason",
  "last_name": "Voorhees",
  "occupation": "Mass murderer"
}]
```
