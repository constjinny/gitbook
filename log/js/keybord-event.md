# keybord event에 대한 분석과 한글 코드

## 😦 사건의 발단

{% hint style="danger" %}
input 값 입력 → `onKeyDown`를 통해 갱신/enter key 감지 → 저장 → input 값 초기화
{% endhint %}

위와 같은 기능을 구현하다가 한글을 입력 후 `onKeyDown`을 통해 enter key를 누르면 입력된 값을 저장 하니 마지막 1문자가 씹혀나오는 현상이 있었다.

일단은 `onKeyPress`로 변경하여 마무리했는, 정확한 문제를 모르니 찝찝하여 한번 다뤄보자고 생각하게 되었고, 예전 프로젝트에서도 조합형 문자 입력시의 문제를 만난 경험.. 그리고 지금 글을 적고있는 gitbook 또한 한글 입력시에만 그가 있어서 한번 다뤄보려고한다.

## 👩🏽‍💻 keybord event

키보드 이벤트란 사용자가 키를 누르거나 놓았을 때 발생하는 이벤트이다.  
이벤트 발생 순서는 다음과 같다.

`keyDown` → `keyPress` → `keyUp`

## ⌨️ keybord event 종류

### keyUp

* 키를 놓았을 때 발생
* 조합형 문자, Backspace/Control/Shift 등 인식 가능
* 입력되는 문자의 갯수가 일치함

### keyDown

* 키를 누를 때 마다 발생
* 조합형 문자, Backspace/Control/Shift 등 인식 가능
* 누를 때 기준으로 1문자 씩 밀려 인지한다.

### keyPress

* 문자가 입력될 때 발생
* 조합형 문자, Backspace/Control/Shift 등은 인식 불가
* 더 이상 새로운 브라우저에서 지원을 하지 않을 예정이라하니 사용을 지양하는게 좋겠다.

## 👾 onKeyDown bug??

`onKeyDown`을 사용하여 input에 문자를 후 enter를 누르면 저장이 되는 기능을 구현하다가 조금 이상한 부분을 발견하였다.  


영문이나 숫자 입력시에는 문제가 없는데 한글을 입력한 후 enter를 누르면,

![](../../.gitbook/assets/_2021-06-17__9.28.25.png)

위 예시 처럼 마지막 1글자가 씹혀서 같이 저장되는 문제였다.  
아래처럼 enter key가 눌렸을 때 이벤트가 2번 실행되면서 문제가 발생했다.

그럼, 비교를 위해 `onKeyDown` 외에 다른 이벤트의 경우도 확인해보자.

### onKeyDown

![](../../.gitbook/assets/_2021-06-18__12.06.36.png)

저장 후 정리되는 것 까지는 괜찮다. 하지만 enter가 2번째 호출되기 전 마지막 1문자가 다시 업데이트 된 것을 확인할 수 있었다. \(저 아이는 어디서 오는걸까...?\)

### onKeyUp

![](../../.gitbook/assets/_2021-06-18__12.07.52.png)

이벤트가 2번 실행되긴한다. 하지만 저장이되면서 input에 입력되었던 값은 깔끔하게 지워졌고, 입력된 값이 없으면 저장하지 않는 로직을 추가해놔서 씹히는 문제는 발생하지 않는다.

### onKeyPress

![](../../.gitbook/assets/_2021-06-18__12.05.40.png)

사실상 `onKeyPress`는 enter를 누르기 전 한글을 입력하는 동안은 이벤트가 실행되지 않았다. 그래서 깔끔하게 1번만 실행되고 마무리 되지만, 이거야말로 다른 기능 구현시 이슈가 발생할 수 있지 않을까 싶었고, 비권장 기능이니 좋은 방법 아니다.

## 🔠 조합형 문자와 완성형 문자

### 조합형과 완성형의 비교

| 구분 | 자판 입력 | 구현 방법 | byte |
| :--- | :--- | :--- | :--- |
| 조합형 | ㄱ + ㅏ + ㅇ = 강 | 자모음\(ㄱ, ㅏ, ㅇ\)을 각각 불러내 최종적으로 '강'을 구현 | 3byte |
| 완성 | 조합형과 동일 | 이미 만들어져 있는 '강'을 불러와 구현함 | 2byte |

### 완성형 문자의 문제

우선 완성형 문자를 사용하게 된 상황을 정리해 보자면 유니코드에 한글코드를 추가해야하는데 그 때 배정받은 자리가 2,350개 였고, 모든 한글을 추가하려면 대략 11,172자리가 필요했다고한다. 부족한 자리에 모든 한글을 채워넣으려니 어쩔 수 없이 완성형태로 정리를 하게된것이였다.

완성형 문자의 문제점을 찾아보면 제일 먼저 보이는 단어는 '[똠방각하](https://namu.wiki/w/%EB%98%A0%EB%B0%A9%EA%B0%81%ED%95%98)'이다. 똠방각하는 옛날 드라마의 제목인데, 유명해진 이유가 완성형 한글 코드에 '똠'자가 없었어서 전산처리시 '또ㅁ', '돔' 등으로 대체하여 쓰여졌다고한다. 이처럼 많은 사람들이 예전에 컴퓨터에서 걁, 겱, 굙, 뾹 등의 단어를 입력시 오류가 뜨는것을 확인하였을 것이다.

### 유니코드 한글 코드 배정

![](../../.gitbook/assets/_2021-06-24__8.49.15.png)

기존의 한글코드는 많은 문제점이 존재했고, 1996년 유니완성형+유니조합형의 형태로 재정이되었다고한다.

<table>
  <thead>
    <tr>
      <th style="text-align:center">&#xAD6C;&#xBD84;</th>
      <th style="text-align:left">&#xD2B9;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">&#xC720;&#xB2C8;
        <br />&#xC644;&#xC131;&#xD615;</td>
      <td style="text-align:left">
        <ul>
          <li>11,172&#xAC1C;&#xC758; &#xBB38;&#xC790;&#xB97C; &#xBBF8;&#xB9AC; &#xC870;&#xD569;&#xD55C;
            &#xD615;&#xD0DC;</li>
          <li>&#xC774;&#xC804;&#xC758; &#xC644;&#xC131;&#xD615; &#xD55C;&#xAE00; &#xCF54;&#xB4DC;&#xC640;&#xB294;
            &#xB2E4;&#xB974;&#xAC8C; &#xC77C;&#xC815;&#xD55C; &#xC870;&#xD569; &#xC6D0;&#xB9AC;&#xB97C;
            &#xAC00;&#xC9C0;&#xACE0; &#xC788;&#xC5B4; &#xC27D;&#xAC8C; &#xC790;&#xC18C;
            &#xC815;&#xBCF4;&#xB97C; &#xCD94;&#xCD9C;&#xD574;&#xB0BC; &#xC218; &#xC788;&#xB2E4;.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>&#xC720;&#xB2C8;
          <br />&#xC870;&#xD569;&#xD615;</p>
        <p>(N&#xBC14;&#xC774;&#xD2B8;)</p>
      </td>
      <td style="text-align:left">
        <ul>
          <li>&#xCD08;&#xC131; 90&#xC790;, &#xC911;&#xC131; 66&#xC790;, &#xC885;&#xC131;
            82&#xC790;, &#xCC44;&#xC6C0;2&#xC790; &#xB4F1; &#xC790;&#xBAA8;&#xB85C;
            &#xAD6C;&#xC131;&#xB41C; &#xD615;&#xD0DC;</li>
          <li>&#xCD08;&#xC131;, &#xC911;&#xC131;, &#xC885;&#xC131; &#xAC01;&#xAC01;
            5&#xBE44;&#xD2B8; &#xC529; 2&#xBC14;&#xC774;&#xD2B8;&#xB97C; &#xC720;&#xC9C0;
            <br
            />&#x2192; &#xAE00;&#xC790; &#xC218; &#xB9CC;&#xD07C; &#xD06C;&#xAE30;&#xB97C;
            2&#xBC14;&#xC774;&#xD2B8;&#xC529; &#xCC28;&#xC9C0;
            <br />&#x2192; &#xAE38;&#xC774; &#xAC00;&#xBCC0;&#xC801;</li>
          <li>&#xBD80;&#xC5EC;&#xB41C; &#xC21C;&#xC11C;&#xAC00; &#xB4A4;&#xC11E;&#xC5EC;&#xC788;&#xC5B4;
            &#xC81C;&#xB300;&#xB85C; &#xC815;&#xB82C;&#xC774; &#xC548;&#xB41C;&#xB2E4;.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

유니완성형과 유니조합형은 서로 변환이 수월하다고 한다.

{% hint style="success" %}
N바이트의 한글 코드

나 = ㄴ + ㅏ = 2byte  
괆 = ㄱ + ㅗ + ㅏ + ㄹ + ㅁ = 5byte
{% endhint %}

## 💊 이렇게 해결해보자.

code sandbox : [https://codesandbox.io/s/key-event-test-8sbz4](https://codesandbox.io/s/key-event-test-8sbz4)

### A. 간단하게 onKeyUp을 사용하자.

`onKeyUp`도 이벤트가 2번 실행되는건 동일하다.  
의도치않은 빈값이 들어갈 수 있기 때문에 주석 step3처럼 확인해주는 과정을 거쳐야한다.

```javascript
// step1. 키 이벤트가 발생
const handleKeyup = (event) => {
	// step2-a. 입력된 key가 enter인지 확인한다.
	if (event.code === "Enter" && event.key === "Enter") {
		// step3. value가 있는 상태인지 확인한다.
    if (value) {
			// step4. value를 저장하고, 초기화시킨다.
	    setSaveds([...saveds, value]);
	    setValue("");
	  }
	}

	// step2-b. enter가 아니면 이벤트를 종료한다.
	return false;
};
```

* 해결은 된다. 불필요한 이벤트 발생 부분이 뭔가 찝찝...

### B. throttle를 사용해보자.

`throttle`는 일정 시간안에 중복이벤트가 발생해도 1번만 처리되게끔 해주는 방식이다.

```javascript
let throttling = null;

const handleKeyup = (event) => {
	if (event.code === "Enter" && event.key === "Enter") {
		if (!throttling) {
			setTimeout(() => {
		    if (value) {
			    setSaveds([...saveds, value]);
			    setValue("");
			  }
      }, 100);
		}
		throttling = true;
	}

	return false;
};
```

* lodash를 활용하면 더욱 편리하게 사용이 가능하다.
* 자매품 debounce도 있다. 다음글에 다뤄봐야겠다.

## 마무리

음...결론적으로 현재로서는 한글처리를 완벽하게 하기 어렵다. 한국인들이 싫어하는 결말이다.  
1byte로 규칙적인 영어, 숫자 처럼 안정적이지 않기 때문에 아마 앞으로도 기능을 구현하면서 다양한 버그들을 만날 가능성이 클 것 같다.

어쩔 수 없이 한글에 대한 버그를 예의주시하며 개발해야할 것 같다.

## 참고

* [https://xetown.com/tips/11368](https://xetown.com/tips/11368)
* [https://tapito.tistory.com/529](https://tapito.tistory.com/529)
* [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=lovefavaz&logNo=140124605492](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=lovefavaz&logNo=140124605492)

