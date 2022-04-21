

# 格式化时间的工具函数，含测试


{yy} 年 年的后两位

{yyyy} 年 四位年

{M} 月 1到12月

{MM} 月 01到12月

{d} 日 1到31

{h}或{hh} 小时 00到23时

{m}或{mm} 分钟 00到59分

{s} 秒 0到59秒

{ss} 秒 00到59秒

如：{yyyy}-{MM}-{dd} {hh}:{mm}:{ss}

转换后：2017-01-01 00:00:00


```
const assert = require('assert');

function formate(fmt, date) {
  // 去掉 {2022} 两侧花括号
  fmt = fmt.replaceAll('{', '').replaceAll('}', '')
  var o = { 
    "M+" : date.getMonth()+1,     //月份 
    "d+" : date.getDate(),     //日 
    "h+" : date.getHours(),     //小时 
    "m+" : date.getMinutes(),     //分 
    "s+" : date.getSeconds(),     //秒 
    "q+" : Math.floor((date.getMonth()+3)/3), //季度 
    "S" : date.getMilliseconds()    //毫秒 
  };
  if(/(y+)/.test(fmt)) 
    fmt=fmt.replace(RegExp.$1, (date.getFullYear()+"").substr(4 - RegExp.$1.length)); 
  for(var k in o) 
    if(new RegExp("("+ k +")").test(fmt)) 
    fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length))); 
  return fmt; 
}


let testDate = '2021-02-03 09:01:03'

assert.equal(formate('{yyyy}-{MM}-{dd} {hh}:{mm}:{ss}', new Date(testDate)), '2021-02-03 09:01:03');
assert.equal(formate('{yy}-{MM}-{dd} {hh}:{mm}:{ss}', new Date(testDate)), '21-02-03 09:01:03');
assert.equal(formate('{yy}-{M}-{dd} {hh}:{mm}:{ss}', new Date(testDate)), '21-2-03 09:01:03');
assert.equal(formate('{yy}-{M}-{d} {hh}:{mm}:{ss}', new Date(testDate)), '21-2-3 09:01:03');
assert.equal(formate('{yy}-{M}-{d} {h}:{mm}:{ss}', new Date(testDate)), '21-2-3 9:01:03');
assert.equal(formate('{yy}-{M}-{d} {h}:{m}:{ss}', new Date(testDate)), '21-2-3 9:1:03');
assert.equal(formate('{yy}-{M}-{d} {h}:{m}:{s}', new Date(testDate)), '21-2-3 9:1:3');

```
