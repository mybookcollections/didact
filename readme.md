##

* fiber, 这个数据结构记录了所要做的(更新节点, update, deletion, creation) 等所有信息. 本身是一个树形的结构, 
  父节点只连接子节点位于数组的第一个, 但是所有子节点都有父节点的引用

  ```js
  let fiber = {
    tag: HOST_COMPONENT,  
    type: "div",  // fiber 类型
    parent: parentFiber, // parent fiber
    child: childFiber, // child fiber
    sibling: null, // slibing fiber
    alternate: currentFiber, // 指向当前未完成工作的 fiber, 以前的同一个位置节点的 fiber, 是为了记录 onUpdate(preProps, nextProps) 的 preProps 用的
    stateNode: document.createElement("div"), // either dom instance, or a (React)class component instance
    props: { children: [], className: "foo"}, // props, 当前工作(mutation) 需要更新的props
    partialState: null, // next state
    effectTag: PLACEMENT, // delete, add or update 
    effects: [] // other info related to effectTag, 子节点和孙子节点都会被pull 上来, 这里边只有root节点会真正用来操作dom, type of `fiber[]`
  };
  ```

* 理解 biber 的数据结构是非常重要的, fiber 有 child, parent, 各自指向 子节点 父节点, 而 sibling 可以理解为
  链表的下一个节点, 需要注意的是 child 和 parent 是双向的, 而 sibling 是单向的.



* wipFiber 和 rootFiber or _rootContainerFiber, fiber 的工作机制是 fiber 中记录了上次所有节点的关联信息, 然后在 effects 中记录了需要做的操作.
  当 wipFiber 中的 mutation 被操作完, wipFiber 就会变成 rootFiber or _rootContainerFiber, 然后下一次渲染的时候会生成一个新的 wipFiber, 然后
  这个 wipFiber.alternate 会被置为上次的 wipFiber 即 (rootFiber), 然后会将新的 elements 节点和老的 fiber 做个对比. 从根节点开始向下对比
  , 产生 creation, deletion or update 的 effectType. fiber 中也缓存了上次的 instance (React.Component) 或者 dom 实例 在 stateNode 中.






##

<p align="center"><img src="https://cloud.githubusercontent.com/assets/1911623/26426031/5176c348-40ad-11e7-9f1a-1e2f8840b562.jpeg"></p>

# Didact [![Build Status](https://travis-ci.org/hexacta/didact.svg?branch=master)](https://travis-ci.org/hexacta/didact) [![npm version](https://img.shields.io/npm/v/didact.svg?style=flat)](https://www.npmjs.com/package/didact)
 
> A DIY guide to build your own React

This repository goes together with a [series of posts](https://engineering.hexacta.com/didact-learning-how-react-works-by-building-it-from-scratch-51007984e5c5) that explains how to build React from scratch step by step.  


## Motivation

Didact's goal is to make React internals easier to understand by providing a simpler implementation of the same API and step-by-step instructions on how to build it. Once you understand how React works on the inside, using it will be easier. 

## Step-by-step guide

| Medium Post | Code sample | Commits | Other languages |
| --- | :---: | :---: | :---: |
| [Introduction](https://engineering.hexacta.com/didact-learning-how-react-works-by-building-it-from-scratch-51007984e5c5) |  |  |  |
| [Rendering DOM elements](https://engineering.hexacta.com/didact-rendering-dom-elements-91c9aa08323b) | [codepen](https://codepen.io/pomber/pen/eWbwBq?editors=0010) | [diff](https://github.com/hexacta/didact/commit/fc4d360d91a1e68f0442d39dbce5b9cca5a08f24) | [中文](https://github.com/chinanf-boy/didact-explain#1-%E6%B8%B2%E6%9F%93dom%E5%85%83%E7%B4%A0) |
| [Element creation and JSX](https://engineering.hexacta.com/didact-element-creation-and-jsx-d05171c55c56) | [codepen](https://codepen.io/pomber/pen/xdmoWE?editors=0010) | [diff](https://github.com/hexacta/didact/commit/15010f8e7b8b54841d1e2dd9eacf7b3c06b1a24b) | [中文](https://github.com/chinanf-boy/didact-explain#2-%E5%85%83%E7%B4%A0%E5%88%9B%E5%BB%BA%E5%92%8Cjsx) |
| [Virtual DOM and reconciliation](https://engineering.hexacta.com/didact-instances-reconciliation-and-virtual-dom-9316d650f1d0) | [codepen](https://codepen.io/pomber/pen/WjLqYW?editors=0010)  | [diff](https://github.com/hexacta/didact/commit/8eb7ffd6f5e210526fb4c274c4f60d609fe2f810) [diff](https://github.com/hexacta/didact/commit/6f5fdb7331ed77ba497fa5917d920eafe1f4c8dc) [diff](https://github.com/hexacta/didact/commit/35619a039d48171a6e6c53bd433ed049f2d718cb) | [中文](https://github.com/chinanf-boy/didact-explain#3-%E5%AE%9E%E4%BE%8B-%E5%AF%B9%E6%AF%94%E5%92%8C%E8%99%9A%E6%8B%9Fdom) |
| [Components and State](https://engineering.hexacta.com/didact-components-and-state-53ab4c900e37) | [codepen](https://codepen.io/pomber/pen/RVqBrx) | [diff](https://github.com/hexacta/didact/commit/2e290ff5c486b8a3f361abcbc6e36e2c21db30b8) | [中文](https://github.com/chinanf-boy/didact-explain#4-%E7%BB%84%E4%BB%B6%E5%92%8C%E7%8A%B6%E6%80%81) |
| [Fiber: Incremental reconciliation](https://engineering.hexacta.com/didact-fiber-incremental-reconciliation-b2fe028dcaec) | [codepen](https://codepen.io/pomber/pen/veVOdd) | [diff](https://github.com/hexacta/didact/commit/6174a2289e69895acd8fc85abdc3aaff1ded9011) [diff](https://github.com/hexacta/didact/commit/accafb81e116a0569f8b7d70e5b233e14af999ad) | [中文](https://github.com/chinanf-boy/didact-explain#5-fibre-%E9%80%92%E5%A2%9E%E5%AF%B9%E6%AF%94) |

## Usage
> 🚧 Don't use this for production code!

Install it with npm or yarn:  

```
$ yarn add didact
```

And then use it like you use React:  

```jsx
/** @jsx Didact.createElement */
import Didact from 'didact';

class HelloMessage extends Didact.Component {

  constructor(props) {
    super(props);
    this.state = {
      count: 1
    };
  }

  handleClick() {
    this.setState({
      count: this.state.count + 1
    });
  }

  render() {
    const name = this.props.name;
    const times = this.state.count;
    return (
      <div onClick={e => this.handleClick()}>
        Hello {name + "!".repeat(times)}
      </div>
    );
  }
}

Didact.render(
  <HelloMessage name="John" />,
  document.getElementById('container')
);
```

## Demos
[hello-world](https://rawgit.com/hexacta/didact/master/examples/hello-world/index.html)  
[hello-world-jsx](https://rawgit.com/hexacta/didact/master/examples/hello-world-jsx/index.html)  
[todomvc](https://didact-todomvc.surge.sh)  
[incremental-rendering-demo](https://pomber.github.io/incremental-rendering-demo/didact.html)

## License

MIT © [Hexacta](https://www.hexacta.com)
