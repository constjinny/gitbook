# Mac 터미널로 Host server변경하기

### 1. 터미널 편집기 열기 <a id="6149"></a>

### 2. hosts 파일 수정 명령어 입력 <a id="3903"></a>

```text
% sudo vi /etc/hosts
or 
% sudo vi /private/etc/hosts
```

### 3. 패스워드 입력 <a id="d27c"></a>

### 4. hosts 파일 수정 <a id="1efa"></a>

```text
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
123.12.1234     daum.net// 전환시 : 아이피 주소 입력, 구분은 tab
// 호스팅 연결되어있으면 이 부분에 해당주소 뜸
~
~
~
~
~
~
~
~
~
~
~
~
"/etc/hosts" 11L, 215C
```

### 5. 설정 재부팅 <a id="e5c8"></a>

```text
dscacheutil -flushcache
```

### 6. 끝 😬 <a id="2e7b"></a>

