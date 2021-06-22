# GitHub 계정 충돌? 계정 여러개 사용하기

나는 업무용 컴퓨터로 코딩을 주로 하고, 작업물을 Github 저장소에 올려두는데 업무/개인용 두 계정을 번갈아 사용하다보니 계정 관련 에러가 발생하였다.

알아보니 SSH Key 사용하여 해결이 가능했다.

## **1. user/.ssh 폴더에 SSH Key 생성** <a id="86fc"></a>

```text
% ssh-keygen -t rsa -C “jinny1@email.com” -f “jinny1”
% ssh-keygen -t rsa -C “jinny2@email.com” -f “jinny2”
```

* 생성시 비밀번호 등록은 자유! 개인적으론 설정해놨다가 매번 입력하기 너무 귀찮아서 없애버렸다.

## **2. 생성된 SSH Key 등록** <a id="1c4e"></a>

```text
% ssh-add jinny1
% ssh-add jinny2
```

## **3. SSH config 파일 생성 및 수정** <a id="ab23"></a>

```text
% touch config
% vi config// config
Host github.com
HostName github.com
User jinny1@email.com
IdentityFile ~/.ssh/jinny1Host github.k.com
HostName github.k.com
User jinny2@email.com
IdentityFile ~/.ssh/jinny2// host 수정 시
Host github.com-jinny2
HostName github.com-jinny2
User jinny2@email.com
IdentityFile ~/.ssh/jinny2
```

\(개인GitHub이랑 업무용 GitHub url이 달라 그냥 설정는데, Host 수정이 필요한듯하다.\)

* git clone 시 url 수정

```text
% git clone git@github.com-jinny2:jinny2/project.git
```

* 이미 clone 받은 저장소라면

```text
% git remote set-url origin git@github.com-jinny2:jinny2/project.git
```

## **5. GitHub에 SSH Key 등록** <a id="e0fb"></a>

```text
% cat jinny1.pub
```

![SSH Key](https://miro.medium.com/max/1968/1*VWQH5_5wZU6zKdXsSbhU8g.png)

* ssh-rsa 부터 본인 이메일까지 전부 긁어서 등록해주어야한다.

![GitHub SSH Key Setting](https://miro.medium.com/max/3640/1*hHtVJwh2cl-DNNrh-dBT7A.png)

🔍 **SSH Key**

* 서버 접속 시 비밀번호 대신 Key를 제출하는 방식
* 비밀번호 보다 높은 보안 or 로그인 없이 자동 서버 접속 시 사용

## 📰 참고 <a id="5b8a"></a>

* [https://velog.io/@jay/multiplegithubaccounts](https://velog.io/@jay/multiplegithubaccounts)
* [https://opentutorials.org/module/432/3742](https://opentutorials.org/module/432/3742)

