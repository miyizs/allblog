---
title: indexDB学习
date: 2017-12-10 09:59:44
tags:
- indexDB
- 翻译
categories:
- 学习
---
前两天在MDN上看到的indexDB的内容，还没有人翻译，自己看的时候翻译了一下，因为英文水平也不行，所以不敢申请发到MDN上，不过翻译下来也算自己的学习材料吧

IDBCursor 是 IndexedDB API中的一个接口，用于遍历和迭代数据库中多条记录的指针。这个指针指向了当前遍历到的那个对象存储空间或者序号。这个指针的位置是在一定范围内变动的，根据记录的key值进行升序或者降序的移动。这个指针允许应用在指针范围内异步的处理所有的记录。

你可以同时拥有无限个指针。一个给定的指针永远指向同一个IDBCursor对象，操作被执行于强调出的那个序号或者对象存储空间上。

方法
IDBCursor.advance
设置指针应该向前移动的次数

IDBCursor.continue
使指针移动到key所匹配的那个位置

IDBCursor.delete
返回一个IDBRequest对象并且独立的删除某个指针所在的位置不改变其他指针的位置，被用于删除特定的记录

IDBCursor.update
返回一个IDBRequest对象，并且独立的更新对象存储空间中指针所在的当前位置。这被用于更新某条特定的记录

属性
IDBCursor.source
返回当前遍历到的IDBObjectStore或者IDBIndex，即使是在指针被反复申明或者迭代到了最后或者事务未被激活的状态下，这个方法从不返回null或者抛出异常。

IDBCursor.direction
返回指针遍历的方向

IDBCursor.key
返回指针所在位置所对应的那条记录的key，如果指针在他的范围之外则设置为undefined，指针的key值可以是任意的数据类型

IDBCursor.primaryKey
返回指针当前的主键。如果指针当前被重复申明或者在范围之外被遍历那么设置为undefined。指针的主键可以是任意的数据类型

示例
在这个例子中我们创建了一个事务，检索了对象存储空间，之后运用指针遍历了对象存储空间中的所有记录，这个指针不需要我们基于key值来检索数据，我们可以抓取所有的数据，并且注意，在每一次循环的遍历中，你都可以从当前记录指针对象下的cursor.value.foo中抓取数据
```javascript
@requires_authorization
function displayData() {
  var transaction = db.transaction(['rushAlbumList'], "readonly");
  var objectStore = transaction.objectStore('rushAlbumList');

  objectStore.openCursor().onsuccess = function(event) {
    var cursor = event.target.result;
    if(cursor) {
      var listItem = document.createElement('li');
      listItem.innerHTML = cursor.value.albumTitle + ', ' + cursor.value.year;
      list.appendChild(listItem);  

      cursor.continue();
    } else {
      console.log('Entries all displayed.');
    }
  };
}
```
[原文地址](https://developer.mozilla.org/en-US/docs/Web/API/IDBCursor)
