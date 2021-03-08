### 🎇JS 중고급 START!!

---





#### L1 - ES3/ES5 스펙의 아키텍처, 메커니즘 관련 키워드

---

##### ES3 스팩

1. Execution Contexts

   >  실행 콘텍스트. 함수 호출 시 함수를 실행하는 환경. 함수가 실행 되었을때 결과를 저장하는 영역. 즉, 함수의 모든 처리는 이 안에서 이루어진다.

2. Definition

   * Function Objects

     : 엔진이 Function 키워드를 만났을때 만드는 Function Object를 뜻한다.

   * Types of Executable Code 

      : 실행 가능 코드. (Global, Eval, Function 코드가 있다.)

   * Global, Function Code

      : 둘은 위치만 다르고 코드는 같다.  단, 실행하는 영역이 다름.

   * Variable Instantiation 

      : 변수를 어떻게 인스턴스화 시켜서 해결할 것이냐.

   * Scope Chain and Identifier Resolution:

     * Identifier Resolution
       : 함수를 호출할때 어떻게 함수 이름을 찾을것이냐. 
       변수의 값을 설정할때 어떻게 변수의 이름을 찾을것이냐. (여기서 Id는 함수, 변수 이름을 뜻한다.)

       1. Global Object

          : 전역 객체이다. (Function Object와 구분하는 이유는 영역이 다르기때문이다.)

       2. Activation Object

          : 함수 호출 시 함수를 실행할 수 있는 환경.  
          그리고 함수가 실행되었을 때 실행되는 결과를 저장하는 오브젝트 
          (Object니까 Property이다. 즉,  이 안에는 또 다른 Object가 존재할 수 있다.)

   * This

     : 인스턴스에서 중요한 위치이다.

   * Arguments Object

     함수의 파라미터를 처리하는 오브젝트

3. Entering An Execution Context

   * Global Code
   * Eval Code
   * Function Code

##### ES5 스펙

1. Executable Code and Execution Contexts

2. Types of Executable Code

   * Strict Mode Code

     : use strict 로 작성했을때의 실행 모드

3. Lexical Environments

   > Lexical(정적), Dynamic(동적)이다.  
   > 자바스크립트는 Lexical 환경을 취한다. 그래서 실행 콘텍스트가 하나의 실행하는 묶음이라면 그 안에서 환경적인 측면을 처리하는것이다.

   * Environment Records

     : 함수가 호출되어 실행될 때, 실행되기 전 상황을 기록

   * Lexical Environment Operations

     : 어떤 변수에 값을 할당했을때 그것을 정적인 환경에 설정한다는 것.

   * The Global Environment

     : Global Object를 처리하는 Environment 

4. Execution Contexts

   > Identifier Resolution: 실행 Context가 실행하는 묶음이면. 식별자 해결은 바탕이 되는 개념. 함수를 호출했을때 어디서 찾을꺼냐? 이러한 개념이 여기 들어가있다.

5. Establishing an Execution Context

   >  Entering이란? 함수 안으로 엔진 컨트롤이 이동했을때 함수 안에 작성된 코드를 어떻게 처리할 것인가. 이러한 개념적인 이야기.

   * Entering Global Code 
   * Entering Eval Code
   * Entering Function Code

6. Declaration Binding Instantiation

   > 바인딩은 변수, this와 관련이다. 
   > 변수를 어떻게 Execution Contexts의 Lexical Environments에 바인딩시켜서 Recording 할것인가.
   > 또한 This를 어떻게 바인딩할 것인가. (This는 함수 안에서만 되는데 ..)
   > 그리고 이것들은 Record 될것이고 Environment에 정리가 될것이다.

7. Arguments Object

   > 파라미터 처리 오브젝트.

   

**함수가 실행되기전에 어떻게 할 것인가. 그리고 함수가 호출되었을때 어떻게 처리할 것인가에 대한 내용이 위에 서술한 모든 내용에 담겨있는것이다.**



#### L2 - 엔진 관점의 핵심 키워드

---

1. 엔진 처리 : 크게 해석, 실행으로 나뉜다.

   * 해석: 컴파일, 실행을 할 환경을 설정
   * 실행: 해석 단계에서 설정된 환경을 바탕으로 코드를 실행하는 것.
     * 함수가 호출 된 다음 단계에 하는것.
     * 키워드는 Context(함수라는 단위를 어떠한 묶음 ,덩어리로 가져갈 것인지 정하는 것)

2. 엔진 관점에서의 부담

   > 단일 Context 개념으로 하나의 box 안에서 모든 함수, 변수 관련된 일을 처리하는 것이 심플하고 빠르다. 여기저기 선언된 변수, 함수를 찾아다니는것은 복잡하고 시간도 오래거린다.
   > 이미 Context 실행중인데 Scope Chain 개념으로 어디가에서 다시 끌고오는 ... 이것은 부담스럽다.

   -> 이것은 궁극적으로 Identifier Resolution(식별자 해결)을 위한것이다. 즉, 함수, 변수이름을 어떻게 빨리 찾고 실행할 것인가.



#### L3 - Execution Context 형태

---

```javascript
function book(){
    var point = 123;   
    function show(){   
        var title = "js책";
        // getPoint();
        // this.bookAmount;
    };
    function getPoint(){
        return point;
    };
    show();
}
book();
```

1. book() 함수 호출 

   1. show function 오브젝트 생성

   2. show의 [[Scope]]에 스코프 설정 (point, getPoint() (변수이름,함수이름)을 설정)
      [[Scope]]: 대괄호 두개([[)는 엔진이 설정하는 프로퍼티를 뜻한다.

   3. getPoint function 오브젝트 생성

   4. show() 호출

      1. 엔진 컨트롤이 show() 함수 안으로 이동

         1. 이동하기전 show 실행 콘텍스트를 만든다. (함수 실행을 위한 Context 환경 구축)

            즉, 하나의 덩어리 안에서 함수가 실행될 수 있도록 만드는 것.

            * 실행 컨텍스트 구성요소

              ``` javascript
              show 실행 콘텍스트(EC): {
              	렉시컬 환경 컴포넌트(LEC): {  //1
                      환경 레코드(ER): {  //3
                          선언적 환경 레코드(DER): {
              				title: "JS책"
                          },
                          오브젝트 환경 레코드(OER): {}
                      },
                      외부 렉시컬 환경 참조(OLER):{
                          point: 123,
                          getPoint: function(){}
                      }
                  },
                  변수 환경 컴포넌트(VEC): {},  //1
                  this 바인딩 컴포넌트(TBC): {  //2
                      글로벌 오브젝트(window)
                  }
              }
              ```

              1. LEC와 같은 레벨에 VEC이 있다. 그리고 LEC, VEC의 초기값은 같다.
              2. TBC를 만든다. (this로 참조할 오브젝트를 바인딩 시키는 것)
              3. 함수코드에서 var title을 만나게되면 ER의 DER의 title:"JS책"을 프로퍼티 형태로 설정
              4. 또한 OER도있지만 나중에 다루자.
              5. show 밖의 변수를 값으로 사용할 수 있다. 이 원리는 show Function Object를 만들때 [[Scope]]에 설정된 것을 OLER에 설정해놨기 때문이다. (point, getPoint)
              6. 함수 밖에있는것이지만 하나의 show 실행 컨텍스트(하나의 덩어리)이기 때문에 내 것처럼 쓸수있는것이다.
              7. this로 참조할 수 있는것은 TBC에 바인딩 시킨다.
              8. show() 오브젝트는 Global Object이다. 하지만 Global Object는 실체가없기때문에 Host Object 개념으로 window 오브젝트를 참조하게된다.

              >  즉, 실행 콘텍스트안에 함수에서 구할 수 있는 값의 덩어리를 만들어 놓는것이다. 
              > 따라서 함수가 메모리위에 올라갔을때, 실행될때 함수에서 다른 값을 구하려고 메모리에서
              > 빠져나오거나 메모리 올리거나 할 필요가없다는 것. 하나의 Context개념으로 묶어놨기 때문이다.

              **:sparkles: :sparkles:이것이 자바스크립트의 Lexical Context(정적 컨텍스트) 개념이다. 이러한 환경을 정적으로 만든다는 것. 이에 맞추어 코드를 작성하면 js엔진이 심플하고 빠르게 처리할 수 있을것이다. 그래서 실행 컨텍스트를 알아야한다.**

              

