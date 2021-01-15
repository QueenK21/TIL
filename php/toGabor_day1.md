Day 1: 온라인 실험에서 Gabor Patch 만들기 step 1 
===========================================


가보 패치를 자극으로 쓰는 온라인 실험을 만들어야하는데, 할 수 있는 방법은



### 1. 일단 Matlab으로 만들어서 이미지로 저장 후 불러오기   

### **2. 어떻게든 html로 해결하기**




2021년 들어서 어떻게든 Matlab 의존도를 낮추기로 결심했기 때문에 1번 방법은 가능하면 피하고 싶은데, 그런 결심을 떠나서도 Matlab으로 랜덤 노이즈를 넣은 가보 자극을 일일이 이미지로 저장하여 불러오는 것도 너무 비효율적이어서 어떻게든 2번 방법을 시도해보기로 했다 (만들어 놓은 이미지를 저장해서 불러오는 순간 더이상 랜덤이 랜덤이 아니기도...).


먼저 다행인 것은, [Online Gabor-patch generator](https://www.cogsci.nl/gabor-generator)라는 온라인에서 값 입력하면 거기에 맞는 가보 패치를 만들어주는 사이트에서 소스코드를 제공하고있다는 점. 맨땅에 헤딩까지는 아니고 여기서 제공해주는 코드에서 시작해서 하나씩 해결해보기로 했다.   
소스코드 받아서 html 처럼 바로 크롬으로 실행시켜보면, 그냥 코드가 화면에 죽 나열되고 아무 것도 제대로 뜨지 않는다.   
> #### php는 내 로컬 컴퓨터에서 돌아가는 웹서버가 필요하다!       

유투브에서 php는 서버사이드 언어라고 했는데 그게 이뜻이었구 나 이제야 알았다. php는 서버에서 실행되지만, html과 JS는 클라이언트에서 실행된다. 

   

처음 해결한 방법은 유토론토에서 제공하는 온라인 실험을 돌리기 위해 웹서버를 맥에 세팅하는 방법인데, 아파치 웹서버를 이용하는 방법이다. 


__1. 아파치, php 버전 확인__    

```bash
$sudo apachectl -v 
$sudo php -v
```   
맥에는 아파치랑 php가 따라오기 떄문에 버전을 확인하고, 



__2. 아파치 웹서버 실행시키기__      

```bash
$ps -ef |grep httpd
$sudo apachectl start
$ps -ef |grep httpd 
```  


*아파치 웹서버가 돌아가고 있는지 확인   
*아파치 웹서버 실행하기   
*아파치 웹서버가 돌아가고 있는지 확인    


아파치 웹서버가 작동하는지 확인하는 것은 URL 에 http://localhost 를 그냥 쳐보면 된다. It works! 라고 뜨면 돌아가고 정상 작동하는 것.



__3. 아파치 configuration file 수정하기__   


/etc/apache2/httpd.conf 에서 밑에 3개 라인을 uncomment 한다.   
```
#LoadModule userdir_module libexec/apache2/mod_userdir.so
#LoadModule php5_module libexec/apache2/libphp5.so 
#Include /private/etc/apache2/extra/httpd-userdir.conf
```   

/etc/apache2/extra/httpd-userdir.conf 에서 밑에 라인을 uncomment 한다.

```
#Include /private/etc/apache2/users/*.conf
```



__4. 아파치 configuration file 만들기__   


/etc/apache2/users/username.conf 파일을 만든 후, 밑에 라인을 삽입한다.   
```
<Directory "/Users/username/Sites/">
    Options Indexes MultiViews
    AllowOverride None
    Require all granted
</Directory>
```   



__5. home directory에 디렉토리 만들어주기__       
```bash
$mkdir ~/Sites
```  



__6. 아파치 웹서버 재시작__      
```bash
$sudo apachectl restart
```  



__7. 끗__     
여기까지 하고나면 디렉토리 폴더(~/Sites) 안에 테스트 할 파일 넣어주면 된다. 그러고나서 http://localhost/~username/test.html 을 url에 치면 정상적으로 작동...   




해야했지만, imagepng 라인에서 permission denied 발생.   
일단은,   
```bash
$chmod -r 777 directiory이름 
````   
으로 해결했는데, 777은 read, write, execute을 어떤 유저에게든 모두 가능하게하는 거라서 위험하다고...일단 내 개인용 노트북에서 다시 테스트해야하는데 내 컴퓨터는 소중하니까 그거는 위의 방법 말고 다른 방법으로 해결해보려고 한다.    

그리고 php5부터는 웹서버가 따로 필요 없다는데, 노트북에선 웹서버 없이 해 볼 예정. 어찌되었든 이제 겨우 가보 패치 만드는 php 파일이 정상적으로 보이니 내일은 저 가보 패치 만드는 파트만 따오는 법을 연구해야함! 









