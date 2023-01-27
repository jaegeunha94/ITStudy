# [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
파이썬 패키지/가상환경 관리 툴로  
패키지들의 의존성을 관리하기 쉽게 해준다.

아나콘다는 자주 쓰이는 과학/계산 패키지가 많이 포함되어 있어  
용량도 크고 상당히 무겁다.

미니콘다는 아나콘다에서  
그런 pre-install된 패키지들을 제외한 경량화 버전이다.  
(명령어 한 줄로 다시 설치 가능)

가상환경 관리도 되니 venv같은것도 안써도 된다.

 
> **가상환경**  
> 프로젝트별로 파이썬 버전(인터프리터 버전, 패키지 버전 등)을 관리하기 위한 프로젝트 환경  
> (쉽게 말해 가상환경마다 환경변수를 다르게 잡아줘 서로 다른 파일이 로드되게 함)


## Miniconda 자주 쓰는 명령어
명령어 | 설명
---|---
conda list | 현재 환경에 설치된 패키지 목록 보기
conda env list | 현재 존재하는 환경들 보기
conda info | conda 정보 보기
conda install [패키지] | 패키지 설치하기
conda create -n [환경 이름] python=[파이썬 버전] | 새로운 환경 만들기
conda remove -n [환경 이름] --all | 환경 삭제하기
conda activate [환경 이름] | 환경에 들어가기
conda deactivate [환경 이름] | 환경에서 나오기
conda update -n base -c defaults conda | conda 자체 업데이트
conda update --all | 현재 환경의 모든 패키지 업데이트


# Miniconda 설치
## Miniconda 설치 옵션
```
Installs Miniconda3 py39_4.11.0

-b           run install in batch mode (without manual intervention),
             it is expected the license terms are agreed upon
-f           no error if install prefix already exists
-h           print this help message and exit
-p PREFIX    install prefix, defaults to $PREFIX, must not contain spaces.
-s           skip running pre/post-link/install scripts
-u           update an existing installation
-t           run package tests after installation (may install conda-build)
```

## 수동으로 설치하였을 경우 설치 과정 메시지
1. Do you accept the license terms? [yes|no] 
2. Miniconda 설치 경로 
3. Do you wish the installer to initialize Miniconda3 by running conda init? [yes|no] 
  * yes면, .bashrc 파일에 환경 변수 기록

##  환경변수 등록이 안되었을 경우
1. Batch 옵션(-b) 으로 설치하였을 경우 혹은  
2. 설치 과정 메시지에서 'conda init'을 'no'로 선택 했을 경우 

콘다의 설치 경로가 환경 변수에 들어가 있지 않는다.

이 경우에 .bashrc 파일에 설치 경로를 추가해준다.    
`export PATH=$HOME/miniconda3/bin:$PATH`
 
 
만약 쉘 시작시 콘다의 기본 환경을 비활성화 하려면  
아래의 명령어를 입력한다.  
`conda config --set auto_activate_base false`


# 참조
[gwangmin1 tistory](https://gwangmin1.tistory.com/38)
