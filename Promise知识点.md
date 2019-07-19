## 语法
new Promise( function(resolve, reject) {...} /* executor */  )  异步函数


function timeoutPromise(delay) {
    return new Promise( function(resolve,reject){
      resolve( "Timeout!" ); //返回 then(e)
      reject( "Timeout!" ); //返回 catch(e)
    } );
}

timeoutPromise().then(res =>{
  // ...
}).catch(e =>{
  // ...
})


