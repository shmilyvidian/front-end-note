# Styled-Components
-  官网：https://www.styled-components.com/
## 基础
### 安装
    - npm install --save styled-components
    - <script src="https://unpkg.com/styled-components/dist/styled-components.min.js"></script>
    - bower install styled-components=https://unpkg.com/styled-components/dist/styled-components.min.js
### 原理
利用标签模板来设置组件的样式，移除了组件和样式的映射，当你定义样式的时候也就是在创建一个组件，组件内自动关联了你定义的样式
### 注意事项
- 定义样式在render函数之外，不然每次render都会导致创建，第一个例子是正确推荐的，第二个例子是错误范例
- css厂商前缀已经被自动引入，使用时不必关心这个问题
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
- 使用<ThemeProvider>包括组件，里面的styled-components都可以拿到设置主题
- 基础用法
```javascript
    const Button = styled.button`
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border-radius: 3px;

        color: ${props => props.theme.main};
        border: 2px solid ${props => props.theme.main};
    `;

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
- 使用函数，这个函数接受传入的主题，可以在函数体内根据条件自定义使用哪个主题
```javascript
    const Button = styled.button`
        color: ${props => props.theme.fg};
        border: 2px solid ${props => props.theme.fg};
        background: ${props => props.theme.bg};

        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border-radius: 3px;
    `;

    const theme = {
        fg: 'palevioletred',
        bg: 'white'
    };

    const invertTheme = ({ fg, bg }) => ({
        fg: bg,
        bg: fg
    });

    render(
        <ThemeProvider theme={theme}>
            <div>
            <Button>Default Theme</Button>

            <ThemeProvider theme={invertTheme}>
                <Button>Inverted Theme</Button>
            </ThemeProvider>
            </div>
        </ThemeProvider>
    );

```
- 没有Styled Components的情况下使用主题，可以使用高阶函数withTheme([Own Component])
```javascript
    import { withTheme } from 'styled-components'

    class MyComponent extends React.Component {
    render() {
        console.log('Current theme: ', this.props.theme);
        }
    }

    export default withTheme(MyComponent)
```
- 主题也可以作为属性props传下去，方便特殊主题重写
```javascript
    const Button = styled.button`
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border-radius: 3px;
        color: ${props => props.theme.main};
        border: 2px solid ${props => props.theme.main};
    `;

    const theme = {
        main: 'mediumseagreen'
    };

    render(
    <div>
        <Button theme={{ main: 'royalblue' }}>Ad hoc theme</Button>
        <ThemeProvider theme={theme}>
        <div>
            <Button>Themed</Button>
            <Button theme={{ main: 'darkorange' }}>Overidden</Button>
        </div>
        </ThemeProvider>      
    </div>
    );
```

### Refs
- 传入refs属性可以拿到当前Styled Components的实例
```javascript
    const Input = styled.input`
        padding: 0.5em;
        margin: 0.5em;
        color: palevioletred;
        background: papayawhip;
        border: none;
        border-radius: 3px;
    `;

    class Form extends React.Component {
    render() {
        return (
            <Input
                placeholder="Hover here..."
                innerRef={x => { this.input = x }}
                onMouseEnter={() => this.input.focus()}
            />
        );
    }
    }
    render(
        <Form />
    );
```
- 注意事项
不支持innerRef="node"，因为在React已经是被废弃的写法
### 安全
由于Styled Components允许任意输入作为插值，在input等表单提交很容易被注入
-  CSS.escape
- polyfill by Mathias Bynens （推荐）
### Styled Components如何处理已经存在的样式

### 媒体模板 @media
- 媒体查询基础用法
```javascript
    const Content = styled.div`
        background: papayawhip;
        height: 3em;
        width: 3em;

        @media (max-width: 700px) {
            background: palevioletred;
        }
    `;
    render(
        <Content />
    );
```
- 因为媒体查询一般句式比较长而且经常重复，这个时候创建一个媒体查询模板
```javascript
    const sizes = {
        desktop: 992,
        tablet: 768,
        phone: 576
    }

    const media = Object.keys(sizes).reduce((acc, label) => {
        acc[label] = (...args) => css`
            @media (max-width: ${sizes[label] / 16}em) {
            ${css(...args)}
        }
    `
    return acc
    }, {})

    const Content = styled.div`
        height: 3em;
        width: 3em;
        background: papayawhip;

        ${media.desktop`background: dodgerblue;`}
        ${media.tablet`background: mediumseagreen;`}
        ${media.phone`background: palevioletred;`}
    `;
    render(
        <Content />
    );
```
### 标签模板
- 标签模板是es6新特性，让你可以自定义插入规则
- 如果你没有传入任何参数，那第一个参数是一个数组就存放这个字符
```javascript
    tag`some string here`;
    tag([ 'some string here' ]);
```
- 一旦你传入插值，第一个参数还是数组装载未被插入替换的字符，其他的参数就是传入的插值
```javascript
    const aVar = 'good';
    fn`this is a ${aVar} day`;
    fn([ 'this is a ', ' day' ], aVar);
```
### 服务端渲染
- Styled Components支持服务端渲染，原理是当服务渲染的时候，利用ServerStyleSheet在App上获取样式，生成一个顶级Scope。
```javascript
    import { renderToString } from 'react-dom/server'
    import { ServerStyleSheet, StyleSheetManager } from 'styled-components'

    const sheet = new ServerStyleSheet()
    const html = renderToString(
        <StyleSheetManager sheet={sheet.instance}>
            <YourApp />
        </StyleSheetManager>
    )

    const styleTags = sheet.getStyleTags()
```

