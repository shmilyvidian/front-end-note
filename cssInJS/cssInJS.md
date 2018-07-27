# Styled-components （内容基于官方文档）
-  官网：https://www.styled-components.com/
## 基础
### 安装
    - npm install --save styled-components
    - <script src="https://unpkg.com/styled-components/dist/styled-components.min.js"></script>
    - bower install styled-components=https://unpkg.com/styled-components/dist/styled-components.min.js
### 原理
利用标签模板来设置组件的样式，移除了组件和样式的映射，当你定义样式的时候也就是在创建一个组件，组件内自动关联了你定义的样式
### 注意事项
- 定义样式在render函数之外，不然每次render都会导致创建
- css厂商前缀已经被自动引入，使用时不必关心这个问题，第一个例子是正确推荐的，第二个例子是错误范例
    ```javascript
        const StyledWrapper = styled.div`/* ... */`
        const Wrapper = ({message}) => {
            return <StyledWrapper>{message}</StyledWrapper>
        }

        const Wrapper = ({message}) => {
            const StyledWrapper = styled.div`/* ... */`
            return <StyledWrapper>{message}</StyledWrapper>
        }
    ```
### 利用props
```javascript
        const Circle = styled.span`
            display: inline-block;
            width:20px;
            height:20px;
            text-align:center;
            line-height:20px;
            border-radius: 50%;
            background: ${props=> props.isActive ? '#4F88FE' : '#324160'};
            margin-right:5px;
            color:#fff
        `
        <Circle isActive={store.tabActiveKey == (i+1)}>{i+1}</Circle>
```

### styled方法可以运用在所有你的标签或者第三方组件，前提是传入了className属性给子级
```javascript
    const Link = ({ className, children }) => (
        <a className={className}>
            {children}
        </a>
    )

    const StyledLink = styled(Link)`
        color: palevioletred;
        font-weight: bold;
    `;

    render(
    <div>
        <Link>Unstyled, boring Link</Link>
        <br />
        <StyledLink>Styled, exciting Link</StyledLink>
    </div>
    );
```
### 继承已有样式 extend，当你想利用一个已有样式，大部分可以服用只会重写一小部分这个功能就非常适合
- demo 
```javascript
    const Button = styled.button`
        color: palevioletred;
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border: 2px solid palevioletred;
        border-radius: 3px;
    `;

    // We're extending Button with some extra styles
    const TomatoButton = Button.extend`
        color: tomato;
        border-color: tomato;
    `;
```
- 注意事项
    当你明确知道这是一个styled component，你可以放心的使用[styled component].extend方法，当时引入的第三方库的时候需要使用styled(Comp)

- 当你使用extend方法时候同时想修改继承过来的标签可以使用withComponent实现
```javascript
    const Button = styled.button`
        display: inline-block;
        color: palevioletred;
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border: 2px solid palevioletred;
        border-radius: 3px;
    `;

    const Link = Button.withComponent('a')

    const TomatoLink = Link.extend`
        color: tomato;
        border-color: tomato;
    `;
    render(
        <div>
            <Button>Normal Button</Button>
            <Link>Normal Link</Link>
            <TomatoLink>Tomato Link</TomatoLink>
        </div>
    );
```
### 附加额外的属性
```javascript
        const Input = styled.input.attrs({
            type: 'password',
            margin: props => props.size || '1em',
            padding: props => props.size || '1em'
        })`
        color: palevioletred;
        font-size: 1em;
        border: 2px solid palevioletred;
        border-radius: 3px;
        margin: ${props => props.margin};
        padding: ${props => props.padding};
        `;
        render(
        <div>
            <Input placeholder="A small text input" size="1em" />
            <br />
            <Input placeholder="A bigger text input" size="2em" />
        </div>
        );
```
### 动画 keyframes
```javascript
    const rotate360 = keyframes`
    from {
        transform: rotate(0deg);
    }

    to {
        transform: rotate(360deg);
    }
    `;

    const Rotate = styled.div`
        display: inline-block;
        animation: ${rotate360} 2s linear infinite;
        padding: 2rem 1rem;
        font-size: 1.2rem;
    `;
    render(
    <Rotate>&lt; 💅 &gt;</Rotate>
    );
```
## 进阶
### 主题
- 使用<ThemeProvider>包括组件，里面的styled-componentsd都可以拿到设置主题
```javascript
    const Button = styled.button`
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border-radius: 3px;

        /* Color the border and text with theme.main */
        color: ${props => props.theme.main};
        border: 2px solid ${props => props.theme.main};
    `;

        // We're passing a default theme for Buttons that aren't wrapped in the ThemeProvider
    Button.defaultProps = {
        theme: {
            main: 'palevioletred'
        }
    }

    const theme = {
        main: 'mediumseagreen'
    };

    render(
        <div>
            <Button>Normal</Button>

            <ThemeProvider theme={theme}>
                <Button>Themed</Button>
            </ThemeProvider>
        </div>
    );
```
### Refs

### 安全

