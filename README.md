# 반응성(Reactivity)

svelte에서 변수 선언 또는 조건절 등에 $: 를 붙여 주면 반응성 코드가 됩니다.

다음 예제는 버튼을 클릭할 때마다 count가 1씩 증가되고 double이 계산되어 표시됩니다. 

```html
<script>
    let count = 0;
    let double = 0;

    // count가 증가하면 double 값이 변경된다.
    // 반응형은 의존하는 값이 변경될 때마다 다른 스크립트 코드 이후와 구성 요소 마크업이 렌더링 되기 전에 실행됩니다.
    // double은 count에 의존한다.
    $: double = count * 2;

  const incCount = () => {
    count = count + 1;
  };
</script>
<div>
  <button on:click={incCount}>Increase Count</button>
</div>
<div>
  count: {count}
</div>
<div>
  double: {double}
</div>
```

svelte에서는 모든 최상위 문(즉, 블럭이나 함수 내부가 아닌)은 $: JS 레이블 접두사를 사용하여 반응형을 만들 수 있습니다. 반응형은 의존하는 값이 변경될 때마다 다른 스크립트 코드 이후와 구성 요소 마크업이 렌더링 되기 전에 실행됩니다.


## 반응형 블록

블록도 반응형이 됩니다. count에 의존하기 때문에 cont가 변경되면 log를 찍습니다.
```html
<script>
  // 반응형 블록
  $: {
    // count에 의존하기 때문에 cont가 변경되면 log를 찍는다.
    console.log("count: " + count);
  }
</script>
```

## 구문도 반응형으로
count가 변경될 때마다 콘솔에 로그를 찍습니다. 
```html
<script>
  // count가 변경될 때마다 콘솔에 로그를 찍는다.
  $: console.log("the count is " + count);
</script>
```

## if 블럭을 반응형으로 
if 블록에서도 반응형으로 만들 수 있습니다. 
```html
<script>
      // if 블록에서도 반응형으로 만들 수 있다. 
  $: if(count > 10) {
    console.log("count is greter than 10");
  }
</script>
```

## 객체 변경 시 반응형으로 
person이 변경됨면 name이 변경됩니다. 
```html
<script>
  let person = { 
    name : "kim";
  }
  // person이 변경되면 name이 변경된다. 
  $: ( { name } = person);

  const changePerson = () => { 
    person.name = "Park";
  }

</script>
```

## 주의 
Svelte의 반응성은 할당에 의해 유발됩니다. 배열이나 객체를 변경하는 메서드는 자체적으로 업데이트를 트리거하지 않습니다.

push()를 사용하여 배열에 요소를 추가해도 반응하지 않습니다. 

```html
<script>
 let myarr = [];
  $: {
    console.log(myarr);
  }
  const pushArray = () {
    // 배열이나 객체를 변경하는 메서드는 자체적으로 업데이트를 트리거하지 않는다. 
    myarr.push("Park");
  }
```

다음과 같이 할당을 하는 경우에는 반응합니다. 
```html
<script>
  const changeArray = () => {
    console.log("changeArray");
    myarr = [ "Park", "Kim"];  // 할당은 반응형이 된다. 
  }
<script>
```

다음은 전체 코드입니다. 
```html
<script lang="ts">
  
  let count = 0;
  let double = 0;
  let multi = 0;

  // multi 값은 count가 변경되더라도 변화가 없음. 한 번만 실행된다.
  multi = count * 3;

  // count가 증가하면 double 값이 변경된다.
  // 반응형은 의존하는 값이 변경될 때마다 다른 스크립트 코드 이후와 구성 요소 마크업이 렌더링 되기 전에 실행됩니다.
  // double은 count에 의존한다.
  $: double = count * 2;
  const incCount = () => {
    count = count + 1;
  };
  // count가 변경될 때마다 콘솔에 로그를 찍는다.
  $: console.log("the count is " + count);

  // 반응형 블록
  $: {
    // count에 의존하기 때문에 cont가 변경되면 log를 찍는다.
    console.log("count: " + count);
  }
  // if 블록에서도 반응형으로 만들 수 있다. 
  $: if(count > 10) {
    console.log("count is greter than 10");
  }

  let person = { 
    name : "kim";
  }
  // person이 변경되면 name이 변경된다. 
  $: ( { name } = person);

  const changePerson = () => { 
    person.name = "Park";
  }

  let myarr = [];
  $: {
    console.log(myarr);
  }
  const pushArray = () {
    // 배열이나 객체를 변경하는 메서드는 자체적으로 업데이트를 트리거하지 않는다. 
    myarr.push("Park");
  }
  const changeArray = () => {
    console.log("changeArray");
    myarr = [ "Park", "Kim"];  // 할당은 반응형이 된다. 
  }

</script>

<div>
  <button on:click={incCount}>Increase Count</button>
  <button on:click={changePerson}>Change the person</button>
  <button on:click={pushArray}>Push</button>
  <button on:click={changeArray}>Change Array</button>
</div>

<div>
  count: {count}
</div>
<div>
  multi: {multi}
</div>
<div>
  double: {double}
</div>
<div>
  name : {name}
</div>
```



















