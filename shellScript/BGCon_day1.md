Day 1: Background Connectivity 분석하기 
===========================================

[뉴로파이프](http://ntblab.github.io/neuropipe/)를 이용해서 fMRI 데이터 분석하는데, 이전에 FFA (fusiform face area) 찾을 때는 shell script를 피상적으로만 이해하고 그냥 분석하기 급급했기 때문에, 이번엔 shell script들 하나 하나 분석하면서 공부한 것을 기록하려고 한다.  
  
__-Loop문__  
shell script는 구문이 꽤 귀엽다.   
```shell
for i in $Sequence
do
    #your code
done 
```
in 안에 들어갈 수 있는 것은 생각보다 엄청 다양하고, do - done 표현은 while 문에도 똑같이 적용된다.  

__-Expansion__   
shell script보다보면 " " 와 ' ' 가 자주 쓰이는데, 두 개의 차이는 double quote는 expansion이 가능하고 single quote은 expansion이 불가능하다는 것.   
아래는 [두 개의 차이에 대한 글](https://www.howtogeek.com/howto/29980/whats-the-difference-between-single-and-double-quotes-in-the-bash-shell/)에서 가져온 내용이다.   
```shell
mkdir "TestDirectory"
mkdir 'TestDirectory'
```   
위의 경우에는 똑같고 아무 차이가 없다. 그러나 아래와 같은 경우를 생각해보자.   
```shell
dir = "TestDirectory"
echo $dir
echo "$dir"
echo '$dir'
```
이 경우, 위의 두 개의 아웃풋은 TestDirectory이지만, 세번째의 경우에는 $dir 가 아웃풋이 된다.   
double quote와 비슷한 기능을 하는게 또 자주 쓰이는 **$** 다. 
```shell
me = 2
echo "me"
echo "$me"
```   
이 경우, 첫번째는 me 를 반환하고 두번째는 2를 반환한다...까지가 스택오버플로우에 나와있는 내용인데, ""도 확장인데 왜 me가 반영될까..? 이건 조금 더 알아보고 추후 추가하자.  

__-If directory exists~__   
```shell
if [ -d "$output_dir" ]; then
  while true; do
    read -p "data has already been converted. overwrite? (y/n) " yn
    case $yn in
      [Yy]* ) rm -rf "$output_dir" ; mkdir -p "$output_dir"; break;;
      [Nn]* ) exit;; 
    esac
  done
elif [ ! -d "$output_dir" ]; then
  mkdir "$output_dir"
fi
```   
디렉토리가 이미 있으면 덮어 씌울지 말지를 물어보고, 아니면 바로 폴더를 만드는 코드다. directory가 물을 경우에는 경로 앞에 -d 를 붙이면 되고, 파일이 있는지 여부를 체크할 경우에는 -f 를 앞에 붙이면 된다. mkdir 에서 p 옵션은 만약 뒤에 적은 디렉토리 명에 중간 폴더가 포함되어 해당 폴더가 존재하지 않을 경우, 최종 만들려는 하위 폴더가 생성되지 않는 오류가 발생하는데 그렇게 존재하지 않을 경우 해당 폴더를 자동 생성해주는 안전한 옵션이다. 셸 스크립트가 귀엽다는 걸 알수있는 case - esac 과 if - fi.  

__-error:Argument list too long__   
쉘에 리밋이 있다는 것을 몰랐다. 엄청나게 많은 dicom 파일을 개별 참가자의 data 파일로 옮겨오기 위해,   
```shell
cp $(find $SourceDirectory -type f -name "*myname*") $DestinationDirectory
```   
를 작성했더니 argument가 너무 길다는 위의 에러가 떴다. (참고로 위 코드에서 -type f 는 source directory에서 파일을 찾겠다는 의미고, -name 은 뒤의 spring이 들어간 파일을 다 찾겠다는 의미다.)  
해결 방법은,   
```shell
find $SourceDirectory -type f -name "*myname*" -exec cp {} \;
```   
{}가 find 의 결과물을 받는 것이다. 이렇게 하면 잘 옮겨지는데, 이유는 잘 모르겠다. difference between cp and exec cp 로 검색해도 설명이 될 법한 글을 찾지 못했다.  

__-dcm2niix__   
shell script 와는 상관이 없지만, dicom 파일에서 nifti 파일로 변환하는 것을 카탈리나에서도 가능하게 해주는 파일이고 fMRI 분석을 위해선 앞으로도 계속 쓰일 아이라 까먹지 않도록 적어둔다. dcm2niix 실행할 때 -f 와 같은 옵션은 shell script 에서의 옵션과 전혀 상관 없으며, dcm2niix 도큐먼트에 들어가면 어떻게 실행하는지, 각 옵션은 무엇을 의미하는지 상세히 나와있다. [dcm2niix document](https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage)