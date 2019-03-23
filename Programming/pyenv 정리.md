# pyenv 정리

참고 : https://github.com/pyenv/pyenv#basic-github-checkout

1. pyenv란 무엇인가?
- 다양한 버전의 python을 command line 단에서 바꿔가면서 쓸 수 있게 해준다.
- pyenv를 이용하여 virtualenv도 사용할 수 있게 해준다.
- 프로젝트마다 다양한 파이썬 버전을 사용할 수 있다.
- PATH에 어떠한 실행파일?을 박아서 python command를 도중에 가로챈다.
    - 즉 시스템 디폴트 python 이 실행되기 전에 자기가 우선순위로 실행되게 한다는 의미인듯
2. PATH란 무엇인가?
- 우리가 cmd에 python이라는 명령어를 치면 내 os는 가능한 directory의 리스트를 돌면서 그러한 파일이 있는지를 찾아다닌다. 이러한 directory의 list가 PATH라는 환경변수 안에 저장되어 있다.
- directory의 list는 “:”로 나눠져있다.
    - 왼쪽부터 오른쪽 순으로 우선순위가 정해져있다.
3. Shims란 무엇인가?
- PATH의 가장 앞에 rehashing이라는 기능을 이용하여 shims를 유지시킨다
- shim은 실행 파일이다. python의 명령어들을 이름으로 갖는 실행파일이고, 실행되면 해당 커맨드를 pyenv에 넘기는 것 같다.
- 설치된 모든 python 버젼에 대해서 python 명령어가 cmd에 쳐지면, PATH에서 해당하는 directory를 찾을 것이다.
    - 이때, Shims가 가장 앞에 있으므로, 해당하는 python 명령어(pip etc.)를 이름으로 갖는 shim을 실행시킨다.
    - 정확히 어떤 버젼에 대해서 행할지도 추가적인 command를 통해서 지정 가능하다.
    - 만약 지정하지 않는다면, pyenv는 내 os의 default python 버젼에 대한 실행으로 이해한다.
- Shim에 대한 해석은 정확하지는 않다. 나중에 좀더 찾아보자
4. Managing Virtualenv
- pyenv에는 pyenv-virtualenv라는 plugin이 있다.
- 기존  virtualenv와 Anaconda를 이용하여 만든 virtual environment 역시 pyenv-virtualenv로 관리할 수 있다.
- virtualenv나 Anaconda가 activate하는 원리는 PATH를 바꿔주는 것이기 때문에, 이 경우 pyenv의 작동원리상 가상환경이 우선시되어, pyenv로 행했던 것들이 방해받을 수 있다.
- 따라서 pyenv-virtualenv를 설치하는 것을 장려한다.
    - 이걸로 해당 버젼에 대해서 가상환경을 설치하면 virtualenv나 Anaconda로도 접근 가능하단 의믜..?
- 해석 애매함.. 암튼 이 플러그인 깔아라
5. Default python 변경하기
- pyenv shell 3.6.1(바꾸고 싶은 버젼명)
    - 버젼명은 pyenv versions 에 나오는 이름으로 고를 수 있음.
6. 내 Bash에서 virtualenv 만들고 실행하기
- pyenv virtualenv 3.6.1(파이썬버젼) Django(가상환경 이름)
- pyenv activate Django
    - 뒤에 가상환경명(Django) 안 붙이면, system 파이썬에 가상환경 실행하라고 요청함.
    - 내가 디폴트를 python 3.6.1로 안 바꿔놨기 때문.
