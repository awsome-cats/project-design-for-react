# Reactアプリの再レンダリングと最適化

## 再レンダリングの条件

- Stateを持つコンポーネントはstateが変更されると再レンダリングされる => 当然(stateが変更されないと正しい値を表示できない)
- propsが変更されたコンポーネントは再レンダリングされる => propsで成り立っているため当然(ただし、依存関係のない変更は避けたい)
- 再レンダリングされたコンポーネントの配下コンポーネントは再レンダリングされる =>　親と子が持つ同じstate以外の子コンポーネントの再レンダリングは不要

### 再レンダリングの最適化1 コンポーネントのmemo化

- 子コンポーネントにstateをpropsで渡している場合は子コンポーネントをmemo化する(親がその他のstateを持っている時は必須)

### 再レンダリングの最適化2 関数の最適化useCallback

### 親コンポーネントのstateが更新されるsetState関数(アロー関数)を子コンポーネントに渡している場合

- アロー関数は常に新しい関数を生成しているとReactは判断するためmemo化が適用されない

### 同じものを使っているとReactに判断してもらうためにuseCallbackを使用する

- 子コンポーネントにsetStateを渡している場合はその関数をuseCallbackでラップする

```js
// 例

import React, {useState,useCallback} from 'react';

const [open, setOpen] = useState(false)

//const onClickOpen = () => setOpen(false)

const onClickOpen = useCallback(() => setOpen(false), [])


return (
    <div>
        <ChildComponent onClickOpen={onClickOpen}>
    </div>
)

// 第二引数の解説
// setOpenの値が変わらないためsetOpen(false)関数を何度も使う場合第二引数の依存関係は[]で良い。
//念のためsetOpen(false), [setOpen]としてもよい
//動的に値が変更される場合はその依存関係を指定する

```

## 変数の最適化

- useMemo

```js
//同じ変数を使用したいときuseMemoを使用すれば再レンダリングされない
const number = useMemo(() => 1*2, [])

```

## コンポーネントの負荷実験

- 2000の値を持つ配列を作成してレンダリングを確かめる

```js

const data = [...Array(2000).keys()]

```
