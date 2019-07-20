##  使用

```javascript

--- 例子
async function fetch() {
  const data = await fetchData()
  if (data.moreData) {
    const moreData = await fetchAnotherData(data);
    return moreData
  } else {
    return data
  }
}


--- 错误处理

async function fetch() {
  try {
    const data = JSON.parse(await fetchData())
  } catch (err) {
    console.log(err)
  }
};
--- 继发 --按顺序执行
async function aaa(){
  let a = await some1()
  let b = await some2()
//这样的写法是继发的 相当于A执行完毕后再执行B
}


---  并发-异步执行多个方法 -无先后顺序
(async () => {
  const listPromise = getList();
  const anotherListPromise = getAnotherList();

  await listPromise;
  await anotherListPromise;
})();

async function loadData() {
  var [res,res2,res3] = await Promise.all([fetch(url1), fetch(url2), fetch(url3)]);
  return "whew all done";
}

```