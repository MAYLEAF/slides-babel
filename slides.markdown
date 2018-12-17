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

---

## babel

- es6 코드를 es5 로 변환시켜주는 도구
  - parse
  - map plugins to ast
  - write
- [Sebastian McKenzie](https://github.com/kittens)가 2014에 시작

???

- The compiler for writing next generation JavaScript
- 6to5
- founder 는 facebook 으로 이직 성공 ^^
- babel 로 이름을 바꾸며 주요 툴로 자리잡았고, tc39 와도 많은 접점
- react (jsx), flow, vue 등 유우명 프로젝트에서 활발히 사용 중

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

## ECMA 표준화 현황

- tc39, stage[0-3]

---

## babel.js 소개

- AST 변환의 컨셉
- lisp macro 와의 유사성

---

## babel.js 대표적 플러그인 소개

- 플러그인 처리 내부 동작 소개

---

## 간단한 플러그인 제작 과정

---

## 활용 방안

---

## References

- [example][1]

[1]: http://example.org/

---

class: center, middle

# 끝
