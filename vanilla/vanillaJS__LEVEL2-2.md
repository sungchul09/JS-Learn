### [SECTION5 execution Context]

---

#### L23 - 실행 콘텍스트 / 실행 콘텍스트 상태 컴포넌트

---

```javascript
function music(title){
  var musicTitle = title;
};
music('음악')
```

1. 실행 콘텍스트

   * 함수가 실행되는 영역, 묶음
   * 함수 코드를 실행하고 실행 결과를 저장
   * 스팩상의 사양이다. (즉, 개발자가 실행 콘텍스트의 뭔가를 바꿀수는없고 엔진이 처리하는 부분이다.)
   * music('음악')으로 함수를 호출하면 엔진은 실행 콘텍스트를 생성하고 실행 콘텍스트 안으로 이동한다.
   * 실행 콘텍스트 실행단계
     * 준비, 초기화, 코드실행 단계
   * 실행 콘텍스트 생성 시점
     * 실행 가능한 코드를 만났을 때
   * 실행 가능한 코드 유형
     * 함수 코드, 글로벌 코드, eval 코드
   * 코드 유형을 분리한 이유는 실행 콘텍스트에서 처리 방법과 실행 환경이 다르기 때문이다.
     * 함수 코드: 렉시컬 환경
     * 글로벌 코드: 글로벌 환경
     * eval 코드: 동적 환경

2. 실행 콘텍스트 상태 컴포넌트

   ``` javascript
   실행 콘텍스트(EC): {
     렉시컬 환경 컴포넌트(LEC): {
       //이 안에 프로퍼티 형태로 작성
     },
     변수 환경 컴포넌트(VEC): {},
     this 바인딩 컴포넌트(TBC): {}
   }
   ```

   * 실행 콘텍스트 상태를 위한 오브젝트 (실행 콘텍스트 안에 생성)
     * 렉시컬 환경 컴포넌트 (LEC)
     * 변수 환경 컴포넌트 (VEC)
     * this 바인딩 컴포넌트 (TBC)



#### L24 - 렉시컬 환경 컴포넌트 / 렉시컬 환경 컴포넌트 구성,설정 / 외부 렉시컬 환경 참조 / 변수 환경 컴포넌트

---

1. 렉시컬 환경 컴포넌트
   * 함수와 변수의 식별자 해결을 위한 환경 설정
   * 함수 초기화 단계에서 해석한 함수, 변수를 {name: value} 형태로 저장한다.
   * 변수 {name: undefined}, 함수{name: function 오브젝트}
     즉, 이름으로 함수와 변수를 검색할 수 있게 된다.
   * 함수 밖의 함수와 변수를 참조할 수 있는 환경을 설정한다.
     즉, 함수 밖의 함수, 변수를 사용할 수 있게 된다.

2. 렉시컬 환경 컴포넌트 구성

   ```javascript
   실행 콘텍스트(EC): {
     렉시컬 환경 컴포넌트(LEC): {
   		환경 레코드(ER):{
         point: 100
       },
       외부 렉시컬 환경 참조(OLER): {
         title: "책",
         getTitle: function(){}
       }
     }
   }
   ```

   * 렉시컬 환경 컴포넌트 생성
     * function, with, try-catch에서 생성
   * 컴포넌트 구성
     * 환경 레코드(ER: Environment Record)
     * 외부 렉시컬 환경 참조(OLER: Outer Lexical Environment Reference)

3. 렉시컬 환경 컴포넌트 설정

   * 환경 레코드(ER)에 함수 안의 함수와 변수를 기록
   * 외부 렉시컬 환경 참조(OLER)에 function 오브젝트의 [[Scope]]를 설정한다.
   * 따라서, 함수 안과 밖의 함수와 변수를 사용할 수 있게 된다.

4. 외부 렉시컬 환경 참조

   * 스코프와 실행중인 함수가 Context 형태이다.
   * 스코프의 변수와 함수를 별도의 처리없이 즉시 사용할수있다.
   * 실행 콘텍스트에서 함수 안과 밖의 함수, 변수를 사용할 수 있으므로 함수 변수를 찾기위해 실행 콘텍스트를 벗어나지 않아도 된다.

5. 변수 환경 컴포넌트

   ```javascript
   실행 콘텍스트(EC): {
   	렉시컬 환경 컴포넌트(LEC): {},
     변수 환경 컴포넌트(VEC): {},
     this 바인딩 컴포넌트(TBC): {}
   }
   ```

   * 실행 콘텍스트 초기화 단계에서 렉시컬 환경 컴포넌트와 같게 설정한다.
   * 초기값을 복원할때 사용하기 위한 것이다. 
     * 함수코드가 실행되면 실행결과를 LEC에 설정한다. 
     * 초기값이 변하게 되므로 이를 유지하기 위한 것.



#### L25 - 실행콘텍스트 실행 과정

``` javascript
var base = 200;
function getPoint(bonus){
  	var point = 100;
  	return point + base + bonus;
}
console.log(getPoint(70));
```

1. 실행 과정
   * getPoint 오브젝트의 [[Scope]]에 글로벌 오브젝트를 설정

   * getPoint() 함수 호출

   * 엔진은 실행 콘텍스트 생성 -> 실행 콘텍스트 안으로 이동

   * ---준비단계---

   * 컴포넌트를 생성해 실행 콘텍스트에 첨부한다.

     * 렉시컬 환경 컴포넌트
     * 변수 환경 컴포넌트
     * this 바인딩 컴포넌트

     ```javascript
     실행 콘텍스트(EC):{
         렉시컬 환경 컴포넌트(LEC): {},
         변수 환경 컴포넌트(VEC): {},
         this 바인딩 컴포넌트(TBC): {}
     }
     ```

   * 환경 레코드를 생성해 렉시컬 환경 컴포넌트에 첨부

     * 함수 안의 함수, 변수를 바인딩한다.

       ```javascript
       실행 콘텍스트(EC): {
           렉시컬 환경 컴포넌트(LEC) = {
               환경 레코드(ER): {}
           },
           변수 환경 컴포넌트(VEC): {},
           this 바인딩 컴포넌트(TBC): {}
       }
       ```

     * 외부 렉시컬 환경 참조를 생성하여 렉시컬 환경 컴포넌트에 첨부하고 function 오브젝트의 [[Scope]]를 설정한다.

       ```javascript
       실행 콘텍스트(EC): {
           렉시컬 환경 컴포넌트(LEC) = {
               환경 레코드(ER): {},
               외부 렉시컬 환경 참조(OLER): {
                   base: 200
               }
           },
           변수 환경 컴포넌트(VEC): {},
           this 바인딩 컴포넌트(TBC): {}
       }
       ```

   * ------ 초기화 단계 -----

   * 호출한 함수의 파라미터값을 호출된 함수의 파라미터 이름에 매핑 -> 환경 레코드에 작성 (여기까지는 외부 실행 상태를 제공하지 않는다.)

     ```javascript
     실행 콘텍스트(EC): {
         렉시컬 환경 컴포넌트(LEC) = {
             환경 레코드(ER): {
                 bonus: 70,
                 point: undefined
             },
             외부 렉시컬 환경 참조(OLER): {
                 base: 200
             }
         },
         변수 환경 컴포넌트(VEC): {},
         this 바인딩 컴포넌트(TBC): {}
     }
     ```

   * ----- 실행단계 -----

   * 함수 안의 코드를 실행한다. (var point = 100;)

   * 실행 콘텍스트 안에서 관련된 함수와 변수를  사용할 수 있다.

   

#### L26 - 환경 레코드 구성

----

1. 환경 레코드 구성

   ``` javascript
   실행 콘텍스트(EC): {
   	랙시컬 환경 컴포넌트(LEC):{
           환경 레코드(ER): {
               선언적 환경 레코드(DER): { //function 변수, catch 문
                   point: 123
               },
               오브젝트 환경 레코드(OER): { //글로벌 함수,변수, with문 (동적)
                   
               }
           },
           외부 렉시컬 환경 참조(OLER): {}
       },
       변수 환경 컴포넌트(VEC):{},
       this 바인딩 컴포넌트(TBC): {}
   }
   ```

   * 환경 레코드를 구분하는 이유는 기록 대상이 다르기 때문이다.
   * 선언적 환경 레코드
     * DER : Declarative Environment Record
     * function, 변수, catch 문에서 사용한다.
     * 앞 절에서 환경 레코드에 설정한다고 했는데 설명을 위한 것이었다.
       실제로는 여기에 설정한다.
   * 오브젝트 환경 레코드
     * OER: Object Environment Record
     * 글로벌 함수와 변수, with 문에서 사용한다.
     * 정적이 아니라 동적이기 때문이다.

2. 글로벌 환경

   ```javascript
   실행 콘텍스트(EC):{
       글로벌 환경(GE):{ // 글로벌 오브젝트
           환경레코드(ER):{
               오브젝트 환경 레코드: 글로벌 오브젝트
           },
           외부 렉시컬 환경 참조(OLER): null
       }
   }
   ```

   * Global Environment
     * 글로벌 오브젝트에서 사용
     * 렉시컬 환경 컴포넌트와 형태 같음
   * 동적으로 함수와 변수를 바인딩한다.
     * 함수에서 var 키워드를 사용하지않고 변수를 선언하면 글로벌 오브젝트에 선언되기 때문이다.
     * 이런 이유로 오브젝트 환경 레코드를 사용한다.
   * 외부 렉시컬 환경 참조 값은 항상 null이다.



#### L27 - this 바인딩 컴포넌트

---

1. this 바인딩 컴포넌트

   * 목적
     * this로 함수를 호출한 오브젝트의 프로퍼티에 엑세스
       (ex. this.propertyName)
   * 엑세스 매커니즘
     * obj.book() 형태여서 this로 obj를 참조할 수 있도록
     * this 바인딩 컴포넌트에 obj 참조를 설정
   * obj의 프로퍼티가 변경되면 동적으로 참조
     * 설정이 아닌 참조이기 때문이다.

2. 예시

   ```javascript
   var obj = {point: 100};
   obj.getPoint = function(){
   	return this.point;
   };
   obj.getPoint();
   ```

   ```javascript
   실행 콘텍스트:{
   	렉시컬 환경 컴포넌트{
           환경레코드(ER):{
               선언적 환경 레코드(DER):{},
               오브젝트 환경 레코드(OER): {}
           }
       },
       변수 환경 컴포넌트:{
           
       },
       this 바인딩 컴포넌트:{
           point: 100,
           getPoint: function(){}
       }
   }
   ```

   * 준비단계
     * 마지막 줄에서 obj.getPoint() 함수 호출
     * 실행 콘텍스트 생성
     * 3개의 컴포넌트 생성
       렉시컬 환경 컴포넌트 / 변수 환경 컴포넌트 / this 바인딩 컴포넌트
     * this 바인딩 컴포넌트에 getPoint()에서 this로 obj의 프로퍼티를 사용할 수 있도록 바인딩
   * 초기화단계
     * 파라미터, 함수 선언문, 변수 선언 없는 상태
   * 실행 단계
     * return this.point; 실행
     * this 바인딩 컴포넌트에서 point 검색
       (getPoint() 함수를 호출한 오브젝트가 this 바인딩 컴포넌트에 설정(참조)된 상태)
   * 추가설명
     * obj.getPoint()에서 obj의 프로퍼티가 this 바인딩 컴포넌트에 바인딩되도록 의도적으로 설계해야한다.



#### L28 - 호출 스택(call stack)

---

1. 호출 스택
   * call stack
     * 실행 콘텍스트의 논리적 구조
   * First In Last Out 순서
     * 함수가 호출되면 스택의 가장 위에 실행 콘텍스트가 위치한다.
     * 다시 함수 안에서 함수를 호출하면 호출된 함수의 실행 콘텍스트가
       스택의 가장 위에 놓이게 된다.
     * 함수가 종료되면 스택에서 빠져나온다 (FILO)
     * 가장 아래는 글로벌 오브젝트의 함수가 위치해있다.



#### L29 - 파라미터 매핑 / 함수 호출 / 파라미터 값 매핑 / 파라미터 이름에 값 매핑 방법

---

1. 함수 호출
   * 함수가 호출되면 3개의 파라미터 값을 실행 콘텍스트로 넘겨준다.
     * 함수를 호출한 오브젝트
     * 함수 코드
     * 호출한 함수의 파라미터 값
   * 함수를 호출한 오브젝트를 this 바인딩 컴포넌트에 설정해 this로 참조한다.
   * 함수 코드는 function 오브젝트의 [[Code]]에 설정되어 있다.
   * 호출한 함수의 파라미터 값은 호출된 함수의 Argument 오브젝트에 설정한다.

2. 파라미터 값 매핑

   * 파라미터 값 매핑이란 ?
     * 호출한 함수에서 넘겨 준 파라미터 값을 호출된 함수의 파라미터 작성 순서에 맞추어 값을 매핑하는 것
   * 엔진처리 관점에서 실행 콘텍스트로 넘겨준 파라미터 값과 function 오브젝트의 [[FormalParameters]]에 작성된 이름에 값을 매핑하고 결과를 선언적 환경 레코드에 설정하는 것

3. 파라미터 이름에 값 매핑 방법

   ``` javascript
   var obj = {};
   obj.getTotal = function(one, two) {
   	return one + two;
   };
   console.log(obj.getTotal(11,22,77));
   ```

   * getTotal 오브젝트의 [[FormalParameters]]에서 호출된 함수의 파라미터 이름을 구한다.(설명 편의를 위해 name이라고 하자.)
     * name은 ["one", "two"] 형태이다.
     * [[FormalParameters]]는 function 오브젝트를 생성할 때 설정한다.
   * name 배열을 하나씩 읽는다.
   * param에서 index 번째의 값을 구한다.
     * index에 값이 없으면 undefined 반환

   * name의 파라미터 이름과 4번에서 구한 값을 선언적 환경 레코드에



#### L30 - 파라미터 값 할당 기준

---

1. 파라미터 값 할당 기준

   ``` javascript
   var obj = {};
   obj.getTotal = function(one, two){
       var one;
       console.log(one + two);
       two = 77;
       console.log("two:"+ two);
   }
   obj.getTotal(11, 22);
   ```

   * 초기화 단계
     * obj.getTotal(11,22) 함수가 호출되면 파라미터 값을 실행 콘텍스트로 넘겨준다.
     * 파라미터 이름에 값을 매핑하여 선언적 환경 레코드에 설정한다.
       {one: 11, two: 22}
     * var one;
       선언적 환경 레코드에서 one의 존재를 체크한다. 파라미터 이름을 설정하였으므로 존재하며 one을 기록하지 않는다.
     * two = 77;
       선언적 환경 레코드에서 two의 존재를 체크한다. 파라미터 이름을 설정하였으므로 존재하며 two를 기록하지 않는다.
     * 함수에 초기화할 코드가 없으므로 첫째줄로 이동해 함수 코드를 실행한다.
   * 실행 단계
     * 선언적 환경 레코드는 {one: 11, two:22} 상태
     * var one;
       단지, 변수 선언이므로 처리하지 않는다.
     * console.log(one + two);
       선언적 환경 레코드에서 one과 two의 값을 구한다.
       11 + 22의 결과인 33이 [실행 결과]에 출력된다.
     * two = 77;
       값을 할당하는 코드이며 실행 단계이므로 선언적 환경 레코드의 two에 77을 할당한다. ({two:22}가  {two:77} 로 변경)
     * console.log("two: "+ two);
       선언적 환경 레코드에서 two의 값을 구한다. (two: 77이 출력된다.)



### [SECTION6 function instance]

#### L31 - function 인스턴스 기준 /  function 인스턴스 생성

---

1. function 인스턴스 기준

   ``` javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point + 200;
   };
   var obj = new Book(100);
   obj.getPoint();
   
   ```

   * function 부분
     * 빌트인 Function 오브젝트
     * function 오브젝트: function 키워드로 생성
     * function 인스턴스: new 연산자로 생성
   * function 오브젝트도 인스턴스
     * 빌트인 Function 오브젝트로 생성하기 때문
     * 성격적으로는 인스턴스이지만 new 연산자로 생성한 인스턴스와 구분하기 위해 강좌에서는 function 오브젝트로 표기
     * new 연산자로 생성하는 인스턴스는 일반적으로 prototype 프로퍼티를 작성

2. 인스턴스 생성 순서, 방법

   * function Book(point) {...}
     * Book 오브젝트를 생성한다.
     * Book.prototyppe이 만들어진다.
   * Book.prototype.getPoint = function(){}
     * Book.prototype에 getPoint를 연결하고 function(){} 을 할당한다.
     * Book.prototype이 오브젝트이므로 프로퍼티를 연결할 수 있다.
   * var obj = new Book(100);
     * Book()을 실행하며 인스턴스를 생성하고 생성한 인스턴스에 point값을 설정한다.
     * Book.prototype에 연결된 프로퍼티를 생성한 인스턴스에 할당한다.
   * console.log(obj.point);
     * obj 인스턴스에서 프로퍼티 이름으로 값을 구해 출력한다.
   * console.log(obj.getPoint());
     * obj 인스턴스의 메소드를 호출한다
   * return this.point + 200;
     * this가 obj 인스턴스를 참조한다.
   * 강좌의 함수/메소드 사용 기준 (프로토타입에 연결되어있으면 메소드, 아니면 함수)
     * Book()은 함수,    getPoint()는 메소드
     * prototype에 연결한다.



#### L32 - 생성자 함수

---

1. 생성자 함수

   * new 연산자와 함께 인스턴스를 생성하는 함수
     * new Book()에서 Book()이 생성자 함수
   * new 연산자
     * 인스턴스 생성을 제어
     * 생성자 함수 호출
   * 생성자 함수
     * 인스턴스 생성, 반환
     * 인스턴스에 초기값 설정
   * 코딩 관례로 생성자 함수의 첫 문자는 대문자
     * new Number(), new Array(), new Book()

2. 생성자 함수 실행 과정

   * new 연산자로 인스턴스 생성을 제어하고 생성자 함수인 Book()으로 인스턴스를 생성하여 반환한다.

   ```javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point;
   };
   
   var obj = new Book(10);
   ```

   * 엔진이 new 연산자를 만나면 function의 [[Construct]]를 호출하면서 파라미터 값으로 10을 넘겨준다.
   * function 오브젝트를 생성할 때 Book() 함수 전체를 참조하도록 [[Construct]]에 설정했다.
   * [[Construct]]에서 인스턴스를 생성하여 반환한다.
   * 반환된 인스턴스를 new 연산자가 받아 new 연산자를 호출한 곳으로 반환한다.
   * new라는 뉘앙스로 인해 new 연산자가 인스턴스를 생성하는것으로 생각할 수 있지만, function 오브젝트으 [[Construct]]가 인스턴스를 생성한다.
   * 그래서 Book()이 인스턴스 생성자 함수이다.

3. 인스턴스 생성 과정

   * new Book(10) 실행

     * Book 오브젝트의 [[Construct]] 호출
     * 파라미터 값을 [[Construct]]로 넘겨준다.

   * [[Construct]]는 빈 Object를 생성

     * 이것이 인스턴스이다. 지금은 빈 오브젝트{ }이며 하나씩 채워나간다.

   * 오브젝트에 내부 처리용 프로퍼티 설정

     * 공통 프로퍼티와 선택적 프로퍼티가 있다.

   * 오브젝트의 [[Class]]에 "Object" 설정

     * 생성한 인스턴스 타입은 Object이다.

   * Book.prototype에 연결된 프로퍼티(메소드)를 생성한 인스턴스의 [[Prototype]]에 설정한다. (Constructor도 같이 설정된다.)

     ``` javascript
     Book 인스턴스:{
         poin:10,
         __proto__ ={
             constructor: Book,  //외부 프로퍼티로 [[Construct]]와는 다르다
            	getPoint: function(){},
             __proto__: Object
         }
     }
     ```

     

####  L33 - constructor 프로퍼티, constructor 비교

---

1. constructor 프로퍼티

   ```javascript
   Book fucntion 오브젝트:{
       prototype:{
           constructor: Book
       }
   }
   ```

   * 생성하는 function 오브젝트를 참조한다.
     * function 오브젝트를 생성할 때 설정한다.
     * prototype에 연결되어 있다.
   * 개인경험
     * constructor가 없더라도 인스턴스가 생성된다. 하지만, 필요하지 않다는 의미는 아니다.
   * ES5: constructor 변경 불가
     * 생성자를 활용할 수 없다.
   * ES6: constructor 변경 가능
     * 활용성이 높다.

2. constructor 비교

   ``` javascript
   var Book = function(){};
   var result = Book === Book.prototype.constructor;
   console.log("1:", result); //1: true
   
   var obj = new Book();
   console.log("2:", Book === obj.constructor); //2: true
   
   console.log("3:", typeof Book); //3: function
   cossole.log("4:", typeof obj); //4: object
   ```

   * Book === Book.prototype.constructor;
     * [실행결과] 1번에 true가 출력된 것은 Book 오브젝트와 Book.prototype.constructor가 타입까지 같다는 뜻
     * Book 오브젝트를 생성할 때 Book.prototype.constructor가 Book 오브젝트를 참조하기 떄문이다.
   * Book === obj.constructor;
     * obj의 constructor가 Book 오브젝트를 참조하므로 [실행결과] 2번에 true가 출력된다.
   * typeof Book;
     * Book 오브젝트의 타입은 function
   * function 오브젝트를 인스턴스로 생성했더니 object로 타입이 변경되었다.
     * 이것은 [[Construct]]가 실행될 때 생성한 오브젝트의 [[Class]]에 'Object'를 설정하기 때문이다.
   * 오브젝트 타입이 바뀐다는 것은 오브젝트 성격과 목적이 바뀐 것을 뜻한다.
     * 다른 관점에서 접근해야 한다.

✨**인스턴스의 가장 큰 특징은 prototype에 있으며 이 prototype에 많은 메소드들이 연결되는점이다.**



#### L34 - prototype / 상속 / prtotype 오브젝트 목적 / 인스턴스 상속

---

1. prototype 오브젝트 목적
   * prtotype 확장
     * prototype에 프로퍼티를 연결하여 prototype 확장
     * Book.prototype.getPoint = function(){}
   * 프로퍼티 공유
     * 생성한 인스턴스에서 원본 prototype의 프로퍼티 공유
     * var obj = new Book(123);
       obj.getPoint();
   * 인스턴스 상속(Inheritance)
     * function 인스턴스를 연결하여 상속
     * Point.prototype = new Book();

2. 인스턴스 상속

   ```javascript
   function Book(title){ // Book 생성자 함수
       this.title = title;
   };
   Book.prototype.getTitle = function(){ //getTitle 함수를 프로토타입에 연결
       return this.title;
   };
   function Point(title){  // Point 생성자 함수
       Book.call(this, title);
   };
   //Book prototype을 Point prototype에 연결시켜준다.
   Point.prototype = Object.create(Book.prototype, {}); 
   var obj = new Point("자바스크립트");
   console.log(obj.getTitle());
   ```

   * 인스턴스 상속 방법

     * prototype에 연결된 프로퍼티로 인스턴스로 생성하여 상속받을 prototype에 연결
     * 그래서, prototype-based 상속이라고도 한다.

   * JS에서 prototype은 상속보다 프로퍼티 연결이 의미가 더 크다.

     * 인스턴스 연결도 프로퍼티 연결의 하나이다.

   * ES5 상속은 OOP의 상속 기능 부족

     * ES6의 Class로 상속 사용

       ```javascript
       class Book{
           constructor(title){
               this.title =title;
           }
           getTitle(){
               return this.title;
           }
       };
       
       class Point extends Book{
           constructor(title){ //constructor 오버라이딩 
               super(title);
           };
       };
       
       const obj = new Point("자바스크립트");
       console.log(obj.getTitle());
       ```



#### L35 - prototype 확장 방법 / 프로퍼티 연결 고려사항 / constructor 연결 / prototype 확장과 인스턴스 형태

---

1. prototype 확장 방법

   * prototype에 프로퍼티를 연결하여 작성
     * prototype.name = value 형태
   * name에 프로퍼티 이름 작성
   * value에 JS 데이터 타입 작성
     * 일반적으로  function을 사용
   * prototype에 null을 설정하면 확장 불가

2. 프로퍼티 연결 고려사항

   * prototype에 연결할 프로퍼티가 많을때 
     * Book.prototype.name1, 2, 3 ~ n 형태는 Book.prototype을 반복해서 작성해야 하므로 번거롭다.
     * Book.prototype={name1:value, ...} 형태로 작성 가능하다.
   * constructor가 지워지는 문제와 대책
     * {name1:value, ...} 형태로 설정한 후 prototype에  constructor를 다시 연결한다.

3. constructor 연결

   ```javascript
   function Book(){};
   Book.prototype = {
       constructor: Book,
       setPoint: function(){}
   };
   var obj = new Book();
   console.log(obj.constructor);
   ```

   * 오브젝트 리터럴{}을 사용하여 프로퍼티를 연결할때는 constructor가 지워지는 것을 고려해야한다.
   * constructor가 없어도 인스턴스가 생성되지만 constructor가 연결된 것이 엊ㅇ상이므로 코드처럼 constructor에 Book function을 할당한다.

4. prototype 확장과 인스턴스 형태

   * prototype을 확장하는 방법과 생성한 인스턴스 형태를 살펴보자.

   ```javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point;
   };
   var obj = new Book(1000);
   obj.getPoint();
   ```

   ```javascript
   obj:{
   	point: 100,
       __proto__ = {
           constructor: Book,
           getPoint: function(){},
           __proto__: Object
       }
   }
   ```

   ---prototype 확장---

   * function Book(point){};
     * Book 오브젝트 생성
   * Book.prototype.getPoint = function(){}
     * Book.prototype에 getPoint 메소드 연결
   * var obj = new Book(100);
     * 인스턴스를 생성하여 obj에 할당
   * obj.getPoint()
     * obj 인스턴스의 getPoint() 호출
   * 인스턴스를 생성하면 prototype에 연결된 메소드를 인스턴스.메소드이름() 형태로 호출한다.



#### L36 - this와 prototype / this로 인스턴스 참조 / this와 prototype / prototype 메소드 직접 호출

---

1. this로 인스턴스 참조

   * this로 메소드를 호출한 인스턴스 참조
     * var obj = new Book();
     * obj.get() 형태에서 this로 obj 참조
   * 인스턴스에서 메소드 호출 방법
     * prototype에 연결된 프로퍼티가 __ proto__ 에 설정되며 인스턴스 프로퍼티가 된다.
     * this.prototype.setPoint() 형태가 아닌 this.setPoint() 형태로 호출된다.

2. this와 prototype

   ```javascript
   function Book(){
       console.log("1:", this.point);  //1: undefined
   };
   Book.prototype.getPoint = function(){
       this.setPoint();
       console.log("2:", this.point);  //2: 100
   };
   Book.prototype.setPoint = function(){
       this.point = 1 00;
   };
   var obj = new Book();
   obj.getPoint();
   ```

   * console.log("1:", this.point);
     * 생성자 함수에서 this는 생성하는 인스턴스 참조
     * 생성하는 인스턴스에 ppoint 프로퍼티가 없더라도 에러가 나지 않고 undefined를 반환 
   * obj.getPoint();
     * this가 메소드를 호출한 인스턴스 참조
     * 즉, 메소드 앞에 작성한 인스턴스 참조 (obj)
   * this.setPoint();
     * this가 인스턴스 참조하며 인스턴스에 있는 setPoint() 호출
   * this.point = 100;
     * this가 인스턴스를 참조
     * 인스턴스의 point 프로퍼티에 100을 할당

3. prototype 메소드 직접 호출

   ```javascript
   function Book(point){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return this.point;
   };
   var obj = new Book(100);
   console.log(obj.getPoint()); //100  (this가 obj를 참조한다.)
   console.log(Book.prototype.getPoint()); //undefined
   ```

   * Book.prototype.getPoint();
     * 인스턴스를 생성하지 않고 직접 메소드 호출
   * Book.prototype을 getPoint()에서 this로 참조
   * obj 인스턴스에는 point가 있지만 Book.prototype에 point가 없으므로 undefined를 반환
     * 인스턴스를 생성하여 메소드를 호출하는 것과 직접 prototype을 작성하여 호출하는 것의 차이

   ✨**인스턴스를 생성하여 메소드를 호출하는것과 직접 prototype을 작성하여 호출하는 것의 차이이다.**

   **물론, Book.prototype을 안쓰는건 아니다. 그러나 바로 메소드를 호출하는게 아닌 call()이나 bind() 등의 메소드를 사용한다.**

   

#### L37 - propertype 프로퍼티 공유 시점

---

1. 프로퍼티 공유 시점

   * 사용하는 시점에 prototype의 프로퍼티를 공유한다
     * 메소드를 호출하는 시점에 프로토타입의 메소드를 공유하는 것이다.
   * prototype의 프로퍼티로 인스턴스를 생성하지만 인스턴스의 프로퍼티는 원본 prototype의 프로퍼티를 참조한다.
     * 복사하여 갖고 있는 개념이 아니다.
   * 인스턴스의 메소드를 호출하면 원본 prototype의 메소드를 호출한다.
   * 원본 prototype에 메소드를 추가하면 생성된 모든 인스턴스에서 추가한 메소드를 사용할 수 있다.
     * 원본 prototype의 메소드를 호출하기 때문이다.

2. 프로퍼티 공유 시점

   ```javascript
   function Book(){
       this.point = 100;
   };
   
   var obj = new Book();
   console.log(obj.getPoint); // undefined
   
   Book.prototype.getPoint = function(){
       return this.point;
   };
   var result = obj.getPoint();
   console.log(result); //100
   ```

   * var obj = new Book();

     * 인스턴스를 생성하여 obj에 할당한다.

   * console.log(obj.getPoint);

     * obj 인스턴스에 getPoint()가 없다.

   * Book.prototype.getPoint = function(){

     ​	return this.point;

     };

     * Book.prototype에 getPoint() 추가
     * 앞에서 생성한 obj 인스턴스에서 getPoint()를 사용할 수 있다.

   * var result = obj.getPoint();

     * 인스턴스를 생성할 때는 obj에 getPoint가 없었지만 getPoint()를 호출하기 전에 Book.prototype에 getPoint를 추가했으므로 호출할 수 있다.

   * retun this.point;

     * 추가하더라도 this가 인스턴스를 참조한다.

   * 이런 특징을 활용하여 상황(필요)에 따라 메소드를 추가, 역동적인 프로그램 개발이 가능하다.



#### L38 - 인스턴스 프로퍼티

---

1. 인스턴스 프로퍼티

   ```javascript
   obj 인스턴스 = {
       point: 100,
       getPoint: function(){},
       __proto__:{
           getPoint: function(){}
       }
   }
   ```

   * prototype에 연결된 프로퍼티도 인스턴스 프로퍼티가 된다.
     * 직접 인스턴스에 연결된 프로퍼티와 차이가 있다.
   * 인스턴스의 프로퍼티를 prototype으로 만든 인스턴스 프로퍼티보다 먼저 사용한다.
   * 인스턴스마다 값을 다르게 가질 수 있다
     * 인스턴스를 사용하는 중요한 목적

2. 인스턴스 프로퍼티 우선 사용

   ```javascript
   function Book(pppoint){
       this.point = point;
   };
   Book.prototype.getPoint = function(){
       return 100;
   };
   var obj = new Book(200);
   obj.getPoint = function(){
       return this.point;
   };
   console.log(obj.getPoint());
   ```

   * Book.prototype.getPoint = function(){ return 100;};
     * prototype에 getPoint를 연결한다.
     * 인스턴스의 getPoint()를 호출하면 100을 반환한다.
   * obj.getPoint = function(){ return this.point;};
     * 생성한 인스턴스에 getPoint를 연결한다.
     * 함수가 호출되면 200을 반환한다.
   * obj 인스턴스 구성 형태
   * obj.getPoint();
     * obj 인스턴스의 getPoint() 호출
     * prototype의 getPoint()가 호출되지 않고 인스턴스의 getPoint()가 호출된다.
     * 인스턴스에ㅔ 연결한 프로퍼티를 먼저 사용하기 때문이다.
   * 인스턴스 프로퍼티는 공유되지 않는다.
   * Class 접근
     * 설계가 중요
     * OOP 개념 이해 필요



### [SECTION7 this]

#### L39 - this 개요 / this와 글로벌 오브젝트 / this와 window 오브젝트

---

1. this 개요

   * 키워드이다.
   * obj.name() 형태로 호출한 함수(메소드)에서 this로 인스턴스(오브젝트)를 참조
   * 실행 콘텍스트의 this 바인딩 컴포넌트에 바인딩된다.

2. this와 글로벌 오브젝트

   * 글로벌 오브젝트에서 this는 글로벌 오브젝트를 참조한다.

   * this와 window 오브젝트

     * window는 JS에서 만든것이 아니며 글로벌 오브젝트의 스코프도 아니다.
     * window와 글로벌 오브젝트를 같은 선상에서 사용한다.

   * Host 오브젝트 개념 적용

   * 글로벌 오브젝트에 코드 작성

     window.onload = function(){  // 여기말고 밖에 작성되는 코드들};

     * this가 window 참조  (글로벌 오브젝트)

     ```javascript
     console.log(this === window);  //true
     ```

     * this가 글로벌 변수 사용

     ```javascript
     var value = 100;
     console.log(this.value); //100
     ```

     * window로 글로벌 변수 사용 (window와 글로벌 오브젝트를 같은 선상)

     ```javascript
     var value=100;
     console.log(window.value); //100
     ```

     * this.value = 100; 형태로 값 할당

     ```javascript
     this.value = 100;
     console.log(window.value); //100
     ```

   * window.onload = function(){//여기에 작성되는 코드들};

     * this가 window 참조
       * window 실행 컨텍스트에서 this 바인딩 컴포넌트에서 this를 참조한다.

     ```javascript
     window.onload = function(){
         console.log(this === window);  //true
     };
     ```

     * this로 로컬(지역) 변수 사용

     ```javascript
     window.onload = function(){
         var value = 100;
         console.log(this.value); //undefined
     }
     ```

     * this.value = 100; 형태로 값 할당

     ```javascript
     window.onload = function(){
         this.value = 100;
         console.log(window.value); //100
     }
     ```

     

#### L40 - this 참조 범위 / this와 strict 모드 / this 참조 오브젝트

---





#### L41 -this와 인스턴스

---



#### L42 - this와 call() 메소드 / this 사용 / Object 사용 / 숫자 작성 / this 참조 변경

---



#### L43 - this와 apply() 메소드 / this와 arguments

---



#### L44 - this와 콜백 함수

---



#### L45 - this와 bind() 메소드 / function 오브젝트 생성,호출 / 파라미터 병합

---



#### L46 - bind() 활용 / 이벤트 처리

---



### [SECTION8 논리적 정리]

#### L47 -

---



#### L48 -

---



#### L49 -

---



#### L50 -

---





