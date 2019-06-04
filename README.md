# Github Page using Hugo

> Go를 접해보기 위해 Hugo를 사용했다.  
> https://github.com/Integerous/Integerous.github.io 의 설명과  
> https://gohugo.io/hosting-and-deployment/hosting-on-github 의 설명을 참고했다.  
> 아래는 내가 사용한 방법이다.  

  
  
#### Github Page using Hugo 과정

  
**1. Install Hugo**

- [Giraffe Academy의 영상: Windows에서 Hugo설치하기](https://gohugo.io/getting-started/installing#windows)
- [hugo 공식 깃헙](https://github.com/gohugoio/hugo/releases)에서 운영체제에 맞는 최신버전을 다운로드한다.
- `C:\Hugo\bin` 디렉토리를 생성해서 다운 받은 압축파일을 해제한다.
- 어느 위치에서나 Hugo를 실행할 수 있도록`$ set PATH=%PATH%;C:\Hugo\bin` 명령으로 환경변수에 `C:\Hugo\bin`를 추가한다.
- 명령 프롬프트에 `$ hugo version` 혹은 `$ hugo help`로 동작을 확인한다.

  
**2. Two Type of Github Pages**

- Hugo의 컨텐츠와 소스파일들을 포함할 `<YOUR-PROJECT>` 저장소를 생성한다. (e.g. `Portfolio-Page`)
- 렌더링된 버전의 Hugo 웹사이트를 포함할 `<USERNAME>.github.io` 저장소를 생성한다. (e.g. `LeeSongA.github.io`)

  
**3. Directory Structure 구성**

- `$ hugo new site Portfolio-Page` 명령으로 로컬에서 컨텐츠를 관리하기 위한 장소(e.g. `Hugo/Portfolio-Page`)를 생성한다.
- `C:\Hugo\Portfolio-Page`에서 `$ dir`로 directory structure를 확인할 수 있다.

  
**4. 테마 다운로드 및 설정**

- https://themes.gohugo.io 에서 원하는 테마를 선택한다.
- 원하는 테마를 fork 한다.
- `~\Portfolio-Page` 경로에서 submodule로 테마를 추가한다.

    ``` 
    $ git init
    $ git submodule add https://github.com/LeeSongA/hugo-agency-theme.git themes/hugo-agency-theme 
    ```

- `config.toml` 파일을 선택한 테마의 설명서에 따라 수정한다.
	- Take a look inside the exampleSite folder of this theme. 
	- You’ll find a file called config.toml. 
	- To use it, copy the config.toml in the root folder of your Hugo site. 
	- Feel free to change the strings in this theme.

  
**5. Remote와 Submodule 설정**

- 깃헙에 만든 `Portfolio-Page` 저장소를 local의 Portfolio-Page 디렉토리의 remote로 등록한다.

    ```
    $ cd C:\Hugo\blog
    $ git init
    $ git remote add origin git@github.com:LeeSongA/Portfoil-Page.git
    ```

- `LeeSongA.github.io` 저장소를 Portfolio-Page의 submodule로 등록한다.

    ```
    $ git submodule add -b master git@github.com:LeeSongA/LeeSongA.github.io.git public
    ```

- `hugo` 명령으로 `public`에 웹사이트를 만들 때, 만들어진 `public` 디렉토리는 다른 remote origin을 가질 것이다.


> SSH <br>
> Generating a new SSh key and adding it to the ssh-agent   
> https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent   
> Adding a new SSH key to your Github account   
> https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account

  
**6. 컨텐츠 생성**

- `$ hugo new post/test1.md` 명령으로 파일을 생성한다. 
  C:\Hugo\Portfolio-Page\content\post\test1.md created

- 컨텐츠 확인하려면
  - `$ hugo server` 혹은 `$ hugo server -t <YOURTHEME>`로 웹서버를 실행한다.
  - `http://localhost:1313/`에 접속해서 확인한다.

  
**7. 컨텐츠 업로드 (Portfolio-Page에)**

- Portfolio-Page로 이동한다.
- 테마가 적용된 Portfolio-Page 내용을 public에 생성한다.
- public 디렉토리로 이동하여 수정된 파일들을 index에 올린다.
- 변경 내용을 commit 하고 LeeSongA.github.io에 push한다.

    ```
    $ cd C:/Hugo/Portfolio-Page
    $ hugo -t <YOURTHEME> 
    $ cd public
    $ git add .
    $ git commit -m <commit-message>
    $ git push origin master
    ```

- `Portfolio-Page` 저장소에도 변경 내용을 push 한다.

    ```
    $ cd Portfolio-Page
    $ git add .
    $ git commit -m <commit-message>
    $ git push origin master
    ```

  
**8. 쉘 스크립트로 업로드 자동화하기**

> It automates the whole process with a simple shell script.  
> [Put it Into a Script](https://gohugo.io/hosting-and-deployment/hosting-on-github/#put-it-into-a-script)의 deploy.sh 파일을 활용하여 쉘스크립트 작성했다.


- Hugo/bin 디렉토리에 deploy.sh 파일을 생성한다.

- The following are the contents of the deploy.sh script:

    ```
    #!/bin/bash

    echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

    # Build the project.
    hugo # if using a theme, replace with `hugo -t <YOURTHEME>`

    # Go To Public folder
    cd public
    # Add changes to git.
    git add .

    # Commit changes.
    msg="rebuilding site `date`"
    if [ $# -eq 1 ]
      then msg="$1"
    fi
    git commit -m "$msg"

    # Push source and build repos.
    git push origin master

    # Come Back up to the Project Root
    cd ..
    ```

- 위의 script에서 Build the project 부분을 수정한다. (e.g. `hugo -t hugo-agency-theme`)

- Portfolio-Page 저장소 Commit & Push를 원하면 아래의 내용을 추가한다.

    ```
    # Portfolio-Page 저장소 Commit & Push
    git add .

    msg="rebuilding site `date`"
    if [ $# -eq 1 ]
      then msg="$1"
    fi
    git commit -m "$msg"

    git push origin master
    ```

  
  
  
### Reference
- [Integerous의 Hugo로 github.io 블로그 만들기](https://github.com/Integerous/Integerous.github.io)
- [Hosting on Github](https://gohugo.io/hosting-and-deployment/hosting-on-github)
- [Giraffe Academy의 영상: Windows에서 Hugo설치하기](https://gohugo.io/getting-started/installing#windows)
- [Hugo 공식 깃헙](https://github.com/gohugoio/hugo/releases)
- [Hugo Theme](https://themes.gohugo.io)
- [Generating a new SSh key and adding it to the ssh-agent](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [Adding a new SSH key to your Github account](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account)
