## 기능
* HOC Component

--------------------------------------------------------------------------------------------------------


<details>
  <summary>HOC Component: Higher Order Component</summary>
  <div markdown="1">
    <ul>
      <li>
        고차 컴포넌트는 컴포넌트를 매개변수로 받고, 리턴할때 새로운 컴포넌트를 반환하는 함수를 의미한다.
        고차 함수와 비슷하지만 고차함수는 함수를 함수를 리턴하는 것과비슷한 원리이다. 
        고차 컴포넌트는 함수 대신 컴포넌트를 인자로 받아 컴포넌트를 리턴한다고 생각하면된다.
      </li>
      <li>
        고차 컴포넌트를 만들때는, 커스텀 훅을 만들때처럼 use를 붙여 네이밍을 하듯이, "with"을 붙어 네이밍을 하는 컨벤션이 존재한다
      </li>
      <br>
 간단한 예 ) 
      <pre>
        <code>
function withExample(WrappedComponent) {
  return function EnhancedComponent(props) {
    return <WrappedComponent {...props} extraProp="example" />
  }
}
        </code>
      </pre>
      <pre>
          <code>                 
const MyComponent = (props) => <div>{props.extraProp}</div>;
const MyEnhancedComponent = withExample(MyComponent);
            <br>
// 다른 컴포넌트의 render 메소드에서
<MyEnhancedComponent />
          </code>
      </pre>
    </ul>
  </div>
</details>

