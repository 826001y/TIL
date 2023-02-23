# ReactJS

## JSX

JavaScript를 확장한 문법

```JavaScript
const h1 = (
    <h1 id="title" onMouseEnter={() => console.log("mouse enter")}>Hello world</h1>
);
//babel 설치 통해 상단 코드를 하단 코드로 변환하여 브라우저가 이해하도록 함
const h1 = React.createElement("h1", {id: "title", onMouseEnter: () => console.log("mouse enter");}, "Hello world");
```

## setState

`React.useState()`는 [undefined, f]의 배열을 반환

- `undefined` : 초기값(변수),
- `f` : undefined 값을 바꾸는 함수

`const [state, setState] = React.useState();`

1. `state`에 값 입력받음
2. `setState` 함수 실행, `state` 값 변경해 저장(해당 컴포넌트 리렌더링)

### (참고) 인자 할당 방식

- 직접 할당 : `setState(state +1)`
- 함수 할당 : `setState((state) => state +1)` (현재 state가 함수의 첫 번째 인자)

### (참고) 컴포넌트 리렌더링 조건

- props 변경
- state 변경
- 부모 컴포넌트의 리렌더링

## Props

부모 컴포넌트에서 자식 컴포넌트로 값 전달(props를 사용하면 자식 컴포넌트의 인자로 객체가 들어가게 됨)

```JavaScript
function Btn ({text, fontSize, onClick }) { //유일한 인자로 props 객체를 받고 컴포넌트에 전달
return ( //JSX로 button 태그(html)에 event listener 보냄
<button onClick={onClick} style={{
backgroundColor: "black",
color: "white",
padding: 10 20,
borderRadius: 10,
fontSize,
}}>{text}</button>
);
}
```

```JavaScript
function App () {
const [value, setValue] = React.useState("Save");
const changeValue = () => setValue("Change");
return (
<Btn text={value} onClick={changeValue} /> //changeValue는 event listener가 아니라 Btn 컴포넌트로 보내지는 prop 함수
//<Btn style={{}} /> 작성 불가
<Btn text="Delete" />
);
}
```

## React.memo

불필요한 리렌더링을 관리

`const MemorizedComponent = React.memo()`

- props가 변경된 부분만 렌더링
- 부모 컴포넌트의 state를 변경했을 때 모든 자식 컴포넌트가 리렌더링되는 경우 방지)

## Prop Types

파라미터를 잘못 할당할 경우 방지

```JavaScript
Btn.propTypes = {
text:PropTypes.string.isRequired,
fontSize:PropTypes.number,
}
```

## useEffect

코드의 실행 시점 관리

- `useEffect(sendEvent, []);`

  컴포넌트가 생성되는 최초 1회만 sendEvent 함수 실행(dependency가 없는 경우)

- `useEffect(sendEvent, [element1, element2]);`

  element1 또는 element2의 값이 변경될 때 sendEvent 함수 실행(여러 개의 dependency 입력 가능)

### (참고) cleanup

```JavaScript
function App () {
const [showing, setShowing]=React.useState(true);
const onClick=()=>setShowing((prev)=>!prev);
useEffect(()=>{
createEvent(); //컴포넌트가 생성될 때 실행
return ()=> console.log("destroyed"); //컴포넌트가 삭제될 때 실행
}, []);
return (
<div>
{showing ? <h1>Hello</h1> : null}
<button onClick={onClick}>{showing ? "hide" : "show" }</button>
</div>
);
}
```
