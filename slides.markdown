class: center, middle

## Future of JavaScript

김남훈<br>
FO4M

---

## TL;DR

- ECMAScript 표준은 계속 진화중
- babel; 최신 표준을 즉시 사용할 수 있게 해주는 도구
- AST 를 사용해서 굉장히 많은 일을 할 수 있다
- AST 변환을 fo4 에서 쓰진 말도록 하자

---

class: center, middle

![](https://raw.githubusercontent.com/babel/logo/master/babel.png)

_The compiler for writing next generation JavaScript_

???

- [Sebastian McKenzie](https://github.com/kittens)가 2014에 시작
- 6to5
- founder 는 facebook 으로 이직 성공 ^^

---

## babel

- es6 코드를 es5 로 변환시켜주는 도구
  - parse
  - map plugins to ast
  - write

???

- babel 로 이름을 바꾸며 주요 툴로 자리잡았고, tc39 와도 많은 접점
- react (jsx), flow, vue 등 유우명 프로젝트에서 활발히 사용 중

---

## ECMAScript 표준화 현황

- tc39
- stages
  - stage0; Strawman
  - stage1; Proposal
  - stage2; Draft
  - stage3; Candidate
  - stage4; Finished

---

## stage0 - `const` class

```javascript
const class Point {
  constructor(x, y) {
    public getX() { return x; }
    public getY() { return y; }
  }
  toString() {
    return '<' + this.getX() + ',' + this.getY() + '>';
  }
}
```

---

## stage0 - decimal

```javascript
0.1 + 0.2
// >> 0.30000000000000004

0.1m + 0.2m
// >> 0.3m
```

---

## stage3 - Private instance methods

```javascript
class Counter extends HTMLElement {
  #xValue = 0;

  #clicked() {
    this.#xValue++;
  }

  constructor() {
    super();
    this.onclick = this.#clicked.bind(this);
  }
}
```

---

## stage4 - Optional `catch` binding

```javascript
try {
  // ...
} catch {
  // ...
}
```

---

## why babel?

- js 세계에서 parser, transpiler 의 표준이 되고자 하는 시도

???

- [Not Born to Die](https://babeljs.io/blog/2015/02/15/not-born-to-die) 문서에서 말하듯 야심있는 프로젝트
- parser, transpiler (and compiler)
- ESTree

--
- 였지만 잘 되진 않았다고 한다.

???

- eslint, uglify, typescript 등 많은 프로젝트가 각기 다른 js parser 사용
- founder 가 재직중인 fb 프로젝트에서는 열심히 사용중

--
- 그래도 es6 to es5 분야에서만큼은 표준.

???

- AST 변환의 컨셉
- lisp macro 와의 유사성

---

## AST

- compiler frontend 의 결과물
- 문법 구조를 반영한 트리 + 메타데이터

---

class: center, middle

# _Demo_

[AST Explorer](https://astexplorer.net/)

---

class: center, middle

# _Yet Another Demo_

[Babel; Try it out](https://babeljs.io/repl)

---

class: center, middle

# Inside of Babel

---

## basic operation

- parse
- map plugins to ast
- write

???

- visitor pattern

---

## stage2 - `throw` expressions

```javascript
// Parameter initializers
function save(filename = throw new TypeError("Argument required")) {
}

// Arrow function bodies
lint(ast, {
  with: () => throw new Error("avoid using 'with' statements.")
});
```

---

```javascript
import syntaxThrowExpressions from "@babel/plugin-syntax-throw-expressions";

export default declare(api => {
  return {
    name: "proposal-throw-expressions",
    inherits: syntaxThrowExpressions,

    visitor: {
      UnaryExpression(path) {
        const { operator, argument } = path.node;
        if (operator !== "throw") return;

        const arrow = t.functionExpression(
          null,
          [t.identifier("e")],
          t.blockStatement([t.throwStatement(t.identifier("e"))]),
        );

        path.replaceWith(t.callExpression(arrow, [argument]));
      },
    },
  };
});
```

---

## result

```javascript
// future
function save(filename = throw new TypeError("Argument required")) {
}
```

```javascript
// present
"use strict";

function save() {
  var filename = arguments.length > 0 && arguments[0] !== undefined ?
    arguments[0] : function (e) {
      throw e;
    }(new TypeError("Argument required"));
}
```

---

class: center, middle

# Creating a custom plugin

---

## spec

- perl 처럼 함수의 마지막 표현식이 자동으로 리턴되었으면 좋겠다.

```javascript
function foo() {
  42;
}

console.log(foo())
// >> 42
```

---

```javascript
export default declare(api => {
  api.assertVersion(7);

  return {
    name: "auto-return",

    visitor: {
      FunctionDeclaration(path) {
        const { body: block } = path.node;
        const last = block.body[block.body.length - 1];
        if (!last) return;

        path.traverse({
          ExpressionStatement(path) {
            const statement = t.returnStatement(path.node.expression);
            path.replaceWith(statement);
          },
        }, last);
      }
    },
  };
});
```

---

## result

```bash
$ cat foo.js
function foo() {
  42;
}

console.log(foo());

$ npx babel foo.js
"use strict";

function foo() {
  return 42;
}

console.log(foo());
```
[http://git.sphd.io/nakim/babel-plugin-auto-return](http://git.sphd.io/nakim/babel-plugin-auto-return)

---

## iu 쓸까말까 babel

- es6 to ActionScript 만들 수 있다.
- 하지만 es6 to es5 용도로만 사용하도록 하자.

???

- 강력한 도구이지만 양날의 검.
- export default 버그는 많은 사람을 고통에 빠뜨렸음
- transform 에서 버그가 생겼을 경우 굉장히 힘든 시간을 보낼 것.

---

## References

- [The State of Babel][1]
- [Not Born to Die][2]
- [Babel Plugin Handbook][3]
- [ECMAScript proposals][4]

[1]: https://babeljs.io/blog/2016/12/07/the-state-of-babel
[2]: https://babeljs.io/blog/2015/02/15/not-born-to-die
[3]: https://github.com/jamiebuilds/babel-handbook/blob/master/translations/en/plugin-handbook.md
[4]: https://github.com/tc39/proposals

---

class: center, middle

# 끝
