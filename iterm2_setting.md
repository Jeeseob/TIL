## iterm2 셋팅방법

기본 셋팅

여기 블로그 참고   
https://medium.com/harrythegreat/oh-my-zsh-iterm2%EB%A1%9C-%ED%84%B0%EB%AF%B8%EB%84%90%EC%9D%84-%EB%8D%94-%EA%B0%95%EB%A0%A5%ED%95%98%EA%B2%8C-a105f2c01bec

### code 명령 사용

vs code에서 cmd + shift + p를 눌러
검색창을 열고 
거기에 shell 을 검색하면, 
code 명령어를 기본으로 설정 할 수 있다.


### zsh-syntax-highlighting
> git clone https://github.com/zsh-users/zsh-syntax-highlighting.git    
> ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

### zsh-autosuggestions
> git clone https://github.com/zsh-users/zsh-autosuggestions    
> ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
 
 <br>
 
두개 깃에서 clone하여, 해당 위치에 저장한 후,   
.zshrc에서 플러그인 추가

> code ~/.zshrc
> vi ~/.zshrc 등으로 들어가면 된다.
```
plugins=(
  git
  zsh-syntax-highlighting
  zsh-autosuggestions
)
```

이부분 수정 (git은 기본설정)

