---
title: JS async 与 wait 实验笔记
tags:
  - JS
categories:
  - JS
toc: false
date: 2020-04-27 17:30:26
---

## 条件
你需要先了解Promise的使用方式

## async
async是一个函数修饰符，使得这个函数返回的是promise,如：
```js
async function demo1(){
    return "message";
}

/*类似于*/
function demo2(){

    return new Promise(function(resolve,reject){
        //success
        resolve('message');
    });
}

//返回类型
console.log(typeof demo1()); //Promise { 'message' }
console.log(typeof demo2()); //object 

//调用
demo1().then(function(msg){

});
```
看到上面的typeof demo1()了吧，返回的是一个promise,所以他自然是可以调用then方法的

那么问题来了，这里的then方法何时触发，因为这里的then其实就是promise中调用了resolve,可是async的resolve是何时触发的呢，那就是这个函数执行完毕，就会触发，这个触发的过程是隐形的，代码不可见的。

那么then()回调里的msg是从哪里来的呢，这是来自原demo1函数里的返回值

```js
//==============无返回时的情况============

async function demo2(){

}
demo2().then(msg=>{console.log(msg);}); //打印结果：undefined


//==============返回字符时的情况============

async function demo3(){
    return "lalala";
}
demo3().then(msg=>{console.log(msg)});    //打印结果：lalala

//==============返回Promise时的情况============

async function demo4(){
    return new Promise(function(resolve,reject){
	resolve('hahaha');
    })
}
demo4().then(msg=>{console.log(msg)}); //打印结果：hahaha


//==============多个Promise时的情况============
async function demo5(){
    const p1 = new Promise(function(resolve,reject){
	resolve('aaaaa');
    });

    const p2 = new Promise(function(resolve,reject){
	resolve('bbbbb');
    })    

    return p1;
}
demo5().then(msg => console.log(msg)); //打印结果：aaaaa


//=========返回非promise但有then方法时的情况========
async function demo6(){
    return {
	then(callback){
    	    callback('cccc');
	}
    }
}
demo6().then(msg=>console.log(msg));    //打印结果：cccc
```
由上面的实验可以看出，如果async函数返回的是字符串，那么它将把字符串当做resolve的参数，如果返回是的一个promise，那么将用这个promise替换掉自己默认的promise，但是从最后一个实例看出来，返回的对象只要含有then方法，就可以覆盖async本身的then

## wait 

### await只能在async函数中使用

wait就是要等，什么情况下需要等呢，同步需要等吗，不需要，因为同步本来就是顺序执行的，wait的目的就是要将异步变为同步,wait等待的对象需要时异步执行的对象，同时wait也需要在异步环境中才有效，即async函数中
```js
//demo6().then(msg=>console.log(msg));
function demo7(){
    await new Promise(function(resolve,reject){
	setTimeout(() => {
		resolve('1秒钟后执行');
	}, 1000);
    })
}
demo7(); //报错：SyntaxError: await is only valid in async function


async function demo8(){
    return await new Promise(function(resolve,reject){
	setTimeout(() => {
	    resolve('1秒钟后执行');
	}, 1000);
    })
}
demo8();//打印结果：1秒钟后执行

async function demo9(){
	return await 5+5;
}
demo9().then(msg=>console.log(msg));//打印结果：10
```
由上可知，wait只能在async函数中运行，但是wait后面的等待是否是Promise并没有任何关系

### await返回的是异步执行后的结果，不再需要then

下面，来验证wait发生的作用是什么

```js

async function asyncSay(msg,ms){
    return new Promise(function(resolve,reject){
        setTimeout(function(){
            resolve(msg);
        },ms);
    });
}

async function demo10(){
    const res = asyncSay('say1',1000);
    console.log(res); 
    //Promise { <pending> }
}

async function demo10(){
    const res = await asyncSay('say1',1000);
    console.log(res); 
    //say1 
}
```
由上看出，当有await时，await作用的对象返回的是promise的resolve后的值，而没有的时候，则是promise本身，这就体现出来了，以前要获取异步的结果，需要调用then方法才行，现在就不需要了

```js

```

### wait后的方法是同步执行的
```js

async function asyncSay(msg,ms){
    return new Promise(function(resolve,reject){
        setTimeout(function(){
	    console.log(ms,'---',msg);
            resolve(msg);
        },ms);
    });
}

async function demo11(){
    asyncSay('msg1',2000);
    asyncSay('msg2',1000);
}
demo11();
/*
打印结果
1000 --- msg2
2000 --- msg1
*/


async function demo12(){
    asyncSay('msg1',2000);
    asyncSay('msg2',1000);
}
demo12();
/*
打印结果
2000 --- msg1
1000 --- msg2
*/
```
由上面的实验可知，加了wait的函数将把异步任务变成同步来执行，谁在前谁就先执行

### 另外一个奇怪的现象
```js
async function demo13(){
    new Promise((resolve,reject)=>{
        setTimeout(()=>{console.log(2000);},2000);
    });

    new Promise((resoleve,reject)=>{
        setTimeout(()=>{console.log(1000);},1000);
    });
}
demo13();
/*
打印结果
1000
2000
*/

async function demo14(){
    await new Promise((resolve,reject)=>{
        setTimeout(()=>{console.log(2000);},2000);
    });

    await new Promise((resoleve,reject)=>{
        setTimeout(()=>{console.log(1000);},1000);
    });
}
demo14();
/*
打印结果
2000
*/

```
上面这个加了await,只执行了第一个Promise，为什么呢？？？？