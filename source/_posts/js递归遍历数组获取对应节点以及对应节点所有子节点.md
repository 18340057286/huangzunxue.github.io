---
title: js递归遍历数组获取对应节点以及对应节点所有子节点
date: 2023-03-03 09:42:08
tags: js
---
```
let  treeList = [
 
        {
 
          id: '1',
 
          name: '父一',
 
          children: [
 
            {
 
              id: '1-1',
 
              name: '子一一',
 
              children: [
 
                { id: '1-1-1', name: '孙一一', children: null },
 
                { id: '1-1-2', name: '孙一二', children: null },
 
                { id: '1-1-3', name: '孙一三', children: null }
 
              ]
 
            }
 
          ]
 
        },
 
        {
 
          id: '2',
 
          name: '父二',
 
          children: [
 
            { id: '2-1', name: '子二一', children: null },
 
            { id: '2-2', name: '子二一', children: null },
 
            { id: '2-3', name: '子二一', children: null }
 
          ]
 
        },
 
        {
 
          id: '3',
 
          name: '父三',
 
          children: null
 
        }
 
      ]
01.根据id查询id所对应的对象

function getObjById(list,id){
//判断list是否是数组
if(!list instanceof Array){
  return  null
}
//遍历数组
for(let i in list){
  let item=list[i]
  //此处的值可修改为与自己数组所匹配的值
  if(item.id===id)
  {
    return item
  }else{
     //查不到继续遍历
      if(item.children){
       let value=getObjById(item.children,id)
     //查询到直接返回
       if(value){
           return value
         }
    }
   
  }
 
 }
 
}
//测试
console.log(getObjById(treeList,"1-1-3"))
结果： { id: '1-1-3', name: '孙一三', children: null }  


2.根据id查询本节点和所有父级节点
 
function  getParentsById(list,id){
    for (let i in list) {
        if (list[i].id === id) {
 
           //查询到就返回该数组对象
           return [list[i]]
        }
 
        if (list[i].children) {
 
          let node = getParentsById(list[i].children, id)
          if (node !== undefined) {
              //查询到把父节点连起来
            return node.concat(list[i])
          }
        }
     }    
}
 
console.log(getParentsById(treeList,'2-3'))
结果： [{id: "2-3", name: "子二一", children: null}, {id: "2", name: "父二", children: Array(3)}]

3.根据id查询该节点和所有子节点

//需要用到上面的根据id查询该节点对象
function getObjById(list,id){
//判断list是否是数组
if(!list instanceof Array){
  return  null
}
//遍历数组
for(let i in list){
  let item=list[i]
  //此处的值可修改为与自己数组所匹配的值
  if(item.id===id)
  {
    return item
  }else{
     //查不到继续遍历
      if(item.children){
       let value=getObjById(item.children,id)
     //查询到直接返回
       if(value){
           return value
         }
    }
   
  }
 
 }
 
}
 
 
 
 // list 为已查询到的节点children数组，returnvalue为返回值（不必填）
function  getChildren (list,returnValue=[]) {
     for(let i in list){
      //把元素都存入returnValue
      returnValue.push(list[i])
      if (list[i].children) {
          getChildren(list[i].children, returnValue)
      }
     }
     return returnValue
 }
 
 
//age:
let obj=getObjById(treeList,"1")
if(obj&&obj.children){
   let childrenList=getChildren(obj.children)
   console.log(childrenList)
  } else {
    console.log("没有该节点或者没有子元素")
  }

结果为：[ {id: "1-1", name: "子一一", children: Array(3)},

     {id: "1-1-1", name: "孙一一", children: null},

    {id: "1-1-2", name: "孙一二", children: null},

    {id: "1-1-3", name: "孙一三", children: null}]
```

转载自大佬爱吃蛋炒饭加蛋
原文链接：[https://blog.csdn.net/qq_27104997/article/details/103617219](https://blog.csdn.net/qq_27104997/article/details/103617219)