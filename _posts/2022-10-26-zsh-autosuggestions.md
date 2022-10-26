---
published: true
title:  "[zsh] zsh 자동완성 플러그인 추가하기"
categories: Etc
tag: [zsh, autosuggestions]
---

우분투 20.04 기준

## 플러그인 설치
> git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

위의 코드를 복사해 터미널에서 실행한다.  
그러면 알아서 zsh 플러그인 폴더에 설치가 된다.  

## 플러그인 적용

텍스트 에디터로 .zshrc를 열어준다.  
> gedit ~/.zshrc

검색기능을 이용해 plugins를 찾아서
```
plugins=(
    #다른 플러그인들... 
    zsh-autosuggestions
    )
```
이부분에 위와같이 zsh-autosuggestions 넣어주고 저장.  
터미널을 다시 실행해보면 적용된다!



### 출처

https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md