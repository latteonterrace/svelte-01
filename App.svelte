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