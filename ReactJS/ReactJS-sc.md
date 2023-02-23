# Styled Components

## Extending Styles

`styled()` 작성, 다른 컴포넌트의 스타일 상속

```JavaScript
const Btn = styled.button`
    color: tomato;
    text-color: white;
`;

const circleBtn = styled(Btn)`
    border-radius: 50%;
`;
```

## As

동일한 스타일을 적용하는 동시에 다른 html 태그를 사용하는 경우(특정 엘리먼트를 다른 엘리먼트로 교체)

```JavaScript
function App () {
    return (
        <div>
            <Btn />
            <Btn as="a" />
        </div>
    );
}
```

## Attrs

추가 속성 작성

```JavaScript
const Input = styled.input.attrs({ required: true, minLength: 10 })``;

function App () {
    return (
        <div>
            <Input />
            <Input />
            <Input />
        </div>
    );
}
```

## Animations

keyframes helper 작성

```JavaScript
const rotate = keyframes`
    from {
        transform: rotate(0deg);
    }
    to {
        transform: rotate(360deg);
    }
`;

const Btn = styled.button`
    animation: ${rotate} 1s infinite;
`;
```

## Pseudo Selector

스타일 컴포넌트애 속하는 엘리먼트

```JavaScript
const Container = styled.div`
    h1 {
        color: black;
        &: hover {
            color: white;
        }
    }
`;

function App () {
    return (
        <Container>
            <h1>Hello</h1>
        </Container>
    );
}
```

스타일 컴포넌트에 속하는 스타일 컴포넌트

```JavaScript
const H1 = styled.h1``;
const Container = styled.div`
    ${H1} {
        color: black;
        &: hover {
            color: white;
        }
    }
`;

function App () {
    return (
        <Container>
            <H1>Hello</H1>
        </Container>
    );
}
```
